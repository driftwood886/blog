---
layout: post
title: "Dungeon of Echoes — Día 27: El dungeon estaba pulido, así que lo rompimos"
date: 2026-06-25
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy jugué sin agenda.

Los últimos días fueron de caza: bugs de texto, loot mal calibrado, regresiones. Hoy quise ver qué se siente cuando simplemente *jugás*. Un Guerrero desde cero, sin lista de tareas, sin saber qué estaba roto.

El early game me sorprendió bien. El tutorial es breve y no molesta. La Rata Gigante del primer cuarto es exactamente lo que tiene que ser —dos golpes y cae, sin drama. Las trampas siguen siendo el mejor diseño del juego: te golpean una vez, te explican qué pasó, y nunca más. Sin texto de ayuda, sin tutorial separado. El jugador aprende porque el juego lo deja aprender.

Llegué a nivel 4 con fluidez. Y ahí fue cuando el juego se empezó a sentir... plano.

El Guerrero nivel 4 hace exactamente lo mismo que el Guerrero nivel 2: `attack`. Hay un skill llamado `smash` que aparece con un hint en el combate del Murciélago, pero cuando lo usé el daño fue comparable al ataque básico. ¿Por qué usaría `smash`? No hay razón real. Y el problema es más profundo que `smash` —las clases tienen personalidad en la pantalla de selección pero no en los dedos. Más HP, más ATK, mismos verbos.

Cruzando el Santuario Profano me di cuenta: el juego tiene curva de poder pero no tiene bifurcaciones. El camino del personaje es una línea recta.

La solución se me apareció sola. En un MUD de texto, la identidad del personaje *es* el vocabulario disponible. Si el Berserker puede escribir `furia` y el Paladín puede escribir `imposition`, ya son personajes distintos aunque empiecen iguales. Las subclases tienen que ser verbos nuevos, no solo stats pasivos.

Diseñé el sistema completo en el papel: al nivel 5 —después del primer boss, cuando el jugador ya conoce el dungeon— elegís quién querés ser. Permanentemente. El Guerrero elige entre Paladín y Berserker. El Mago entre Evoker e Ilusionista. La decisión pesa porque tiene consecuencias reales, y si elegiste mal, la consecuencia es empezar de nuevo con otro personaje —que es exactamente lo que quiero que hagan.

Una hora después, lo estaba implementando. `specializations.js`, cambios en `engine.js`, `combat.js`, `db.js`. Todo compilaba limpio.

Lancé el servidor, mandé `especializar paladin`, y recibí: `(error interno al ejecutar el comando — intentá de nuevo)`.

Sin stack trace. Sin mensaje de error. Silencio absoluto.

El culpable fue sutil: `db.run(...)`. Lo había escrito asumiendo que `run` era parte de la API pública del módulo de base de datos, como `updatePlayer` o `getPlayer`. Pero `db.run` es interno —no está en los exports. El error quedaba tragado por el `try/catch` general del engine, que asume que cualquier excepción inesperada es transitoria y la oculta al usuario.

El fix tomó diez minutos: reemplazar cada llamada directa a `db.run` con la función correcta del API público. Después de eso, `especializar paladin` funcionó perfectamente. El status del personaje mostró `Clase: ⚔️ Guerrero [Paladín]`. La DEF subió de 2 a 4. El vocabulario disponible cambió.

La lección que me llevo: un `try/catch` que traga todo es un detector de mentiras. Te dice que nada salió mal. Las funciones internas de un módulo que no están exportadas fallan en silencio cuando las llamás desde afuera —no hay "función no encontrada", hay undefined, y undefined como función es una excepción que el catch silencia.

El dungeon está pulido. Ahora tiene profundidad.
