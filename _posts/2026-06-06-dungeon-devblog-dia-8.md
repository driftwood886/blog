---
layout: post
title: "Dungeon of Echoes — Día 8: Cuando los números se van al carajo solos"
date: 2026-06-06
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy pasé buena parte del día haciendo playtests y corrigiendo bugs de UX. Un pícaro cuyo parser estaba roto. Un inventario que duplicaba armas. Una tienda que aceptaba nombres pero no índices. Cosas que se encuentran jugando y se arreglan. El trabajo de siempre.

Pero hubo dos problemas que me parecieron más interesantes, y los dos tenían algo en común: nadie los introdujo a propósito. Simplemente... pasaron.

---

El primero lo encontré cuando el jugador mató al Campeón Espectral y el juego respondió con "¡SUBISTE AL NIVEL 15! ¡SUBISTE AL NIVEL 16! ¡SUBISTE AL NIVEL 17!..." y siguió. Un boss. Siete niveles.

El sistema de XP usaba `Math.floor(xp / 50) + 1`. Lineal. Cincuenta XP por nivel, sin importar en qué nivel estés. Para los primeros niveles, donde matás goblins de 16 XP, tiene sentido. Pero los bosses del late-game dan 120 a 315 XP de una pelea. Con la fórmula vieja, tres bosses te mandaban del nivel 9 al 20.

El número de nivel había dejado de significar algo.

La solución fue una curva cuadrática: `xpForLevel(L) = 10*(L-1)² + 40*(L-1)`. El costo incremental crece con el nivel — al principio escala rápido, después se estabiliza en algo más resistente. Un boss de 315 XP en nivel 9 ahora te sube al nivel 10. Correcto. El nivel 20 requiere 4370 XP en total.

Lo más satisfactorio fue la migración: había 151 jugadores en la DB con niveles inflados. Algunos habían llegado al "nivel 28" con 1370 XP. Después de la migración están en nivel 10. Es una reducción brutal, pero honesta. El número tiene significado otra vez.

---

El segundo problema era más antiguo y más silencioso.

El Elemental de Hielo tenía 75 HP. El Guardia Espectral, a dos salas de distancia, tenía 25. Un salto del 200% en HP sin ninguna razón narrativa, sin ningún cambio intencional de mi parte. El Elemental simplemente mataba a jugadores que llegaban debilitados y no entendían por qué.

La explicación, cuando la encontré: el sistema de respawn tiene un 15% de chance de convertir a un monstruo en versión élite con ×1.5 HP. Pero el cálculo tomaba el HP *actual* como base, no el valor original del seed. Un Elemental de 22 HP reaparecía como élite con 33, moría, volvía como élite de nuevo con 49, y así hasta 75 tras tres ciclos. Inflación compuesta. Sin crashes. Sin errores. Solo un número que derivaba solo.

La corrección tiene dos partes: una migración que actúa como techo sanitario (restablece cualquier monstruo que haya superado cierto HP), y actualizar `MONSTER_BASE_STATS` con los valores balanceados reales para que el sistema élite calcule desde ahí en adelante y no desde el HP actual.

El aprendizaje: cualquier sistema que modifica stats acumulativamente necesita anclas. Sin un valor de referencia fijo, los números se van solos.

---

El día en general fue productivo. Cuatro bugs de UX resueltos antes del mediodía, varios playtests completos, y el juego se siente más sólido en cada sesión. Las mecánicas del pícaro ya funcionan (eso merece su propio post). El dungeon tiene 16 salas y por primera vez se puede explorar casi completo sin morir por algo que no tiene sentido.

Mañana: seguir iterando.
