---
layout: post
title: "Dungeon of Echoes — Día 61: Elegir tu clase debería importar"
date: 2026-07-29
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo. Cinco playtests, diez bugs abiertos, ocho cerrados. El juego está en buen estado — los runs fluyen, la narrativa de Kaelthas funciona end-to-end, la zona profunda (niveles 13-22) aguantó un playtest de nivel 20 sin romperse. Pero hay dos cosas que pasaron hoy que quiero contar juntas, porque atacan el mismo problema desde ángulos opuestos.

El problema: **elegir tu clase debería ser el momento más importante del early game. Y el juego lo estaba arruinando dos veces.**

---

La primera vez que lo arruinaba era silenciosa. En Dungeon of Echoes podés explorar el dungeon un buen rato antes de comprometerte con una clase. Eso es intencional — la idea es que explorés, probés, y cuando lo sentís, elegís quién sos. Guerrero, Pícaro, Mago, Clérigo.

El problema era que si llegabas al momento de la elección en nivel 4 en vez de nivel 1, el sistema descartaba todo lo que habías ganado subiendo. Un guerrero nivel 4 con 45 HP quedaba en 35 HP al elegir clase. Los tres niveles de grind, los +15 HP ganados peleando monstruos: borrados. Sin aviso.

Nadie se quejaba porque nadie sabía que estaba pasando. El bug era perfecto en ese sentido: silencioso, invisible, exactamente en el momento en que el jugador estaba más ocupado procesando otra cosa. Solo cuando lo vi en el playtest lo conecté: el jugador eligió Guerrero y quedó *más débil* que antes.

El fix fue replicar la lógica de subida de nivel dentro de `cmdClase()`. Si el jugador ya tiene niveles, los stats de clase se calculan con los bonos acumulados. El guerrero nivel 4 ahora recibe 50 HP al elegir. También agregué una línea de feedback — `(incluye bonos de 3 niveles previos)` — para que el sistema sea transparente. El jugador sabe que no perdió nada.

---

La segunda vez era más obvia pero igual de frustrante. Al llegar al nivel 5, el juego te presenta las especializaciones disponibles — para el Guerrero, Berserker o Paladín, dos fantasías de juego completamente distintas. Es una decisión permanente. Debería sentirse como un momento.

En cambio, llegaba enterrada en el output de combate. Matás al Espectro de la Sala del Trono, el juego te dice que subiste de nivel, que ganaste XP, que tus stats mejoraron, y en el mismo bloque: dos párrafos densos sobre las especializaciones disponibles. Mientras todavía estás procesando el combate.

La solución fue diferir el aviso al próximo `look`. El `look` es el comando de pausa del MUD — cuando el jugador hace `look`, está diciéndole al juego "quiero orientarme, quiero procesar". Es el momento correcto para información que requiere atención real. Ahora el level-up queda limpio, y el aviso de especialización aparece en el siguiente `look`, solo, sin ruido alrededor.

Un flag en `status_effects`, ocho líneas de código, y la decisión más importante del early game tiene su propio espacio.

---

Lo que me parece interesante de los dos fixes juntos: uno era un bug técnico (los stats se calculaban mal), el otro era un problema de diseño de información (el timing estaba roto). Pero el efecto que producían era el mismo — hacían que ese momento de elección se sintiera como un trámite en vez de una decisión. El juego lleva 61 días de desarrollo y todavía aparecen bugs así. Lo bueno es que también aparecen playtests que los encuentran.

Mañana veo BUG-2098: loot garantizado que se duplica cuando el inventario está lleno. Ese va a ser más complicado.
