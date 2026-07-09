---
layout: post
title: "Dungeon of Echoes — Día 41: cuando morir no duele (y cuando duele demasiado)"
date: 2026-07-09
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hubo un playtest que me dejó con dos problemas opuestos en la misma sesión.

El run era un Pícaro Asesino, nivel 1 hasta el fondo del dungeon. Todo fue bien hasta el Abismo Eterno. La Sombra del Vacío —el boss final— lo mató en exactamente cinco turnos. No es que el jugador hubiera jugado mal: tenía el equipamiento correcto para su nivel, había llegado sin muertes previas, conocía los patrones. La parálisis del primer turno cayó garantizada (eso está diseñado así, es la firma del boss). Luego tuvo un 35% de chance de parálisis en cada turno siguiente. Cayó dos veces más. En tres de los cinco turnos, el Pícaro no pudo atacar. El boss ganó sin sudar.

El problema con un boss así es que no da la sensación de "casi lo logro, si hubiera jugado diferente". Da la sensación de "no había nada que hacer". Son cosas muy distintas.

Bajé la probabilidad de parálisis aleatoria de 35% a 25% y reduje el bonus de ataque en Fase 2 de +4 a +3. Cambios conservadores. No trivializan el fight —el primer turno sigue siendo parálisis garantizada y la Fase 2 sigue siendo un salto real—, pero ahora hay un par de turnos extra de ventana donde la skill del jugador puede hacer diferencia.

También corregí algo narrativamente incómodo: si el Pícaro acumulaba sus tres puntos de sombra para el Golpe desde las Sombras y después la parálisis caía, perdía todos los puntos sin hacer daño. El mensaje decía que "la oscuridad del boss absorbió tus sombras". Mecánicamente frustrante. Lógicamente raro. Ahora, si el jugador tiene el Golpe activo, la parálisis no puede activarse ese turno. Las energías de sombra chocan y se repelen. Oscuridades en conflicto, no una tragándose a la otra. El mensaje lo explica. Y el boss sube el nivel recomendado de 7+ a 8+ en todos los hints del juego —porque el salto de dificultad era real y pretender lo contrario era deshonesto.

---

Pero esto me dejó pensando en el otro extremo del problema.

La Sombra del Vacío mataba demasiado fácil. Pero ¿importaba que te matara? En realidad, no mucho. Morías, reaparecías en la Entrada, y todo seguía igual: mismo inventario, mismo oro, mismos niveles. La única consecuencia era perder la racha de combo. El dungeon te miraba morir y te decía "bueno, de nuevo entonces".

Sin consecuencias reales, el combate pierde peso. Un enfrentamiento no genera tensión cuando el peor resultado posible es... reaparecer y volver a intentarlo.

Había cuatro opciones sobre la mesa: pérdida de oro recuperable, penalización temporal de stats, loot cayendo al suelo, o pérdida de XP. Descarté la pérdida de XP rápido —es la penalización más frustrante psicológicamente y la que más rompe la sensación de progreso. La penalización de stats tiene el problema de ser invisible. El loot en el suelo ya existe con los restos de aventureros anteriores.

Elegí el oro recuperable.

Al morir con más de 20 monedas, perdés el 15% de tu fortuna, con un mínimo de 5g y un techo de 50g. Ese oro no desaparece: cae como una bolsa en la sala donde moriste. Si volvés, la encontrás.

El umbral de 20g es para no hundir más a alguien que ya gastó todo en equipo. El techo de 50g es para no convertir una muerte en una catástrofe para jugadores avanzados. Lo que me interesó del resultado es cómo cambia la narrativa con muy poco código. Ahora el mensaje dice: *"💸 Al caer, derramás tu bolsa. Perdiste 15g — quedaron en Sala de los Ecos. Podés recuperarlas volviendo a esa sala."*

Eso convierte una muerte en una decisión: ¿vale la pena volver a esa sala llena de monstruos para buscar 15 monedas de oro?

A veces sí. A veces no. Eso ya es diseño.

El día terminó con varios playtests más, bastantes bugs resueltos y el sistema de hints contextual implementado para jugadores que se quedan dando vueltas sin saber qué hacer. El juego se siente considerablemente más sólido que ayer. Pero las dos historias que me quedan resonando son estas: el boss que era imbatible, y la muerte que no tenía dientes. Ambas tenían la misma raíz —el juego no estaba calibrado para que las decisiones del jugador importaran de verdad. Hoy importan un poco más.
