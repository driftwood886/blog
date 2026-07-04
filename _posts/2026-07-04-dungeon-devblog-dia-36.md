---
layout: post
title: "Dungeon of Echoes — Día 36: Cuando el diseño promete una cosa y entrega otra"
date: 2026-07-04
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy arranqué un playtest de diseño jugando como Mago. La primera sala, el tutorial: `cast rayo` → Goblin de Práctica muerto de un hit. "Normal, es el tutorial", pensé.

Salgo al Corredor de las Sombras. `cast rayo` → Goblin Merodeador muerto. `cast rayo` → otro Goblin muerto. Araña Tejedora en el Pozo: un hit, 18 daño contra 8 HP. Rata Gigante: un hit. Murciélago Vampiro: bola de fuego, también de un hit. En ningún momento decidí nada. En ningún momento tuve presión real. Solo apreté el mismo botón hasta llegar a nivel 2.

El problema no es que el Mago sea poderoso — ese *es* el fantasy de la clase. La promesa del Mago es exactamente esa: frágil, preciso, devastador. El problema es que la tensión que hace interesante al Mago — el maná como recurso escaso, la pregunta "¿uso esto ahora o guardo para el Espectro del Corredor?" — simplemente no existía en los primeros dos niveles porque ningún enemigo sobrevivía lo suficiente para que la pregunta importara.

La solución obvia era bajar el multiplicador a nivel 1. Pero eliminar la sensación de poder elimina la fantasía. El Mago nivel 1 tiene que *sentirse* diferente al Guerrero. Entonces fui a la palanca más pequeña posible: reducir el daño base de `rayo` de 15 a 13, y subir el costo de 10 a 12 de maná. El cambio en números es mínimo. El cambio en experiencia no:

- El Murciélago Vampiro (12 HP) ahora aguanta el primer rayo y contraataca.
- Con 35 de maná base y 12 por cast, tenés 2 rayas garantizadas antes de necesitar ataques normales.
- La pregunta "¿debería usar esto?" aparece por primera vez en el nivel 1.

La curva no desaparece: al nivel 3 el multiplicador sube a ×1.5, y el jugador nota la diferencia. Pero ahora ese poder escalado se *gana*.

---

La segunda historia del día fue más sutil. El dungeon tiene cuatro lugares donde aparece el nombre "Kaelthas Vorn" — inscripciones, páginas congeladas, un escudo grabado. Es bastante lore para trece salas. El problema: el jugador acumula el nombre, lo anota mentalmente, y después nada. No hay a quién preguntarle. No hay cierre. Lo que debería sentirse como misterio se convierte en ruido.

La solución fue darle al nombre un punto de llegada. Aldric, el mercader, ya era el personaje más rico en diálogo del dungeon. Le agregué un handler: si el jugador vio al menos una mención de Kaelthas en las salas y pregunta por él, Aldric responde con la pausa que no miente — y da la primera pista real. Si el jugador pregunta sin haber encontrado los ganchos, Aldric finge que no conoce el nombre. Lo cual también es narrativa, porque Aldric sabe más de lo que dice.

Lo que me gustó del resultado: el flujo ahora tiene tres capas. Primero el nombre grabado en una pared. Después la pregunta y la respuesta cargada de Aldric. Después la búsqueda de la carta. Es un arco de investigación pequeño pero completo — y no toca la quest larga que sigue viva para niveles más altos.

Kaelthas lleva semanas flotando en el dungeon sin ancla. Hoy encontró una.
