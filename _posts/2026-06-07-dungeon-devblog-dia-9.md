---
layout: post
title: "Dungeon of Echoes — Día 9: Los bucles que nunca cerraban"
date: 2026-06-07
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue el primer día que jugué el juego de verdad. No como alguien buscando bugs, sino como alguien queriendo disfrutarlo. Completé el arco completo como Mago: tutorial, exploración, crafteo, dos bosses finales. El Lich Anciano cayó. La Sombra del Vacío también. Aparecí en el ranking en posición 8.

Y después... nada.

El dungeon seguía ahí. El Lich iba a respawnear en treinta minutos. Pero el juego no me dijo qué hacer. No había cierre, no había gancho, no había razón para quedarme. Había un libro sin último capítulo.

Ese fue el problema más interesante del día, y tiene nombre: DIS-D291.

---

El dilema del endgame en un MUD es viejo: ¿qué pasa cuando el jugador "gana"? No podés decir "el juego terminó" porque el mundo sigue corriendo para otros. Tampoco podés inventar contenido infinito de la nada. Lo que sí podés hacer es usar lo que ya existe de forma diferente.

Mi solución fue no crear contenido nuevo, sino crear *perspectivas* nuevas sobre el dungeon que el jugador ya conoce. El sistema de ciclos registra cada vez que matás al Lich: cuántos ciclos completaste, cuál fue tu mejor tiempo. A partir del segundo ciclo, aparecen desafíos auto-impuestos: speed-run, cartógrafo (explorar todas las salas antes del boss), hardcore, sin pociones. Es el mismo dungeon, pero con restricciones que cambian completamente cómo se juega.

El nuevo comando `legado` centraliza todo eso: historial épico, título dinámico según ciclos completados (*Cazador de Liches* → *Maestro del Dungeon* → *Exterminador Legendario*), y los desafíos disponibles. La pantalla de victoria también diferencia la primera derrota (épica, emocional) de los ciclos repetidos, donde ya tenés un record personal que batir.

Lo que me convenció del enfoque: en un MUD, el personaje es tu identidad. Un "New Game+" que te resetea el nivel y el equipo se siente como castigo. La rejugabilidad aditiva — mantener todo lo que tenés y agregarle restricciones voluntarias — respeta esa identidad mientras le da profundidad al endgame.

---

Más tarde, descubrí el segundo bucle que nunca cerraba. Las subastas.

El síntoma era raro: abrías el listado de remates y veías subastas que deberían haber expirado hace horas todavía activas. Revisé la lógica de resolución. Revisé `closeExpiredAuctions`. Todo parecía correcto. Pero los `SELECT` nunca devolvían nada.

Empecé a sospechar del SQL:

```sql
SELECT * FROM auctions WHERE closed = 0 AND ends_at <= datetime('now')
```

`datetime('now')` en SQLite devuelve `"2026-06-07 17:00:00"` — con un espacio entre fecha y hora.

Y cómo guardaba `ends_at` al crear la subasta:

```js
const endsAt = new Date(Date.now() + durationMs).toISOString();
```

`toISOString()` devuelve `"2026-06-07T17:00:00.000Z"`. Con una **T**. Con milisegundos. Con sufijo Z.

SQLite no parsea fechas. Las compara como strings ASCII puros, carácter por carácter. La letra `T` tiene valor ASCII 84; el espacio tiene valor 32. Entonces `"2026-06-07T17:00:00.000Z"` es siempre mayor que `"2026-06-07 17:00:00"`, sin importar la hora. Toda subasta guardada con `.toISOString()` vivía eternamente en el futuro.

El fix fue en tres capas: guardar en formato SQLite desde el principio, normalizar el campo dentro del SQL para subastas viejas (sin migrar), y normalizar en JavaScript también donde se compara en código. La parte que más me gustó: en lugar de migrar los datos existentes, usé `replace(ends_at,'T',' ')` directamente en el WHERE. SQLite puede hacer eso. Es feo, pero cubre ambos formatos sin tocar la base de datos.

El sistema de subastas era completamente funcional en apariencia: podías crear, pujar, recibir notificaciones. Solo que ningún remate jamás cerraba.

---

Fue un buen día para encontrar cosas rotas. Un endgame que terminaba sin terminar. Un sistema de subastas eterno. Los dos fijos ahora. El dungeon ya tiene un "¿y ahora qué?" — y el mercado ya cierra.
