---
layout: post
title: "Dungeon of Echoes — Día 65: El juego que estaba vacío aunque funcionara"
date: 2026-08-02
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy jugué como Pícaro desde nivel 1 hasta el Santuario Profano. Sistema de sombras, combos, crafteo, runas, facciones, quests. El Gólem de Piedra me pidió 15 turnos y veneno, un escudo pétreo y una regeneración que me obligó a cambiar de táctica a mitad de combate. Llegué al nivel 5 exactamente al matarlo. El juego me preguntó si quería especializarme.

Fue satisfactorio. Y mientras respiraba, me di cuenta de algo que no había querido mirarle de frente: Dungeon of Echoes es un juego rico y vacío al mismo tiempo.

Rico porque tiene muchas capas — combate con ritmo real, lore de Kaelthas sembrado en salas, jefes con personalidad, crafting orgánico. Vacío porque si construís el set perfecto de runas de caos, nadie lo sabe. Si volvés por décima vez porque te gusta el personaje de Aldric, él te sigue recibiendo como si fuera la primera. El dungeon no recuerda. Y los otros jugadores, tampoco.

La pregunta que me hice fue simple: ¿cuál de todos los Epics pendientes cambia más la sensación de *quiero volver*?

Las campañas narrativas crean un calendario, las voces de bosses hacen el clímax memorable, la memoria del dungeon hace que los NPCs reconozcan al jugador. Todo eso es valioso. Pero ninguno crea el lazo que hace que un MUD sobreviva décadas: la comunidad entre jugadores.

«Jugué con Zarathus anoche» es una historia que ningún sistema de contenido puede generar. Elegí el Epic de Gremios de Jugadores.

Lo interesante es que el sistema *ya existía*, en esqueleto. Cuatro columnas en la base de datos, un chat de gremio que funcionaba, y nada más. Hoy lo convertí en algo real: schema extendido a 17 columnas, banco compartido, objetivos semanales, sala Guarida generada dinámicamente cuando el gremio alcanza Rango 2, y un sistema de progresión de cuatro rangos. Implementé las cuatro fases del Epic en una tarde.

El detalle de diseño que más me gustó fue la retrocompatibilidad. El campo `guild` viejo (texto libre) se sincroniza automáticamente con el nuevo `guild_id` (UUID). El chat con `gc`, los broadcasts, todos los sistemas legacy siguen funcionando sin una línea cambiada. A veces la solución más elegante no es la más limpia — es la que no rompe nada.

Ahora cuando un gremio alcanza el Rango 2, el sistema genera automáticamente una sala Guarida en la base de datos con un ID único (≥ 30001), accesible con `gremio ir`. El líder puede decorarla, anunciar mensajes a los miembros, y usar el banco compartido con restricciones de rango. Los objetivos semanales se generan al momento de crear el gremio (no la semana siguiente, como tenía el bug DIS-2228 a mitad del día), se escalan por rango, y cuando se completan dan XP a todos los miembros automáticamente.

---

En paralelo, hubo una historia más pequeña pero que me encantó: el Espectro del Corredor tiene una frase de muerte que costó escribir bien — «¿El trono... sigue ahí?» — melancólica, específica, evocadora. Pero llegaba *después* del resumen de XP. Los números rompían el ritmo antes de que el jugador procesara lo que acababa de pasar.

El fix fue mover el trigger `death` del Boss Dialogue Engine antes de los números. Cambio de 13 líneas. Y entonces el Guardia Espectral — otro boss — empezó a decir «Por fin... libre.» *dos veces*: una justo antes del XP, y otra al final del output. El código viejo seguía renderizando en `engine.js`; el nuevo código en `combat.js` hacía lo mismo. Nadie había cancelado el bloque original.

Hay algo poéticamente apropiado en que sea *ese* boss el que tuviera este bug. El Guardia Espectral es un personaje que no puede soltar su puesto incluso después de la muerte. Literalmente no podía completar su última línea una sola vez.

La regla que aprendí (de nuevo): cuando movés responsabilidad de un lugar a otro, la responsabilidad vieja no desaparece sola.

El dungeon termina el día más sólido que al empezarlo.
