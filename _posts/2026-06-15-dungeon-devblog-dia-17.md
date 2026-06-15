---
layout: post
title: "Dungeon of Echoes — Día 17: Siete caracteres que destruyeron a toda una clase"
date: 2026-06-15
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice el primer playtest completo del dungeon jugando exclusivamente como Mago. De punta a punta: tutorial, zonas intermedias, bosses, crafteo, economía. Lo que encontré fue una clase que tenía todos los ingredientes para ser brillante... y un bug que la hacía fundamentalmente injugable.

El Mago tiene un multiplicador de daño de 1.5x sobre el físico. En papel es la clase más satisfactoria del juego: llega una araña, la fulminás de un hechizo, seguís. El problema es que cada vez que eso pasaba —cada vez que un hechizo mataba de un golpe— el turno siguiente tiraba "error interno al ejecutar el comando". No importaba qué hicieras después. Attack, look, inventario: todo fallaba ese turno. El juego se recuperaba, pero ya perdiste una acción. En un combate ajustado contra el Guardia Espectral con 8HP, esa acción perdida es la diferencia entre vivir y morir.

Lo más irónico: el bug afectaba *exactamente* el momento más satisfactorio que puede tener un Mago. Cuatro monstruos distintos, mismo patrón. La clase entera rota, en su mejor momento.

Fui a buscar el bug en `cmdCast()`. El código que maneja la muerte de un monstruo es largo —XP, drops, achievements, contratos semanales, un montón de lógica acumulada. Pero ahí, en el medio, encontré esto:

```js
// Fix de BUG-336: usar combat.dropLoot().\n      const { droppedLoot: castLoot } = combat.dropLoot(target, ...);
```

¿Ven ese `\n`? En el archivo fuente es literalmente un backslash y una n. No un salto de línea real. Son dos caracteres que *dicen* "salto de línea" pero no lo son. El resultado: `const { droppedLoot: castLoot }` quedaba dentro del comentario `//`. JavaScript lo ignoraba. `castLoot` nunca se declaraba. Cuando más abajo el código lo usaba, Node tiraba `ReferenceError`. El servidor lo atrapaba y devolvía el genérico "error interno".

Siete caracteres. Eso bloqueó a toda la clase Mago, en su caso de uso más frecuente, desde el momento en que alguien escribió ese comentario hace quién sabe cuánto.

El fix fue separar el comentario de la declaración. Tres líneas de commit.

---

Pero BUG-553 era solo el principio. Con el bug resuelto, tuve que enfrentar lo que el playtest también había revelado: el Mago como clase estaba económicamente broken de una forma diferente. Aldric, el mercader del dungeon, vendía exactamente las mismas cosas para todas las clases. El Guerrero necesita pociones de salud. El Mago necesita pociones de salud *y* pociones de maná. Dos gastos donde hay uno. La tienda no tenía ni un ítem pensado para él.

Hoy cerré cinco tareas de clase: la poción de maná bajó de 20g a 12g, Aldric ahora vende vara de energía y pergamino de hechizo, hay tres recetas de crafteo exclusivas para el Mago, los ítems mágicos dan mensajes de flavor al equiparlos, y los bosses físicos/pétreos tienen resistencia mágica del 65% —para que el multiplicador 1.5x del Mago no trivialice cada combate importante.

El pergamino de hechizo es el ítem que más me gusta: cuando tenés 2 puntos de maná y el Gólem todavía tiene 20HP, ese pergamino de 25g es la diferencia entre ganar y morir. Decisiones claras, consecuencias reales.

El Mago ya es una clase que vale la pena jugar.
