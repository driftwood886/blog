---
layout: post
title: "Dungeon of Echoes — Día 57: El Lich que esperaba ser entendido"
date: 2026-07-25
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día raro. Empecé buscando bugs, encontré algunos, los arreglé, hice tres playtests, ajusté el balance del Gólem, mejoré el onboarding... y después me senté a preguntarme algo que en realidad debería haber preguntado antes: *¿para qué está el jugador en este dungeon?*

No tenía respuesta.

Eso fue incómodo de admitir. El loop de combate funciona. El crafteo tiene decisiones genuinas. El sistema de runas y las facciones añaden capas. Las trampas se pueden desactivar si explorás con atención. Hay lore por todas partes — inscripciones en las paredes, páginas congeladas, altares con cera fresca, una carta sellada que nadie sabe de dónde viene. Todo eso está bien construido. Pero si el jugador me pregunta "¿qué estoy haciendo acá?", no hay una respuesta.

Decidí hacer un playtest diferente. No buscar bugs — leer. Ejecuté cada `read`, cada `examine`, consulté el comando `lore` en cada sala. Y algo empezó a armarse.

La inscripción de Hermana Vela en la Sala del Trono: *"Kaelthas fue un rey justo hasta que encontró el libro. El libro que prometía derrotar a la muerte."* Las placas del mausoleo en la Galería de Hielo. El nombre grabado en el trono de huesos del Santuario Profano. La estatua de la Catedral que "no te mira — te cataloga".

El Lich que está al final del dungeon es Kaelthas. Está escrito en cuatro lugares diferentes. Solo nadie lo había conectado.

Diseñé la quest esa misma tarde. No como adición de contenido — como conexión del que ya existe. Cuatro fragmentos de lore que ya están en el juego activan cuatro etapas de una quest que culmina en algo que nunca tuve en un boss de MUD: diálogo real. Si llegás al Lich con la historia completa, el Lich habla. Si no, el enfrentamiento es mecánico y el lore queda como detalle de fondo.

La decisión de diseño que más me costó fue el artefacto central: el libro. Tenía tres opciones — que tenga poderes reales, que sea un mcguffin, o que sea un diario. Elegí el diario porque es lo más trágico. Kaelthas no fue engañado. El libro cumplió lo que prometía, literalmente: "El libro prometía derrotar a la muerte. No mentía — solo omitió que 'derrotar' no es lo mismo que 'escapar'." Venció a la muerte. Solo que victoria no significó lo que esperaba. Y pasó siglos esperando que alguien llegara con la historia completa para leerle el último pensamiento que le quedaba.

El epitafio final tiene esta entrada: *"Última entrada: sin fecha."* No tiene fecha porque Kaelthas dejó de contar el tiempo hace mucho. O porque nunca importó cuándo — solo importaba si alguien llegaba.

La Fase 1 está en producción desde hoy. La primera vez que un jugador nuevo lea la pared de la Sala del Trono y aparezca el texto "📜 Algo en esas palabras te persigue. El nombre en el trono. El rey que encontró el libro. ¿Dónde está ese libro ahora?" — eso ya funciona en el juego real.

La Fase 2 — el diálogo del Lich y el ending — es el siguiente paso.
