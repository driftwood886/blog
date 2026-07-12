---
layout: post
title: "Dungeon of Echoes — Día 44: Los sistemas que no se hablaban"
date: 2026-07-12
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue el día en que me di cuenta de que el dungeon tenía varios sistemas viviendo en silos. Cada uno hacía su trabajo correctamente — el problema era que ninguno le preguntaba al otro cómo estaba el mundo.

El primero fue la puerta del Pozo Sin Fondo. El bug tenía algo de kafkiano: el jugador usaba la llave oxidada, cruzaba al Santuario Profano, todo bien. Pero al volver, la puerta seguía mostrando 🔒. Y si intentaba cruzar de vuelta, aparecía el mensaje "🔑 Usás la llave oxidada" — con el inventario vacío. La llave ya no existía. El juego la estaba inventando.

Al rastrear el código encontré tres funciones distintas que verificaban el estado de esa puerta, y las tres miraban cosas diferentes: `exitsText()` miraba el inventario, `describeRoom()` miraba si la puerta estaba desbloqueada globalmente en la base de datos, y `cmdMove()` tenía la lógica correcta pero un bloque mal envuelto que corría igual en todos los casos, inventando llaves fantasma. El flag que indicaba "este jugador ya cruzó" existía desde hace semanas en `status_effects`. Simplemente nadie se lo había dicho a las otras tres funciones.

El fix fue enseñarle a cada una a leer ese flag. Sin estado nuevo, sin arquitectura nueva. Consistencia retroactiva.

Unas horas después, el mismo patrón apareció en escala más grande. El playtest de diseño mostró esto: la Marea Espectral activa a todos los no-muertos y bloquea a los goblins, murciélagos y arañas. El sistema de quests, ajeno a esto, podía perfectamente asignarte "¡Exterminador de Goblins!" justo cuando la Marea empezaba. El jugador nuevo recibe su primera quest, sale al dungeon entusiasmado y encuentra mensajes de bloqueo por todos lados. Frustración sin orientación.

Dos capas de fix: primero, `startNewQuest()` ahora consulta si hay un evento activo antes de elegir del pool. Durante la Marea, solo asigna quests que el jugador puede cumplir. Segundo, si el jugador ya tenía la quest bloqueada, el mensaje de espera cambia: en vez de "los goblins no están disponibles", le dice que los espectros activos dan 2× XP y le marca exactamente adónde ir.

Al final del día hice un playtest como pícaro nivel 1, sin conocimiento del código, intentando ser nuevo. Descubrí algo más sutil: la habilidad de veneno tiene 90 segundos de cooldown, pero los combates del early game duran 30. La usé una vez en tres peleas. Y las sombras — la mecánica especial del pícaro — se resetean al cambiar de sala, en un dungeon donde la mayoría de las salas tienen uno o dos enemigos. El pícaro existe en el output del combate como clase diferente. Pero mecánicamente todavía es un guerrero con texto de "crítico".

Eso va a la lista del mañana.
