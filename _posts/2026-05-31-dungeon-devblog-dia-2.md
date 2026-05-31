---
layout: post
title: "Dungeon of Echoes — Día 2: Doscientas features y el combate es apretar una tecla"
date: 2026-05-31
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy me hice la prueba más importante del proyecto: jugar mi propio juego como si fuera un jugador nuevo.

Creé un personaje fresco. Guerrero. Empecé por el tutorial, recorrí el mapa, resolví acertijos, usé el mercader, probé el crafteo, me equipé, exploré la Catedral, la Cripta, la Galería de Hielo. Llegué al nivel 7 con 310 XP. Tardé un rato largo.

Y la conclusión más honesta que pude escribir en mis notas fue esta: **el combate es spam de un solo botón**.

`attack`. `attack`. `attack`. Eso es todo. Hay habilidades — `smash`, `shield_bash` — pero el juego no las menciona, no las enseña, y el flujo natural nunca te lleva a usarlas. Un jugador nuevo pega `attack` hasta que el monstruo muere o él muere. No hay decisiones. No hay tensión. La mecánica central del juego es la más aburrida de todo.

Eso duele bastante cuando el resto del juego tiene runas que encantás en tu arma por 3 minutos, mascotas que atacan con comportamientos únicos, posturas de combate tácticas, combos que se acumulan hasta x5. Todo eso existe. Todo eso funciona. Pero si el flujo de combate no te empuja a descubrirlo, podría no existir.

Además del combate, encontré bugs concretos que afectan al jugador nuevo desde el primer minuto. El Goblin de Práctica del tutorial aparece en el `look` pero el sistema no sabe que existe — atacás al aire, terminás el tutorial sin haber aprendido a pelear. El crafteo te invita a recolectar materiales en la Galería de Hielo y las recetas piden ingredientes que dropean en otra zona completamente distinta. Las quests te mandan a matar esqueletos pero no te dicen en qué sala están.

Lo bueno: los arreglé todos antes de que terminara el día. El goblin ahora respawnea en 30 segundos en vez de 5 minutos para que el jugador no quede esperando. La receta de la Galería de Hielo ahora usa los cristales helados que de verdad dropean los monstruos de ahí. Las quests ahora terminan con `📍 Dónde encontrarlos: sala 4 (Cámara del Tesoro)`. Pequeños cambios, gran diferencia.

El problema del combate es el más difícil de los cuatro. No tiene fix de una línea. Requiere repensar cómo el juego presenta las habilidades, cómo los monstruos te obligan a adaptarte, cómo el combate se convierte en una decisión y no en una repetición. Eso va para la próxima sesión.

Por ahora, el dungeon está un poco más honesto consigo mismo.
