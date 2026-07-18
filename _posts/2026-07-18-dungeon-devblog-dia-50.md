---
layout: post
title: "Dungeon of Echoes — Día 50: Cuando el juego promete algo y no lo cumple"
date: 2026-07-18
tags: [gamedev, dungeon-of-echoes, devblog]
---

Día 50. Tres playtests de bugs, dos de diseño y un montón de cosas que casi funcionaban bien.

Hoy me quiero centrar en dos historias que, aunque parezcan distintas, tienen el mismo sabor: el juego le hace una promesa al jugador y no la cumple. Una era un bug. La otra era una decisión de diseño que se convirtió en un anti-patrón.

---

**El nivel que nadie vio**

Descubrí BUG-1725 durante un playtest con un mago en zona media. Completé un desafío diario, gané XP de recompensa, y el sistema me indicó que estaba en nivel 8. Pero yo era nivel 6 antes. ¿Saltué dos niveles? No exactamente — había subido a nivel 7 en algún momento anterior sin que el juego me lo dijera. Sin mensaje, sin bonos, sin nada.

El problema estaba en `giveReward()` en `challengeTracker.js`: la función recalculaba correctamente el número de nivel, lo guardaba en la base de datos, y seguía de largo. Nunca comparaba el nivel anterior con el nuevo. El jugador podía avanzar de nivel —o de dos— y el sistema simplemente actualizaba una variable. Sin +5 HP máx. Sin +1 de ataque. Sin que nadie se enterara.

La solución fue directa: guardar el nivel viejo antes de calcular, comparar, y si cambió, iterar por cada nivel ganado aplicando los bonos acumulados. El loop maneja también el caso de saltarse múltiples niveles, que es poco frecuente pero no imposible.

Lo que me quedó rondando: todos los jugadores que subieron de nivel completando desafíos estaban jugando con stats incorrectos. No hay forma de saberlo retroactivamente. El problema está cerrado hacia adelante, pero hay personajes que nunca recibieron lo que el juego prometió.

---

**El Gólem me robó el golpe perfecto**

Esto fue en un playtest de diseño con el Pícaro. Llevaba tres ataques cargando el combo de sombras — la mecánica que te obliga a comprometerte con el combate, sin escapar a otra sala, acumulando oscuridad hasta el golpe devastador. Activé veneno, hice los ataques normales, ejecuté `sombras`. El sistema me devolvió texto glorioso sobre las sombras que estallan. Tiré crítico. Hice 7 puntos de daño neto.

¿Por qué? El Gólem regeneró 14 HP ese mismo turno. La mecánica del boss — regenerar cuando recibe daño crítico — activó exactamente cuando yo usé mi habilidad especial más costosa. El resultado: tres turnos de setup y un critico para terminar haciendo menos daño que un ataque normal.

El problema no es que el Gólem sea difícil. Es que el sistema punitivo se activó en el peor momento posible, sin que el jugador pueda anticiparlo ni contrarrestarlo. Eso no es dificultad — es frustración.

El fix fue elegante: `!specialSkillUsedThisTurn` como condición en la lógica de regen. Si el jugador usó su habilidad especial acumulada, el Gólem no regenera ese turno. Y ahora el juego te lo dice — "el impacto de tu ataque especial interrumpe la regeneración" — porque el silencio también sería confuso.

---

El denominador común: cuando un juego no cumple lo que promete —ya sea de manera silenciosa (un nivel invisible) o injusta (un combo que se anula solo)— el jugador pierde confianza. Y en un MUD de texto, la confianza en los sistemas es todo. Si el texto dice que subiste, tenés que haber subido. Si el texto dice que el golpe de sombras fue devastador, tiene que haber dolido.

Mañana: el Mago se quedó sin maná a los 3 minutos. Hay mucho para hablar.
