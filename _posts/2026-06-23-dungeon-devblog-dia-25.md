---
layout: post
title: "Dungeon of Echoes — Día 25: El dungeon que aprende a respirar"
date: 2026-06-23
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy tuve dos realizaciones que en el fondo son la misma: el juego le estaba mintiendo al jugador. Una vez con el mundo, otra con los números.

## El dungeon estático

El problema lo sabía desde hace tiempo pero no lo había atacado: cada partida de Dungeon of Echoes empezaba exactamente igual. Los mismos monstruos, en los mismos lugares, con los mismos stats. El sistema de eventos globales existía — `worldEvents.js` tiene código de hace meses, con "Luna de Sangre", "Invasión", "Niebla Espesa" — pero los eventos estaban mal diseñados y no se sentían.

El bloodmoon viejo hacía +2 de daño plano. Para un Goblin (ATK 3) eso es irrelevante. Para el Lich Anciano (ATK ~18) eso lo lleva a 20, que sigue siendo razonable. Un flat bonus que no escala es inútil en el early y benigno en el late — la peor combinación posible para un evento que se supone que cambia el mundo.

Rediseñé los dos eventos centrales del sistema con una pregunta simple: ¿qué hace que un jugador *quiera* que salga un evento, sabiendo que el riesgo es real?

**Luna de Sangre (🌑)**: solo afecta monstruos de nivel 3 en adelante. Los mobs del early game quedan intactos — la Rata, la Araña, el Murciélago. Pero el Espectro del Corredor, el Gólem de Piedra, el Lich Anciano ganan +30% de ATK proporcional... y dan +75% de XP. Un jugador de nivel 5 puede *querer* que salga la Luna de Sangre para farmear más eficientemente, sabiendo que los bosses van a doler más.

**Carga Arcana (⚡)**: hechizos +50% daño. Un Mago en Carga Arcana con rayo hace ~33 de daño base vs los ~22 normales. Es el momento de consumir esa poción de maná que venías guardando.

Y un detalle pequeño que me importa más de lo que parece: `look` ahora muestra el evento activo al final de la descripción de sala. Cuando entrés a la Galería de Hielo y veas "⚡ Carga Arcana — Una onda arcana recorre el dungeon... (⏱ 6m 43s restantes)", vas a saber que ese es el momento.

## La quest que nunca avanzaba

Más tarde en el día, durante un playtest, el Guerrero acumuló unos 56g entre combate, saqueo y ventas. La quest "Acumulador de Riquezas" pedía 50g. El marcador decía 0/50.

El sistema de tracking guardaba un acumulador separado (`goldEarned`, distinto del gold actual del jugador) — la lógica estaba bien. El problema era cuántas veces se llamaba `recordProgress('gold')`: exactamente una. En `cmdPick`, el comando para recoger una moneda individual del suelo.

El juego tiene cuatro formas de ganar oro: recoger una moneda, recoger todo del suelo, saquear al matar un monstruo, y vender en la tienda. Solo la primera registraba progreso. El jugador que usa `loot` o vende ítems — o sea, cualquier jugador que juega normalmente — progresaba silenciosamente hacia cero.

Fix mecánico, rápido. Pero la quest existe hace semanas. Todo ese tiempo mirando 0/50 sin importar cuánto oro ganaras.

Un bug que solo miraba una puerta de cuatro. Clásico.

---

Con esto el dungeon tiene por primera vez un "pulso" — algo que pasa sin que el jugador lo inicie. Quedan dos fases del sistema de eventos épico: NPCs itinerantes y eventos narrativos desencadenados por acciones (el Lich cae → XP global por 15 min). Eso va a transformar el dungeon de tablero a lugar.
