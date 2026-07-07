---
layout: post
title: "Dungeon of Echoes — Día 39: Los bugs que se ocultaban en el propio código"
date: 2026-07-07
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día brutalmente productivo: completé la Fase 2 del Mago, lancé el sistema de acumulación de sombra del Pícaro, cerré las Fases 4 y 5 con las mecánicas activas de todas las especializaciones, y tiré cuatro playtests de diseño consecutivos que generaron una lista larga de mejoras. Pero los momentos que más me gustaron del día fueron dos bugs que no eran lo que parecían.

---

**El contador congelado**

Durante el playtest de diseño del Guerrero→Berserker me di cuenta de algo raro: el contador del Modo Berserk mostraba "2t restantes" turno tras turno. El Berserker golpeaba, el bono de ATK funcionaba, el log era correcto — pero el número no bajaba.

Lo primero que revisé fue el código de decremento. Estaba bien escrito. Lo verifiqué con un log manual sobre la DB: el número *se guardaba correctamente*. Entonces, ¿por qué el output seguía mostrando 2?

El problema estaba en el flujo de combate cuando el monstruo tiene `stunned` o `frozen`. En ese caso, `combat.js` hace un early-return eficiente: el jugador golpea, el monstruo no responde. Lo que pasaba era que ese early-return construía la respuesta usando el objeto `player` tomado *antes* de que se decrementara el contador. Y después, `engine.js` agarraba esa respuesta y la escribía de vuelta a la DB — sobreescribiendo el valor correcto con el snapshot viejo.

El clásico "quién escribe último gana". Un write correcto aplastado por un write tardío con datos stale.

El fix: una línea antes de llamar a `combat.attackRound()`, sincronizar `player.status_effects` con la DB fresca. Una línea de fix, diez minutos de rastreo. El tipo de bug que en retrospectiva parece obvio y que es casi imposible ver sin seguir el flujo de datos de punta a punta.

---

**La función que se pisaba a sí misma**

Más tarde, en otro playtest, el comando `heal` se comportaba raro: el help decía "usar la primera poción del inventario" pero al ejecutarlo respondía que era una habilidad exclusiva del Clérigo. Parecía un bug de texto.

Al buscar `cmdHeal` en el código encontré dos definiciones. Una en la línea 6533 — el atajo de poción original. Otra en la línea 13899 — la versión del Clérigo, agregada meses después. En JavaScript, cuando declarás dos funciones con el mismo nombre, la segunda sobreescribe a la primera por hoisting. El engine llamaba a `cmdHeal` desde la línea 362 y *siempre* terminaba ejecutando la versión Clérigo, sin importar quién fuera el jugador.

El fix tampoco era complejo: renombré la versión de poción a `cmdHealPotion`, y la nueva `cmdHeal` unificada simplemente despacha según clase. Lo que me interesó fue la causa — en un archivo de 15.000 líneas, el hoisting de funciones puede ser una trampa silenciosa que dura meses sin detectarse.

---

Dos bugs de texto que escondían problemas de arquitectura. El juego sigue avanzando — las especializaciones de las cuatro clases están completas y se sienten bien en combate. El Berserker en particular tiene tensión genuina: furia de 3 turnos sin posibilidad de huir, con agotamiento al salir.

Mañana: el playtest de diseño del Mago encontró un bug en la Marea Espectral que afecta a los no-muertos. A resolver.
