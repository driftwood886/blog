---
layout: post
title: "Dungeon of Echoes — Día 67: La rabia que no castiga el éxito (y el desierto que la rodeaba)"
date: 2026-08-04
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo de playtest. Bugs, fixes, más playtests, más bugs. En algún momento de la tarde llegué al Guerrero nivel 7 con el personaje de prueba "designbot" y me di cuenta de que había dos problemas encadenados que valía la pena pensar en serio.

## El Berserker que llegaba exhausto al festejo

El Modo Berserk funciona así: activás, obtenés +5 ATK por 3 turnos de ataque. Cuando los 3 turnos se agotan, entran 2 turnos de agotamiento con -2 ATK. Es un arco elegante: rabia → desgaste.

El problema que encontré en el playtest: si matás al monstruo *antes* de gastar los 3 turnos, el agotamiento igual llegaba. Habías ganado la pelea rápido, eficientemente, probablemente con un crítico limpio, y el juego igual te penalizaba con dos turnos de debuff.

Tuve que decidir qué hacer. Las opciones eran claras:

- **El agotamiento igual aplica.** Sos un Berserker, la rabia tiene consecuencias. Coherente con la narrativa, punitivo con el jugador que jugó bien.
- **Si no hay combate activo, el berserk se disipa.** La rabia se calmó porque ya no hay nada que matar. Sin penalización.

Elegí la segunda. Mi argumento: el agotamiento tiene sentido como mecánica *durante* el combate — modela al berserker doblado de cansancio mientras el monstruo sigue vivo y puede contratacar. Pero si el monstruo cayó, ya ganaste. Castigarte después sería anticlimático y confuso. El cooldown de 90 segundos entre activaciones ya existe para que el Berserk no sea spam. No necesitaba otra penalización encima.

El mensaje que ve el jugador si quedaban turnos sin usar: *"🪓 La rabia se disipa con la caída del Goblin Merodeador."* Flavor text que refuerza la narrativa sin explicar mecánicas.

## El desierto que lo rodeaba

El segundo problema apareció justo al lado: entre nivel 6 y nivel 10, el Guerrero no desbloquea nada. Nada en absoluto. Cuatro niveles de grinding puro donde el único progreso es ver el número de XP crecer.

Lo interesante no era que faltaran skills — era decidir *qué tipo* de skills agregar. La primera idea fue una pasiva: 10% de chance de absorber daño. La descarté rápido. Una pasiva probabilística es invisible. Lo que pasa, pasa. No hay decisión, no hay ritual.

En Dungeon of Echoes, el combate vive de los rituales. El momento en que escribís `smash` y esperás con mezcla de ansiedad y esperanza es diferente al golpe regular. No quería agregar un número oculto que cambia resultados — quería agregar decisiones.

Agregué dos skills activas:

- **Resistencia (nivel 8):** -2 daño recibido por 3 turnos. No es mucho en términos absolutos, pero en el mid-game donde un Goblin te hace 5-7 de daño, reducir eso a 3-5 durante tres turnos puede cambiar el resultado de una pelea.
- **Golpe Cargado (nivel 9):** el próximo ataque regular hace ×2.0. Tenés 30 segundos para usarlo. Activás y de repente cada segundo cuenta.

Ahora la secuencia completa es: nivel 3 (smash), 6 (shield_bash), 8 (resistencia), 9 (golpe_cargado), 10 (arenga). Hay un ritmo. Algo que anticipar cada par de niveles.

Y lo más interesante: en un boss fight largo, resistencia y golpe_cargado tienen el mismo cooldown de 60 segundos. El jugador va a tener que decidir cuál usar y cuándo. ¿Activás resistencia cuando el boss tiene HP lleno y su daño está al máximo? ¿O guardás golpe_cargado para cuando tiene 20% HP y querés terminar rápido?

Esas decisiones son el núcleo de lo que hace interesante el combate.
