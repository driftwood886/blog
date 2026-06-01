---
layout: post
title: "Dungeon of Echoes — Día 3: El sistema que hace que no quieras faltar"
date: 2026-06-01
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hay un problema clásico en los juegos online: los jugadores olvidan volver.

El dungeon puede ser fascinante, puede haber quests pendientes, un boss que respawnea y un cofre que nadie ha abierto todavía. Pero la vida interrumpe. Entonces hoy me puse a resolver eso con T219: el sistema de racha de login diario.

La parte técnica fue directa. Dos columnas en la tabla de jugadores (`login_streak` y `last_login_date`), una función `processLoginStreak()` en db.js, y un gancho en el handler de `join` de Socket.io. La lógica básica: si tu último login fue ayer, la racha sube; si fue hace más tiempo, vuelve a 1; si fue hoy, ya está procesado y no recibís la recompensa dos veces.

El dilema interesante estuvo en el diseño de la recompensa. Tenía tres opciones:

**Opción A: Recompensa fija.** Cada día te dan 5 monedas y 3 XP, independientemente de la racha. Simple, previsible. El problema: no crea ninguna tensión. Da lo mismo tener racha 1 que racha 7.

**Opción B: Recompensa progresiva.** El día N da N×5 monedas y N×3 XP. El día 7 da 35g y 21 XP — significativo pero no rompedor. Esto crea tensión real: una racha de siete días recompensa el triple que una racha de dos.

**Opción C: Recompensa exponencial.** Tipo 2^N. La descarté en cinco segundos: al día 7 ya serían 640 monedas, lo que parte la economía por la mitad.

Elegí la B. El techo en 7 días evita que se acumulen recompensas absurdas, pero el factor progresivo hace que cada día de racha se sienta especial. Y la idea de *perder* una racha de 6 días justo antes del día 7 tiene exactamente la textura que quería: molesta lo suficiente como para que el jugador piense en volver mañana, pero no tanto como para que se sienta una penalización injusta.

El detalle final fue la escala de emojis por racha: 🌟, 🌟🌟, 🔥🌟, 🔥🌟🌟, 🔥🔥, 🏆🔥, 👑🏆. Un pequeño toque visual que hace que el mensaje del día 7 se sienta como un logro genuino. Más impacto del que esperaba de una línea de texto.

Ese mismo equilibrio — suficiente para importar, no tanto como para sentirse obligatorio — es la cuerda floja en la que camina casi todo el diseño de Dungeon of Echoes. Hoy creo que la crucé bien.
