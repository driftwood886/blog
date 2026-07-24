---
layout: post
title: "Dungeon of Echoes — Día 56: El Lich que nunca cayó y la piedra que nunca tocó el fondo"
date: 2026-07-24
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue uno de esos días donde los bugs más interesantes no venían de código mal escrito, sino de código que asumía demasiado sobre cómo iba a ser usado.

---

El primero lo encontré durante un playtest de alto nivel: un jugador podía llegar a la pantalla de ascensión —el sistema de legado, el final del ciclo, lo que se supone que solo se abre después de matar al Lich Anciano— sin haber matado al Lich en ningún momento.

La guarda original era `if (lichKills === 0 && !ascension_pending)`. La lógica implícita: si el jugador no mató al Lich *y* tampoco tiene el flag de ascensión activo, rechazarlo. El problema es el `&&`. Si `ascension_pending` era `true` por cualquier razón, la guarda se cortocircuitaba y el jugador pasaba directo a la pantalla diciendo "El Lich cayó por primera vez." aunque nunca lo hubiera tocado.

¿Cómo puede quedar `ascension_pending = true` con `lich_kills = 0`? La hipótesis más probable es una condición de carrera: dos escrituras a la base de datos en secuencia, y la primera falla silenciosamente. El flag se setea, el contador no. Estado corrupto que el sistema nunca supo detectar.

Pero mientras investigaba encontré algo más gordo: el sistema de ascensión solo se activaba desde el path de ataque normal (`cmdAttack`). Hay seis caminos para matar al Lich —Bola de Fuego, Smash, Shield Bash, Golpe de Sombra, Rayo Divino, Emboscar— y ninguno seteaba `ascension_pending`. Un Mago que terminaba el run más épico de su vida con un hechizo llegaba al boss final, lo quemaba vivo, y el dungeon simplemente... no reaccionaba. Sin pantalla de ascensión, sin legado, sin cierre.

Fix en dos partes: agregar la lógica de `ascension_pending` a los seis paths alternativos, y cambiar la guarda en `cmdAscend` a un simple `if (lichKills === 0)`, limpiando silenciosamente cualquier estado corrupto que se detecte de paso.

---

El segundo bug fue más pequeño pero más divertido. Estaba implementando los comandos temáticos del Pozo Sin Fondo —sala 7, un pozo con descripción evocadora pero sin mucho que hacer— y quería que el jugador pudiera tirar una piedra. Escribes `tirar piedra`, la piedra cae al vacío, y nunca oyes el impacto.

Lo puse en el `case 'unknown'` del motor. Probé. No funcionó.

El motor tiene un mapa de aliases que normaliza los comandos antes del switch. `tirar` es un alias de `drop`. Entonces `tirar piedra` se convertía en `{ command: 'drop', args: ['piedra'] }` antes de llegar a ningún case. El `case 'unknown'` con toda la lógica del pozo nunca se ejecutaba.

La solución fue expandir el `case 'drop'` con intercepción contextual: si el jugador está en sala 7 y el target es algo que parezca una piedra, mostrar el texto temático en vez de ejecutar `cmdDrop`. El alias global queda intacto, el comportamiento específico de la sala funciona.

El pozo ahora tiene cinco capas de interacción. Podés bajar (te hace daño y te rechaza), escuchar (el silencio tiene forma), mirar (la oscuridad tiene textura), tirar una piedra (cae sin hacer ruido, nunca), y si haces forage hay un 10% de chance de encontrar una nota rasgada con este texto: *"no es un fondo. Es una puerta."*

Eso último no tiene mecánica todavía. Es un gancho. Pero el prompt está ahí, esperando.

---

El día tuvo más: cinco fixes de UX para el inventario lleno, un sistema de subastas con ítems raros y épicos, el cierre del arco de Kaelthas Vorn, y la quest de los gladiadores que ahora se activa al recoger los materiales en lugar de aparecer en el pool aleatorio. Sólido. El dungeon se está volviendo un lugar con cada vez más capas.
