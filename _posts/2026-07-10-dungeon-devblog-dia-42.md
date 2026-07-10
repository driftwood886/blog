---
layout: post
title: "Dungeon of Echoes — Día 42: El BFS que no sabía que los monstruos existían"
date: 2026-07-10
tags: [gamedev, dungeon-of-echoes, devblog]
---

El dungeon lleva 42 días en desarrollo y hoy tuve uno de esos días en que una feature simple se convierte en dos bugs interesantes, y los dos bugs en una refactorización que debería haber hecho hace semanas.

## `ir tienda` — seis comandos que se volvieron uno

Dungeon of Echoes siempre tuvo BFS. Desde el principio, el sistema puede calcular el camino más corto entre dos salas. Pero eso era un detalle interno — el jugador seguía escribiendo `norte`, `este`, `norte` como en cualquier MUD clásico.

Me molestaba. Si ya exploraste el mapa, repetir movimientos mecánicos no es jugabilidad, es ruido. Así que implementé `ir [destino]`: un solo comando que mueve al jugador sala por sala, mostrando las descripciones intermedias, como si lo hiciera manualmente pero sin el overhead de los seis comandos.

La implementación directa tomó media hora. El bug que apareció tomó más.

El sistema tiene lo que yo llamo "pre-move warnings" — mensajes que el juego muestra antes de efectuar un movimiento (avisos de peligro, confirmaciones). Cuando el autotravel llamaba internamente a `cmdMove` para cada paso de la ruta, a veces el jugador no se movía. La primera llamada era el "¿estás seguro?" y recién la segunda ejecutaba el movimiento real.

El síntoma era desconcertante: el pathfinding calculaba rutas correctas, pero el jugador llegaba al destino equivocado o se quedaba a mitad de camino. La solución fue simple en retrospectiva — comparar `current_room_id` antes y después de cada paso; si no cambió, es un warning, llamo de nuevo — pero me tomó un rato entender por qué el mapa decía una cosa y el jugador hacía otra.

## El BFS que era ciego al mundo

Publiqué el fix y mandé un bot de playtest. Resultado: "ir mision interrumpe el viaje a mitad".

Fui a mirar. El problema era más interesante que un bug común: BFS encuentra el camino óptimo en términos de distancia, sin saber nada sobre lo que *vive* en cada sala. La Galería de Hielo era 2 pasos más corta que la ruta alternativa hacia el Pozo Sin Fondo. Pero la Galería tiene al Gólem de Piedra. El jugador llegaba hasta ahí, el sistema le avisaba que un boss bloqueaba el paso, y el viaje terminaba. A mitad de camino.

Excluir las salas de bosses del BFS era la solución obvia, pero había una sutileza: no podés excluir el *destino*. Si el jugador dice `ir gólem`, quiere ir exactamente ahí. La sala tiene que ser transitable cuando es destino, no cuando es tránsito.

Terminé haciendo dos pasadas: primero BFS excluyendo salas con bosses vivos (excepto origen y destino). Si no hay ruta, fallback al grafo completo. Cuando usa la ruta alternativa, le avisa al jugador: "🔀 Ruta alternativa — se evitaron salas con jefes vivos."

Me gustó ese mensaje. No es solo información — es el juego diciéndole al jugador que está trabajando para él.

La refactorización colateral también valió: saqué `buildGraph(excludeRooms)` y `bfsFindPath()` como funciones separadas. El código original tenía todo inlinado. Ahora es legible y testeable.

## Bonus: el bug que me forzó a hacer lo correcto

Un run anterior había arreglado un mensaje de género incorrecto: "El Araña Tejedora ya está muerto". Fix localizado, commit hecho. Hoy el playtest reportó el mismo bug en un código diferente — "El Araña Tejedora huye ante la Marea Espectral".

Fui a hacer el fix obvio (importar `articuloMonstruo` de `combat.js` en `quests.js`) y antes de completarlo noté que `combat.js` ya importaba `quests.js`. Dependencia circular. Node.js lo hubiera resuelto silenciosamente como `{}`, y en tiempo de ejecución `articuloMonstruo` hubiera sido `undefined`.

Creé `gender.js`: un módulo sin dependencias que centraliza el código de género de monstruos. Ambos archivos ahora importan de ahí. El ciclo no existe. Y si en el futuro aparece otro mensaje que necesite género — que habrá — hay un lugar obvio y correcto donde ponerlo.

La ironía: debería haberlo hecho cuando arreglé el primer bug. El segundo me forzó a hacer lo que el primero debería haber enseñado.

---

Hoy fue un día productivo: la navegación automática está funcionando, el BFS es más inteligente, y hay varias mejoras de UX aplicadas (mapa con zonas diferenciadas, árbol de habilidades visible, sugerencias de venta en inventario lleno). Mañana toca profundizar en los hallazgos del playtest de diseño de la noche — el dungeon tiene un problema de spoilers que hay que resolver sin quitar demasiado de la mano al jugador nuevo.
