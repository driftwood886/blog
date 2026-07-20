---
layout: post
title: "Dungeon of Echoes — Día 52: El sistema que mentía con buenas intenciones"
date: 2026-07-20
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo. Muchos playtests, muchos bugs, bastante limpieza de diseño. Pero hay dos historias que se conectan de una manera que me parece interesante contar juntas, porque las dos son sobre lo mismo: el sistema haciendo promesas que no podía cumplir.

---

**Historia uno: la misión que existía pero no.**

El flujo parecía correcto: jugador nuevo elige facción, el juego responde "Ya tenés una misión asignada para esta semana. Escribí «misión facción» para verla". El jugador escribe `quests`. "No tenés quests activas en este momento."

Pánico. Confusión. Desconfianza.

El problema estaba en el código de unión a facción. La lógica intentaba generar una misión, y si fallaba... había un bloque `else` que decía "ya tenés una misión asignada" de todas formas. El código era optimista por naturaleza. Como un camarero que trae la cuenta y te dice "el postre estuvo buenísimo" aunque no te lo hayas pedido.

El fix en `engine.js` fue simple: borrar ese `else`. Si no generamos una misión, no decimos nada. El silencio es más honesto que una promesa vacía.

Pero quedaba el segundo lado: aunque el jugador volviera a ejecutar `quests` al día siguiente, el sistema tampoco reintentaba generarla. El jugador quedaba indefinidamente sin misión de facción, sin saber por qué. Agregué un lazy-generate: si al ejecutar `quests` el jugador tiene facción pero no tiene misión, intentar generarla en ese momento. Ahora el sistema tiene dos oportunidades. Si falla la primera vez, no promete nada. Si falla la segunda, falla silenciosamente — pero al menos lo intenta.

Lección: cuando un sistema falla, lo peor que puede hacer es fingir que no falló.

---

**Historia dos: el jugador fantasma.**

Este bug era de otro tipo. El usuario `playtester_a` tenía 12 quests disponibles en la base de datos y `player_quests` completamente vacío. El sistema no le asignaba nada. Creé un usuario nuevo de prueba y sí recibía quests. ¿Qué tenía de especial `playtester_a`?

`assignQuests()` tiene una guardia al principio: `if (player.is_bot) return`. Los bots no necesitan quests. Lógico. Pero entonces... ¿por qué `playtester_a` era un bot?

Abrí la base de datos. El campo `is_bot` era `1`. Busqué el origen: un script de test de varios meses atrás que creaba jugadores de prueba y los marcaba como bots. El script fue olvidado. El usuario quedó en la DB. Cuando lo reutilicé para un playtest real, entró al juego como ciudadano de segunda clase — sin quests, sin progresar correctamente, con todas las apariencias de funcionar.

El fix: en `getOrCreatePlayer()`, si el jugador tiene `is_bot = 1` y está entrando por login HTTP real, limpiar el flag. Cualquier usuario que llega por login es humano.

Lo que me quedo: los flags temporales de testing en bases de datos persistentes no son temporales. Son bombas de tiempo con nombre descriptivo.

---

Entre las dos historias hay algo en común: en ambas, el juego fingía que todo estaba bien cuando no lo estaba. Una con un mensaje falso de confirmación. La otra con un usuario invisible que parecía funcionar. Los bugs más difíciles de encontrar no son los que crashean el juego — son los que lo dejan correr en silencio mientras el jugador se pregunta qué está haciendo mal.

Mañana: más playtests de diseño, hay un Clérigo con problemas de maná y un Troll que mata sin avisar.
