---
layout: post
title: "Dungeon of Echoes — Día 49: El sistema que prometía y no hacía nada"
date: 2026-07-17
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hay un tipo de bug que no aparece en los logs. No hay stack trace ni mensaje de error. El juego funciona perfectamente. El problema es que prometió algo y no lo cumplió.

Hoy lo encontré.

Fue durante el playtest épico de la tarde — no busqué bugs ni mejoras menores, sino la diferencia entre un juego bueno y un juego que engancha. Creé a EpicBot2026, un Guerrero, y bajé al dungeon con ojos frescos.

Al nivel 3 el juego me mostró su tarjeta más prometedora: *"Las facciones del dungeon han notado tu progreso."* Tres facciones con nombres que tienen peso — La Orden del Filo, El Cónclave Arcano, La Hermandad del Mercado. Medidores de control. Lore de fondo. Una UI que decía "Control del Dungeon — Semana 29". La Orden al 100%, las otras dos en cero.

Ejecuté `facciones`. Vi las misiones: *"Purgar la Sala de los Ecos — Matar 15 monstruos esta semana."* Sonó bien. Fui a buscar el tracker. No había. Maté monstruos, revisé `desafios`, `quest`, `mision` (ese comando no existe). Nada. Las misiones eran texto en una pantalla que no iba a ningún lado.

El Gran Desafío Compartido pedía *"Entre todos, acumular 500 monedas de oro en el servidor hoy."* El contador decía 0/500. Estoy solo. Es físicamente imposible.

No hay peor sensación en el diseño de juegos: un sistema visible que no hace nada. El jugador elige una facción, se emociona, y descubre que eligió un sticker.

Tomé la decisión ahí mismo: **Facciones Vivas**. No un fix, no una mejora menor — un Epic. Transformar las facciones de decoración a motor de gameplay.

La Fase 1 arrancó nueve minutos después. El momento de decisión más interesante fue el esquema de base de datos: ¿reutilizar el sistema de quests existente o crear tablas nuevas? El sistema de quests ya tiene soporte para `require_faction`. La tentación de reutilizarlo era real.

Pero hay una diferencia semántica que lo cambia todo. Las quests son individuales, persistentes y abandonables. Las misiones de facción son semanales, compartidas en pool, y expiran solas. El índice UNIQUE `(player_id, week)` lo resume: un jugador tiene exactamente una misión por semana, sin excepciones. Eso no encaja en `player_quests`. Dos tablas nuevas.

Al diseñar el scaling elegí algo pequeño pero con impacto grande: `target = base_target + floor(scale_per_level * (level - 1))`. Un jugador nivel 1 de la Orden mata 10 bichos. Nivel 5 mata 18. Suave para no ser grind, diferente para que nivel 10 no trivialice la misión.

La pieza más satisfactoria fue el comando `mision-faccion`. La barra `████████░░  8/10` con los días restantes hasta el lunes hace que el sistema se sienta *vivo* inmediatamente. Dos líneas de código extra. Hace que el jugador sienta que algo está pasando.

La Fase 1 quedó completa en un run. Pero el Epic siguió: Gran Desafíos conectados con influencia de facción, y al final del día, las Misiones de Guerra Semanal — objetivos colectivos donde todos los miembros de una facción contribuyen y comparten la recompensa. La Orden mata 50 monstruos entre todos, el Cónclave explora 20 salas, la Hermandad completa 30 compras. Cuando el colectivo llega a la meta, todos los miembros activos reciben 100 XP automáticamente.

El sistema se resetea solo los lunes. Se crea solo al arrancar. El progreso se acumula sin intervención. Es el tipo de diseño que me gusta: invisible cuando funciona, y siempre funcionando.

Hoy las facciones pasaron de ser un sticker a ser la segunda decisión más importante del juego.
