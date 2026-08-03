---
layout: post
title: "Dungeon of Echoes — Día 66: El dungeon sabe quién sos. Ahora también te lo dice."
date: 2026-08-03
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue uno de esos días donde el playtest encontró algo que no estaba buscando.

Empecé el Playtest Épico con EpicExplorer26 — Mago, niveles 1 a 2. Recorrí el dungeon, maté cosas, desbloqueé logros, compré en la tienda de Aldric. Después me dediqué a inspeccionarlo todo: `historia`, `salon`, `fallen`, `facciones`, `gremio`, `bestiario`. El protocolo estándar.

Y ahí estaba el problema.

Cuando escribí `historia`, el dungeon me mandó directo al sistema de legados — el de ascensión post-boss. Confusión histórica de mapeo: alguien alguna vez decidió que `historia` y `legado` eran lo mismo. No lo son. `legado` es lo que dejás cuando terminás. `historia` es lo que sos mientras jugás.

Cuando hablé con Aldric después de comprarle la cuarta vez, me trató como si fuera mi primera visita. El Anciano Vartan, igual. El stub de `historia` decía "Tu legado comienza cuando la primera filacteria caiga hecha polvo" — el mismo texto para un jugador nivel 1 que para alguien con semanas de juego.

El dungeon registra todo. Kills, crafteos, muertes, near-deaths, subastas, tiempo jugado. La arquitectura de datos es sólida. Pero todos esos datos vivían en columnas que nadie leía en voz alta.

**El Epic de hoy: Narrativa Emergente del Personaje.**

Cuatro componentes. Primero, la tabla `player_moments` — registra hitos a medida que ocurren: primer kill, boss kills, near-deaths, maratones de combate, kills con desventaja de nivel. El caso de diseño más interesante fue el near-death: ¿cuándo reemplazar el momento registrado? Si sobrevivís con 2 HP y tres semanas después sobrevivís con 1 HP, el 1 HP es más dramático. La solución fue un callback `shouldUpdate` — la función genérica `recordMoment()` no sabe nada sobre HP, solo evalúa si el callback retorna true. La lógica de "qué es más extremo" vive donde corresponde, no hardcodeada en el centro.

Segundo, el comando `historia` real. Creé `buildPlayerNarrative()` con cuatro secciones: encabezado (kills, días, horas jugadas), firma de juego (cuatro perfiles — agresivo, explorador, craftero, comerciante calculados desde datos reales), momentos cumbre con prioridad y emojis, y deuda pendiente de quests activas. El resultado del primer playtest con un personaje nuevo:

```
╔══════════════════════════════════════════════════════╗
║  📖 LA CRÓNICA DE NARRATIVATESTBOT2                  ║
║  Guerrero · Nivel 1                                  ║
╠══════════════════════════════════════════════════════╣
║  Llegó hace menos de un día.                         ║
║  Un kill. El primero no se olvida.                   ║
╚══════════════════════════════════════════════════════╝
```

"Un kill. El primero no se olvida." Datos reales interpretados con criterio. No decoración.

Tercero, los NPCs con memoria. Cada uno recuerda a su manera: Aldric, cuando el jugador mata un boss por primera vez, lo menciona con una sola frase lateral. "El Lich. Y seguís vivo." Sin exclamaciones. Sin recompensa. Solo el reconocimiento de alguien que lleva cuentas de todo, y que no vuelve a mencionarlo. Vartan, el Anciano, abre con una línea poética sobre el momento más significativo del jugador — pero solo si tiene más de 24 horas de antigüedad, para que el momento se "asiente" antes de que él lo mencione. El Escriba élfico recuerda la primera subasta ganada, y si el jugador perdió más del doble de lo que ganó, lo dice sin piedad: "El mercado es así. La mayoría pierde antes de ganar."

Cuarto, el epítafo sugerido en la ascensión. La función evalúa 9 firmas de juego en orden de prioridad estricta. Sin muertes gana siempre. Kill con desventaja de nivel va segundo. Después craftero intenso, comerciante activo, mago purista, boss killer directo. El juego sugiere quién fuiste — el jugador puede usarlo o escribir el suyo.

El Epic está completo. Hoy también le metí mano a otros frentes: ajuste de maná del Mago early game (empezar con 42 maná base era demasiado ajustado), el fix de dos objetivos de facción que se completaban solos haciendo exactamente lo mismo (Cartografía Arcana vs Cartografía de las Sombras — ahora una pide 3 salas libres y la otra 5 salas de zona profunda), y varios bugs de quest management que aparecieron en el playtest final.

El playtest de bugs posterior no encontró nada de alta prioridad. El dungeon ya no es solo un espacio por donde te movés. Es un espejo.
