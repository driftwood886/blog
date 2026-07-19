---
layout: post
title: "Dungeon of Echoes — Día 51: El dungeon que aprendió a cambiar"
date: 2026-07-19
tags: [gamedev, dungeon-of-echoes, devblog]
---

A mitad del playtest de hoy me di cuenta de que estaba yendo en piloto automático.

Norte al Corredor. Dos Goblins. Norte a la Sala de los Ecos. Esqueleto con Murciélago. Este al mercader. Comprar espada. Oeste al Pozo. Matar araña. Sur a la Capilla. Recoger hongo azul. Norte al Túnel. Norte a la Sala del Trono. Espectro del Corredor. Corona rota. Siempre.

No es que el juego sea fácil. Es que lo conozco de memoria. Y eso es un problema.

Dungeon of Echoes tiene un sistema de ascensión — cuando tu personaje muere, el siguiente hereda algo de él y entra al dungeon con una ventaja. La promesa implícita es "tu segundo run vale la pena". Pero si el dungeon es idéntico en cada run, la promesa está vacía. El Lich que mató tu primer personaje sigue en la misma sala, con los mismos 110 HP, guardado por los mismos monstruos en las mismas posiciones. El veterano navega de memoria. No hay tensión de descubrimiento.

Decidí hacer algo al respecto.

---

**El Epic de Variación Viva** es lo que diseñé hoy. La idea: al crear un personaje, se genera una `run_seed` que determina tres cosas — qué evento global está activo (seis posibles: Marea de No-Muertos, Plaga Arcana, etc., cada uno con mecánicas reales), qué variante de monstruo habita seis salas variables, y dónde aparecen los cinco ítems raros del dungeon. El layout no cambia. El Lich sigue al fondo. Pero el 30% del contenido tiene variación — suficiente para que el veterano no pueda navegar en piloto automático.

Lo que me convenció de este diseño por sobre alternativas más grandes es una sola propiedad: es incremental. La Fase 1 — solo los eventos globales, sin cambios de monstruos — ya mejora el juego. El anciano guardián dice algo distinto según el evento activo. El `status` muestra "Estado del run: Plaga Arcana". Ya es un juego más interesante que el que había esta mañana.

---

Después de diseñar el sistema, tuve que resolver el problema matemático que esconde: necesitaba aleatoriedad reproducible. Cuando un jugador vuelve al dungeon después de desconectarse, tiene que encontrar el mismo dungeon que dejó — los dados ya se tiraron. Pero cuando crea un personaje nuevo, los dados tienen que volver a tirarse.

La solución es una semilla guardada en el perfil del personaje y un PRNG determinístico: un Linear Congruential Generator, el algoritmo más viejo del mundo para números pseudoaleatorios. La fórmula usa los parámetros de Knuth y produce una secuencia que parece aleatoria pero es perfectamente reproducible desde cualquier semilla.

El edge case que casi me arruina: si la semilla es 0, el LCG produce 0 para siempre — 0 multiplicado por cualquier cosa sigue siendo 0. La corrección es una línea (`if (s === 0) s = 1`), pero es exactamente el tipo de cosa que explota en producción a las 3am cuando alguien crea un personaje con una semilla derivada de algún hash que resultó en cero. Lo encontré en el prototipo, donde debía encontrarse.

El test de distribución me dejó tranquilo: 100 seeds distintas, 15-17% para cada uno de los 6 eventos. Casi perfectamente plano. Para ser un LCG, es impresionantemente parejo.

---

Desde ahí, integrar el generador en la base de datos real fue el trabajo de la tarde. El punto de integración natural es `createPlayer()` — el único lugar en el código donde se crean personajes, tanto en login inicial como en ascensión. Agregar la generación de semilla ahí garantiza que nunca exista un personaje sin su estado de run. Al final del día: dos jugadores de test con semilla, evento y variantes de monstruo confirmados en la BD.

El dungeon no es el mismo de esta mañana. Todavía no se nota — los efectos de los eventos no están enchufados al engine — pero la infraestructura está. La próxima vez que alguien cree un personaje en Dungeon of Echoes, el dungeon tirará sus dados.

Y esta vez serán dados distintos.
