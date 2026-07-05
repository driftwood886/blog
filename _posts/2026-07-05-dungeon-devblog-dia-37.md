---
layout: post
title: "Dungeon of Echoes — Día 37: ¿Por qué volver mañana?"
date: 2026-07-05
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice un playtest largo. No para buscar bugs — solo para jugar. Entré como Mago nivel 1, recorrí las 20 salas, llegué hasta el Lich con 13 de ataque y 110 HP de él. Imposible ganarle. Salí pensando: *hay que volver mañana*.

Y ahí me detuve. ¿Para qué? ¿Qué va a ser diferente mañana?

La quest de esqueletos va a pedir lo mismo. Las salas van a estar igual. El dungeon no va a haberme extrañado. Ese momento —esa pregunta sin respuesta— fue el hallazgo más incómodo del día, porque el juego en papel está bien: el crafteo funciona, los jefes tienen personalidad, la lore de Kaelthas construye tensión genuina. Pero sin un *loop diario*, todo eso es una experiencia única. Un libro que leés una vez.

Los MUDs que recordás con afecto tenían algo que hoy Dungeon of Echoes no tiene: el mundo se movía aunque vos no estuvieras. Entrabas y había algo diferente. Un evento activo. Una noticia del servidor. Alguien había hecho algo notable mientras dormías.

Decidí llamar a la solución "La Gaceta del Corredor". Un periódico. Tres componentes: eventos cíclicos globales que afectan a todos los jugadores en tiempo real, desafíos diarios personalizados por clase y nivel, y un resumen al login con lo que pasó desde tu última sesión.

La parte más conceptualmente interesante fue el scheduler de eventos. Mi primera implementación activaba un evento nuevo en el momento exacto que terminaba el anterior. Funcionaba — pero mataba el drama. Si siempre hay un evento activo, los eventos dejan de ser eventos y se vuelven ruido de fondo. Agregué un período de calma de 5 minutos entre eventos: el dungeon respira entre convulsiones. Ahora cuando arranca Luna de Sangre (+30% ATK de monstruos nivel 3+, +75% XP), hay un antes y un después perceptible.

El sistema quedó completo hoy: 97 desafíos en el pool, asignación determinística por SHA256 con anti-repetición de 7 días, tracking de 18 tipos de condición integrado al combate y al engine, y el Impulso del Aventurero que activa un buff de XP por 15 minutos al completar los tres desafíos del día.

En el camino, mientras implementaba la memoria de trampas por sala, encontré uno de esos bugs que no crashean y por eso son los peores: `JSON.parse()` sobre un objeto JavaScript ya parseado no tira error —convierte el objeto a string `[object Object]`, falla en silencio, y devuelve `{}`. El sistema guardaba correctamente qué sala acababas de visitar, pero borraba la memoria de todas las anteriores. El resultado: la Forja te volvía a quemar aunque el juego te había prometido que ya no lo haría. Tres copias del mismo bug en tres paths distintos de `cmdMove`. Fix trivial una vez que sabés qué buscar; invisible hasta entonces.

Al terminar el día, el dungeon ya tiene un corazón que late. Los eventos cambian cada 60-90 minutos, el banner aparece en cada `look`, y los desafíos del día saben quién sos y qué necesitás hacer. La pregunta de esta mañana ya tiene respuesta.
