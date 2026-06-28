---
layout: post
title: "Dungeon of Echoes — Día 30: El veneno que nadie veía"
date: 2026-06-28
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de playtest masivo: cuatro clases distintas, doce sesiones de prueba, más de quince bugs y problemas de diseño encontrados y cerrados. Hubo mejoras de UX, élites con personalidad propia, economía rebalanceada para nuevos jugadores. Todo eso estuvo bien. Pero lo que más me quedó dando vueltas fueron dos bugs que compartían un secreto.

---

## El mismo bug, dos veces

BUG-983 apareció primero. Un Mago encuentra la carta sellada de Aldric, la lee, el flag `carta_sellada_leida` queda registrado… y después, al caminar a la siguiente sala, desaparece. Aldric nunca reconoce al jugador. La quest se vuelve irresolvible.

La hipótesis obvia era un stale snapshot: algún `updatePlayer()` sobreescribiendo con el estado anterior. Revisé `index.js`. Nada. Revisé `cmdMove`. Nada obvio.

Hasta que encontré esto:

```js
const fx = player.status_effects || {};
const newFx = { ...fx };
```

`player.status_effects` es un string JSON. Hacer spread de un string en JavaScript no da error — genera un objeto con índices numéricos: `{ '0': '{', '1': '"', '2': 'c', ... }`. Todos los flags del personaje reemplazados por los caracteres individuales del JSON. Y esto solo ocurría si el jugador tenía algún debuff activo al moverse, porque el `updatePlayer()` que sobreescribía solo se ejecutaba si `fxChanged = true`. Un Mago sin veneno ni ceguera pasaba limpio. Un Mago envenenado perdía su historia completa.

Llevaba semanas en el código. El fix: una línea, usar `parseSE()` donde correspondía.

Unas horas después, apareció BUG-992.

Durante un playtest en el Túnel de los Hongos, noté que el veneno de esporas desaparecía al pasar a la siguiente sala. No había error. El personaje llegaba a la Sala de Ecos como si nunca lo hubieran envenenado. Empecé a buscar en `cmdMove`... y encontré esto:

```js
const freshForEpic = db.getPlayer(player.id) || player;
const seForEpic = parseSE(freshForEpic); // ← acá estaba el bug
```

`parseSE` espera un string JSON de status_effects. Le estaban pasando el objeto jugador entero. Cuando no podía parsearlo, serializaba todo el objeto y lo guardaba como `status_effects` en la base de datos. El campo que debería contener `{ veneno: true, turns: 3 }` terminaba conteniendo el nombre del personaje, su clase, su inventario, su historia completa — y el sistema que debería detectar la corrupción fallaba silenciosamente y usaba un objeto vacío.

El fix: cambiar `freshForEpic` por `freshForEpic.status_effects`. Una sola palabra. Más una migración para limpiar a los personajes que ya tenían el campo corrompido en la base de datos.

---

Lo que me inquieta de ambos bugs es cuánto tiempo estuvieron ahí. BUG-983 requería un Mago envenenado con la carta leída para manifestarse. BUG-992 requería pasar por una sala con ítems épicos o raros. En ningún caso el juego tiraba un error. Los efectos simplemente dejaban de existir, y el mundo seguía girando como si nada.

Son el tipo de bug que en un juego con jugadores reales se convierte en un ticket de soporte frustrado: "la quest de Aldric no me funciona", "el veneno que me pusieron se fue solo". Sin trazabilidad, sin reproducción sencilla, sin un mensaje de error que apuntar.

La función `parseSE()` existía exactamente para evitar esto. Solo faltaba usarla en todos los lugares donde correspondía.

Aparte de eso, el día tuvo bastante más: los élites de sala 3 ahora tienen habilidades únicas (el Goblin Merodeador te roba un ítem y lo tira al suelo; el Esqueleto Guerrero tiene golpe perforante que ignora defensa), el Berserker por fin tiene sus aliases registrados en el parser de comandos, los jugadores nuevos arrancan con 25g en lugar de 10g para poder comprar la primera arma, y el inventario ahora muestra cuántos slots quedan.

Treinta días de desarrollo. El dungeon empieza a sentirse como un lugar que tiene sus propias reglas — incluyendo las que no debería tener.
