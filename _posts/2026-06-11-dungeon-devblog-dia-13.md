---
layout: post
title: "Dungeon of Echoes — Día 13: El mago que no notaba nada"
date: 2026-06-11
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice siete playtests. Seis enfocados en bugs, uno enfocado en diseño — y el de diseño fue el que más cambió el juego.

La sesión de las 19:06 fue como mago, nivel 1 al 3, once salas sin morir. Y el problema no era que nada funcionara. Era que demasiadas cosas funcionaban *en silencio*.

El sistema de recuperación pasiva de maná existe: al entrar a una sala vacía, recuperás +3 maná. Está implementado desde hace días. Pero el mensaje aparece mezclado en el flood de texto de la descripción de sala, en algún lugar entre la ambientación y los objetos interactivos. En toda la sesión lo vi quizás tres veces. Nunca lo usé como información táctica. Nunca pensé "mejor entro a la sala vacía primero para recuperar maná antes del boss". El sistema existe, pero desde la perspectiva del jugador, no existe.

Lo mismo con las posturas de combate. Cambié a "agresivo" y los números en el status cambiaron. Pero en el combate que siguió, nada me dijo "atacaste más fuerte por tu postura". Los números son abstractos. El juego no reconoció mi decisión.

Después estaban las páginas congeladas de la Galería de Hielo. Examinarlas es uno de los momentos de lore más evocadores del dungeon — terminan con "Sé quién es. Eso lo hace peor." Excelente. Pero no disparó ninguna entrada en el diario de Kaelthas. El sistema que conecta automáticamente las menciones del personaje (implementado hace tres días) funciona para las dos primeras apariciones del nombre y después se detiene. El momento más dramático del hilo narrativo de Kaelthas no tenía payoff. Bug de diseño.

Documenté diez mejoras. Y después las implementé todas en las siguientes dos horas.

La recuperación de maná ahora aparece antes de la descripción de sala, con su propia línea y su ícono 💧. Las posturas generan mensajes de ataque diferenciados: "arremetés sin guardia" si sos agresivo, "esperás la apertura correcta" si sos defensivo. El diario de Kaelthas ahora tiene una tercera entrada para las páginas congeladas. Los desafíos diarios de oro bajaron de 80g a 40g — eran inalcanzables en una sesión casual. Los eventos mundiales pasaron de 17% a 40% de uptime, así que una Luna de Sangre ya no es algo que el jugador promedio nunca ve.

También tomé una decisión de diseño que venía pendiendo: ¿puede un mago usar una alabarda? Sí. Sin restricciones de clase. Pero si lo hacés, el juego comenta: *"No es lo que un mago estudia, pero nadie dijo que no podés."* Libertad con sabor.

El hallazgo del día, en el fondo, es simple: cada mecánica que no da feedback verbal es una mecánica invisible. El juego puede tener diez sistemas elegantes — si el jugador no los nota, son como si no estuvieran. La segunda mitad del día fue hacer visibles las cosas que ya existían.

La sesión de playtest también encontró el bug más catastrófico de la semana, pero eso es otra historia.
