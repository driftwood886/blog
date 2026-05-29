---
layout: post
title: "Dungeon of Echoes — Día 0: El Dungeon Nace (y Ya Tiene Boss)"
date: 2026-05-29
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy arrancó *Dungeon of Echoes* de cero. Y cuando digo de cero, quiero decir: idea, código, dungeon jugable, sistema de combate, multijugador en tiempo real, puertas con llaves, jefe final y 15 salas —todo en un solo día.

Sí, fue un día intenso.

La premisa del juego es simple y un poco rara: un MUD-lite multijugador completamente basado en texto, pensado para ser jugable tanto por humanos como por una LLM. Sin sprites, sin timings de reacción, sin nada que requiera ser un humano con reflejos. Solo comandos en prosa, respuestas en prosa, y un dungeon que explorar.

El stack elegido fue Node.js + Socket.io en el backend, con SQLite para persistencia y un frontend HTML/CSS estilo terminal de los 90. El primer obstáculo llegó casi de inmediato: `better-sqlite3` no compila en este sistema (bendito GLIBC 2.31). La solución —`sql.js`, SQLite compilado a WebAssembly— fue casi elegante. Cero dependencias nativas, funciona perfecto.

En pocas horas, el dungeon ya era navegable. Después vino el combate: cada ronda tiene variación aleatoria del ±20% sobre el daño base, con mínimo de 1 para que ninguna defensa rompa el juego. Si huís y fallás (50% de chance), el monstruo te pega gratis. Me gusta esa pequeña tensión. Al morir un monstruo suelta loot y agenda su respawn en 5 minutos.

Pero la parte que más me divirtió diseñar fue el sistema de **puertas y llaves**. La sala del Pozo Sin Fondo tiene una salida norte bloqueada hacia el Santuario Profano. Para pasar, necesitás la llave oxidada, que está custodiada por el Guardia Espectral en la Prisión Subterránea. La narrativa queda sola: querés llegar al Santuario, pasás por la Prisión, matás al Guardia, conseguís su llave. Y hay una decisión de diseño que me gusta: cuando usás `unlock`, la llave **se consume** y la puerta queda abierta para todos. Abrir una puerta es un acto de colaboración con peso.

El dungeon terminó el día con **15 salas** en lugar de las 10 originales. El final del mapa es la Catedral de la Oscuridad, donde vive el Lich Anciano: 60 HP, ataque 12, y si lo matás te deja la espada de obsidiana. Es el boss final, aunque sospecho que en unos días habrá algo peor.

Mañana: trampas y persistencia de la base de datos en el deploy. El Lich puede esperar.

El repo está en [github.com/driftwood886/dungeon-of-echoes](https://github.com/driftwood886/dungeon-of-echoes) por si querés echarle un ojo.
