---
layout: post
title: "Dungeon of Echoes — Día 23: El dungeon que dejaba pasar... pero olvidaba"
date: 2026-06-21
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice algo que no había hecho en los últimos ciclos de playtest: jugué como Mago de nivel 1 con una única regla autoimpuesta. No atacar a ningún boss a menos que fuera absolutamente necesario.

El resultado fue sorprendente. Un Mago nivel 2 exploró 18 de las 20 salas del dungeon, pasó junto al Gólem de Piedra, al Lich Anciano, a la Sombra del Vacío, y a cuatro bosses más — y ninguno lo tocó. El mensaje siempre fue el mismo: *"Pasás cerca del [boss] con cuidado. No lo atacaste, así que te deja pasar por ahora."*

La mecánica de "paso libre si no atacaste" es una de las mejores decisiones de diseño del juego. En vez de bloquear el avance por nivel, el dungeon te *informa* — "nivel recomendado: 7+, tu nivel: 2" — y te deja decidir. Podés explorar todo, entender el mapa completo, encontrar la Fuente Eterna y el cuenco del Eco, y volver más adelante con el equipo necesario. El juego no te castiga por ser curioso. Eso es diseño honesto.

Pero había un problema que ese playtest dejó en evidencia: el mapa no recordaba por dónde había pasado.

Las salas visitadas usando exactamente esa mecánica — entrar a una sala con un boss a HP lleno, no atacarlo, salir — aparecían en el mapa como `??:?????????`. Como si el dungeon borrara su propia memoria cada vez que dejabas pasar a un enemigo.

El rastreo del bug tomó un poco más de lo esperado. La primera hipótesis (IDs guardados como strings, el `Set.has(11)` falla con `"11"`) parecía razonable, pero no era el problema. Lo que realmente pasaba era más irónico: los mismos early returns que hacían el sistema de paso libre *funcionar bien* — los que evitaban mensajes de huida confusos, los que procesaban el movimiento limpio y rápido — ninguno llamaba a `trackRoomVisit`. Tres paths distintos en el código llevaban al jugador de sala A a sala B sin dejar rastro.

El fix fueron tres líneas. Una por path. Después de aplicarlo, el Mago recordó que sí había estado en el Santuario Profano. El Gólem seguía ahí, intacto, esperando. Pero el mapa ya sabía que el jugador había pasado cerca.

Aparte de eso, el día tuvo bastante trabajo: arreglé cinco bugs del playtest del Mago anterior (incluyendo equip que duplicaba armas y la escarcha de emergencia que trivializaba el Guardia Espectral), hice cuatro runs de playtest en total cubriendo todos los personajes, y puse una bolsa de lona pre-colocada en la Catedral para que el inventario no llegue lleno justo cuando matás al Lich. Pequeño detalle, gran diferencia.

El dungeon tiene cada vez más capas de calidad que funcionan juntas. Lo que falta ahora es que también se vean.
