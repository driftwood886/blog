---
layout: post
title: "Dungeon of Echoes — Día 59: El Berserker que no hacía lo que prometía"
date: 2026-07-27
tags: [gamedev, dungeon-of-echoes, devblog]
---

El Berserker es la clase más frágil del juego. Menos HP que el Guerrero base, sin escudos, sin magia de curación. Todo lo que tiene es daño. La fantasía es clara: sos una bomba andante. Sacrificás vida por poder, y si jugás bien, destruís todo antes de que te destruyan a vos.

Hoy descubrí que esa fantasía no funcionaba.

El combo central del Berserker es `furia → smash`. Activás furia (te cuesta el 20% de tu HP), tu próximo ataque hace ×2 de daño, y `smash` ya tiene ×1.8 propio. En teoría: ×3.6 de daño total. En la práctica, desde siempre, lo que ocurría era esto: activabas furia, usabas smash, y el golpe salía sin el bonus. El ×2 aparecía en el *siguiente ataque automático* — el más irrelevante del combate.

El jugador que construía su estrategia alrededor del combo simplemente concluía que el Berserker era débil. No había mensaje de error. No había crash. Solo la expectativa que nunca se cumplía.

El origen era arquitectural: el juego tiene dos caminos de combate. `combat.js` maneja los ataques normales y tenía el código para leer y consumir el buff de furia. `engine.js` tiene los bloques para cada skill especial — incluyendo `smash`. Cuando se implementó `berserker_rage`, nadie lo llevó al bloque de smash. El multiplicador estaba guardado en `active_scrolls`, esperando, y smash pasaba de largo.

El fix fue espejo exacto: copiar el patrón de `combat.js` al bloque smash de `engine.js`. Leer el buff, aplicarlo, consumirlo, mostrar el label `🪓 [FURIA ×2.0]`. El combo ahora produce ×3.6. El Berserker por fin es lo que decía ser.

Pero no terminó ahí.

Horas después, un playtest con personaje de nivel 12 encontró otro bug en la misma clase. La descripción de `modo_berserk` es explícita: "+5 ATK por 3 **turnos de ataque**". La Sombra del Vacío me aplicó parálisis en el primer turno del berserk. El contador pasó de 3 a 2 sin que yo hubiera atacado.

La promesa del sistema dice *turnos de ataque*, no *turnos de tiempo*. Cuando perdés el turno por parálisis, no atacaste. El berserk debería conservarse.

El código hacía esto: decrementaba el contador, lo persistía en base de datos, y *después* llamaba a `combat.attackRound()` para ejecutar el ataque. Si esa función detectaba parálisis, retornaba `{ paralyzed: true }` — pero el decremento ya había ocurrido. El counter se fue solo.

Al revisar el código para otro bug anterior (`spectralBlocked`), encontré que la solución ya existía. Cuando la Marea Espectral bloquea un ataque, hay un bloque de "reversión de recursos" que restaura el estado pre-decremento. El precedente era perfecto: extendí esa misma lógica al caso `paralyzed: true`. Si te paralizaron, el contador vuelve a donde estaba.

Lo más delicado fue el agotamiento post-berserk — los 2 turnos de penalidad que siguen al modo. Si estás en agotamiento y te paralizan, tampoco debería decrementar. Requirió capturar el snapshot del estado antes de decrementar, igual que ya hacía otro sistema similar.

El resultado: el Berserker ahora hace lo que promete en ambas situaciones. El combo de furia explota como diseñado. La parálisis no te roba turnos de berserk. 

Hubo muchas más cosas hoy — el backlog llegó a cero en algún momento de la tarde, una rareza histórica; agregué una mecánica donde quedarse en una sala vacía después de limpiarla acelera el respawn (la sala pasa de "muerta" a "en tensión"); corregí un bug donde los desafíos diarios contaban cada kill dos veces. Pero la historia del día es el Berserker: una clase diseñada para el riesgo calculado que llevaba tiempo siendo riesgo sin recompensa. Ya no.
