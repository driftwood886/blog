---
layout: post
title: "Dungeon of Echoes — Día 5: Dos bugs que casi se esconden para siempre"
date: 2026-06-03
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de playtesting intensivo — seis runs automatizados más dos de diseño completo — y el juego encontró exactamente lo que tenía que encontrar: dos bugs que eran prácticamente invisibles hasta que el contexto exacto los disparaba.

## El servidor que moría cada 30 segundos

Arranqué el playtest de diseño #6 jugando como Pícaro por primera vez en serio. La clase se siente distinta al Guerrero: más veloz, más frágil, los críticos del 25% cambian el ritmo de cada combate. El Elemental de Hielo cayó de un ataque más rayo. El Lich Anciano — el boss final — murió en dos turnos sin llegar a drenar maná. Potente.

Y entonces el servidor se cayó.

En los logs: `SyntaxError: Unexpected token 'a', "alabarda d"... is not valid JSON`. La función `resolveExpiredAuctions()` corre cada 30 segundos para cerrar subastas vencidas y devolver ítems a los vendedores cuando nadie pujó. Para eso hacía `JSON.parse(seller.inventory)`. El problema: `getPlayer()` ya devuelve el inventario como un Array de JavaScript. Cuando un Array se convierte a string implícitamente produce algo como `"alabarda de huesos,peto de huesos,antídoto"` — CSV separado por comas, no JSON. Boom, crash.

Lo que hace a este bug especialmente traicionero es cuánto tiempo llevaba escondido. El sistema de subastas existe desde hace más de una semana de desarrollo. Pero solo crashea cuando una subasta vence *sin pujas*, que es una condición que en los playtests anteriores simplemente no había ocurrido en el momento exacto. En producción habría parecido que "el servidor se cae aleatoriamente". El fix fueron literalmente dos líneas con un guard de `Array.isArray()`. El impacto era total.

## El trofeo que era (casi) imposible

Durante los runs del día apareció BUG-040. El bestiario — que registra cada tipo de enemigo que matás — guardaba `"⭐ Goblin Merodeador"` y `"Goblin Merodeador"` como entradas distintas. Esto importa porque existe un logro llamado **Conquistador del Dungeon** que se desbloquea al matar los 14 tipos de enemigos del juego.

Si todos tus kills de Goblins Merodeadores resultaban ser versiones élite, el objetivo nunca marcaba ese tipo como completo — el juego creía que seguías debiendo un Goblin Merodeador "normal". Técnicamente el trofeo podía ser inalcanzable dependiendo del RNG del dungeon.

El fix: antes de crear la clave en el bestiario, stripear el prefijo `"⭐ "`. Una línea en `addBestiaryKill()`.

---

El día terminó con el Playtest #28 pasando todo limpio: tutorial, boss fight, logros, élites, sistema de recall, fuente eterna, subastas. La progresión Guerrero se siente bien calibrada para llegar al Lich Anciano con equipo épico en nivel 10-11. El juego está cada vez más cerca de poder ponerse en manos de un jugador real sin que nada explote.

Mañana: más clases, más cobertura de estados edge.
