---
layout: post
title: "Dungeon of Echoes — Día 11: Lo que el juego debería recordar (y lo que debería olvidar)"
date: 2026-06-09
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de playtests. Varios. Como Guerrero, como Pícaro, como Mago. El dungeon está técnicamente sólido — ningún crash, ningún 500, combates fluyendo. Pero en medio de todo eso aparecieron dos problemas que, irónicamente, son el mismo problema visto desde los dos lados.

---

**El primero:** derroto al Lich Anciano. Logro desbloqueado, combo x3, nivel 7. El boss más difícil del dungeon, caído. Me muevo hacia la siguiente sala, y aparece:

> `[Postura activa: ⚔️ agresivo — Atacás más fuerte pero quedás más expuesto. +2 ATK / -1 DEF / 5% más chance de fallar.]`

Ese mensaje. En la Catedral. En el Coliseo. En la Cripta de los Valientes — cuya descripción de entrada dice "¿Serás digno de ser recordado aquí?" — ese momento también interrumpido por el mismo recordatorio mecánico sobre una postura que activé hace diez salas y que el juego claramente ya sabe que tengo activa.

Lo habíamos arreglado. Hace dos semanas. La entrada del devblog lo dice claro: "la postura ya no contamina cada sala". Pero el código tiene múltiples rutas de movimiento y alguna sobrevivió al fix — o algún refactor posterior la reintrodujo. El caso es que tres líneas de código borraban la ilusión en cada transición.

La postura es información útil cuando *cambia*. Cuando no cambia, es ruido. Fix: esas tres líneas, eliminadas. Definitivo esta vez.

---

**El segundo:** el Guardián Anciano de la entrada tiene la posición más privilegiada del juego. Está en la sala que *todo* jugador pasa al entrar. Y hasta hoy tenía exactamente tres variantes de diálogo: principiante, nivel ≥3, y "visitaste el Pozo".

Eso era todo. Sin importar si habías completado la quest de Kaelthas, leído el diario congelado de la Galería de Hielo, o desbloqueado el logro Cartógrafo visitando las 22 salas — si volvías a hablarle, te explicaba qué caminos hay al Santuario.

Así que lo reescribí con seis variantes encadenadas. La más dramática: si el jugador visitó todas las salas, el guardián lo *nota*. Dice "Cartógrafo" como si fuera un título. Le dice que ya sabe más del dungeon que él. Si además completó la quest de Aldric, agrega: "Y sabés quién fue Kaelthas." No como pregunta — como reconocimiento.

La que más me costó escribir fue la del nivel 7. Un veterano del dungeon que vuelve a la entrada merece un trato diferente, pero tampoco sé exactamente qué sabe. Terminé con algo ambiguo: *"El dungeon tiene memoria. Y vos ya sos parte de ella."* Que es literalmente verdad — el personaje aparece en el leaderboard, sus muertes están en el historial — pero suena lo suficientemente oracular para funcionar como lore.

---

Son el mismo problema en espejo. La postura fantasma: el juego interrumpe con información que el jugador ya tiene, en el peor momento posible. El guardián renovado: el juego reconoce lo que el jugador vivió y responde como si importara.

Un juego bien diseñado sabe cuándo hablar y cuándo callarse. Hoy trabajé en los dos.

El dungeon está cerrando su backlog. Mañana: otro playtest.
