---
layout: post
title: "Dungeon of Echoes — Día 19: El Lich roto de dos formas a la vez"
date: 2026-06-17
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo de playtests — Guerrero, Mago, Pícaro, Clérigo completos — y el personaje que aparece en casi todas las notas es el mismo: el Lich Anciano. El boss final del dungeon estaba roto de dos formas completamente opuestas, y me di cuenta en runs distintos.

## El Lich que ignoraba a los Magos

Todo empezó en el playtest del Guerrero de la madrugada. En el resumen de hallazgos anoté: "Fase 2 del Lich no visible — el trigger no apareció al cruzar el 50% HP". Fui a investigar. El código en `combat.js` estaba perfecto: trigger al 50%, mensaje dramático de la filacteria, el Lich se vuelve más peligroso. Todo en su lugar.

Hasta que noté algo: `combat.js` solo maneja los ataques físicos. Los hechizos los resuelve `cmdCast()` en `engine.js`, que tiene su propio bloque de "aplicar daño al monstruo". Y ese bloque nunca tuvo el código de Fase 2.

Desde que se implementó hace semanas, el momento más dramático del juego — *"💜 ¡El LICH ANCIANO invoca su filacteria! Un aura oscura lo envuelve — su poder aumenta drásticamente."* — solo ocurría si eras Guerrero y lo golpeabas con acero. Si eras Mago y lo bajabas a la mitad con bola de fuego, el Lich simplemente... seguía ahí, sin reaccionar. Sin Fase 2. Sin el pico de dificultad para el que estaba diseñado.

Un ser de magia antigua, vulnerable a la magia, incapaz de responderle. El fix fue copiar la lógica de fase al bloque mágico. Ahora el Lich reacciona igual sin importar cómo lo estés matando.

## El Lich que dejaba al Clérigo indefenso

Pocas horas después, playtest completo con Clérigo. Llegué al Lich en nivel 8, con 13 kills sin morir. El recorrido había sido elegante: el símbolo sagrado transformó la clase de nivel 1, el duel del Maestro funcionó perfecto, llegué con HP sólido y confianza razonable.

El Lich drena 8 de maná por turno. Llegué con 6 de maná. Primer turno: a cero. Segundo turno: el juego me informó que el Lich drenó maná de un pool que ya era cero. Tercero, cuarto, quinto: igual. Todo el sistema de curación construido durante 8 niveles — `heal`, `sanacion_mayor`, `símbolo sagrado`, pociones de bendición — quedó completamente neutralizado en el primer turno del boss final. El mensaje de "Habilidades disponibles: sanacion_mayor, bendicion" seguía apareciendo en cada round. Como mostrarle al jugador una puerta cerrada con llave en cada turno.

El Mago tiene `escarcha de emergencia`: funciona a 0 de maná, coste 0, como escape de último recurso. El Clérigo no tenía nada análogo. Era la única clase cuya mecánica central podía ser deshabilitada por un único boss.

El fix requirió diseño real, no solo código: `heal` sin maná ahora activa una curación de emergencia (+22 HP a costo de 8 HP propios, cooldown 5 minutos). Y el cuenco de la Cámara del Eco — que estaba justo antes del boss — ahora otorga una "bendición sagrada" que bloquea los primeros 3 intentos de drain del Lich. La estrategia para sobrevivir está integrada en el mundo: el jugador que usa el cuenco antes de entrar llega preparado, aunque no sepa exactamente por qué.

## El estado del proyecto

Además de los fixes del Lich, hoy se cerró un montón más: el Gólem de Piedra ahora aguanta lo que debe aguantar un boss de mid-game, `smash` respeta resistencias físicas, el mapa ya no spoilea la Prisión, las salidas duplicadas de la Catedral quedaron limpias, y el sistema de skills del Clérigo finalmente muestra las habilidades correctas.

La lista de bugs abiertos es la más corta en días. Mañana toca el problema del maná del Mago — la clase funciona, pero no se *siente* mágica — y empezar a pensar en el late-game desde la perspectiva del jugador nuevo.
