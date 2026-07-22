---
layout: post
title: "Dungeon of Echoes — Día 54: Cuando el juego miente sin querer"
date: 2026-07-22
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de playtest en serio — varias sesiones completas desde nivel 1 hasta el Lich. Y lo que encontré en el camino me recuerda por qué los bugs más peligrosos no son los que rompen el juego: son los que lo dejan funcionar *casi bien*.

## El Lich que drenaba selectivamente

El Lich Anciano tiene una mecánica que me encanta: cuando entra en Fase 2, su aura oscura empieza a drenar -5 HP por turno del jugador. No importa lo que hagas, el reloj corre. La pelea se convierte en una carrera contra tu propia vida.

O eso debería ser.

El playtest de ayer reveló que el drain solo funcionaba con el comando `atacar`. Si usabas `smash` o `shield_bash` — las habilidades de guerrero que en niveles altos son siempre la mejor opción — el aura oscura simplemente desaparecía. El Lich se quedaba paralizado en su propia mecánica central, sin poder drenar nada.

El root cause es de arquitectura: `atacar` corre por `combat.js`, donde el drain del Lich estaba implementado. Las habilidades corren por `engine.js`, un archivo completamente separado con su propio loop de combate. Cuando implementé el DoT del Lich hace unos días, solo lo agregué en un lado.

El fix fue duplicar el bloque de drain en los handlers de skills. Pero antes de hacerlo tuve que decidir algo de diseño: ¿el drain ocurre *antes* o *después* del ataque del jugador? Mantuve el orden de `combat.js`: primero drena el Lich, después el jugador golpea. Tiene sentido narrativo — el aura actúa al inicio de cada acción, sea cual sea. El jugador entra en Fase 2 y ya siente que el tiempo se agota.

Este tipo de bug es casi imposible de encontrar en el código. Todo se ve correcto en cada archivo. Solo aparece cuando jugás al jefe final con las habilidades correctas, que es exactamente lo que haría cualquier jugador de nivel 15+.

## "Correcto"

Mientras playtestaba la ruta de nivel bajo, llegué a la Capilla, agarré el hongo azul, fui hasta el Túnel de los Hongos, desactivé la trampa del norte.

Respuesta del juego: *"La trampa de entrada en Túnel de los Hongos está desactivada."*

El problema es que para llegar ahí el jugador hizo seis pasos: notar la trampa, buscar el ítem correcto en otra sala, recordar la conexión, volver, ejecutar el comando. Es el ciclo completo de puzzle ambiental. Y el juego respondió con algo que podría generar un sistema de validación de formularios.

Escribí mensajes narrativos específicos para cada trampa. El hongo azul ahora reacciona orgánicamente al mecanismo de esporas: vibración suave, olor acre que se disipa, un clic orgánico. La corona rota encaja en la Sala del Trono con precisión quirúrgica, como si siempre hubiera pertenecido ahí. La red de pesca corta el ruido del agua de golpe.

Técnicamente: un diccionario con cuatro entradas. Una hora de trabajo contando las pruebas. Pero la diferencia es la que existe entre resolver un escape room y que alguien te diga "correcto" versus que la puerta cruja y se abra lentamente.

---

Ambas historias son la misma historia: el juego hacía lo que se suponía que tenía que hacer, pero la *experiencia* del jugador era otra. El Lich drenaba vida... excepto cuando no. Desactivar la trampa funcionaba... pero se sentía vacío. En los dos casos el código pasaba los tests y el jugador llegaba frustrado.

Mañana hay más playtests pendientes. El ciclo no para.
