---
layout: post
title: "Dungeon of Echoes — Día 22: El Pícaro que Vació el Dungeon Sin Rasguños"
date: 2026-06-20
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de mucho playtest. Guerreros, Magos, Clérigos — todo el dungeon pasado varias veces para verificar que los fixes de los últimos días seguían en pie. Pero la historia del día se la lleva el Pícaro.

Arranqué una sesión de diseño como Pícaro nuevo. Tutorial, cuero endurecido, cuchillo envenenado crafteado con veneno concentrado. Salí al dungeon con toda la confianza.

Y el dungeon me dejó entrar sin pestañear.

15 kills. 0 muertes. Máximo 12 HP de daño total en toda la sesión — recibidos del Campeón Espectral, el único boss que logró rozarme. El Lich Anciano, el jefe final, cayó en 4 turnos. Solo tuvo oportunidad de atacarme una vez. Erró.

La mecánica de sigilo es simple: entrás invisible, el primer ataque es crítico garantizado y el monstruo no puede responder ese turno. Para bosses ya habíamos bajado el multiplicador de ×2 a ×1.5. Pensábamos que era suficiente. Los números decían otra cosa: lanza espectral del eco (ATK 21 efectivo), sigilo (×1.5 = 31 daño), golpe sucio en el segundo turno (23 daño + veneno), el veneno drena antes del siguiente contraataque. El Lich pasa de 90 HP a muerto antes de procesar lo que pasó.

El problema no era el daño del sigilo aislado. Era el combo: **turno gratis** de daño sin respuesta, seguido de golpe sucio, seguido de 20% de esquiva como red de seguridad. Los bosses matemáticamente no tienen cómo matar al Pícaro si no pueden atacar el primer turno y encima tienen que adivinar si el golpe conecta.

Estuve un rato pensando cómo romper el combo sin destruir la clase. No quería castigar al Pícaro por jugar bien — el sigilo tiene que seguir siendo valioso. La solución que encontré fue conceptual antes de ser mecánica: *estos bosses no pueden ser sorprendidos*. No porque el Pícaro sea débil, sino porque el Campeón Espectral, el Eco Viviente, el Lich Anciano y la Sombra del Vacío son criaturas de poder arcano que perciben tu presencia incluso en las sombras.

Mecánicamente: el crit ×1.5 aplica igual. El jugador hace daño extra. Pero el boss "rompe el sigilo" y contraataca en ese mismo turno — sin stun. El Pícaro tiene que bancarse el golpe de respuesta.

Para el Elemental de Hielo y el Krakeling (los monstruos de mid-game que también caían en 1-2 golpes) elegí un punto medio: siguen aturdidos el primer turno, pero su crit desde emboscada pasa de ×2 a ×1.7. Un tier de "élite que aguanta más" sin llegar a los bosses finales.

La tensión del diseño fue decidir qué bosses entran en la lista de inmunes al stun. El Guardia Espectral quedó afuera — es poderoso pero es mid-game, y el Pícaro debería poder manejarlo. Los cuatro inmunes son los que aparecen cuando el juego espera que tengas nivel 6+. A esa altura ya sabés lo que hacés, y el dungeon tiene que saber también.

El resultado esperado: el Pícaro sigue siendo la clase más potente en combate de precisión. El sigilo da ventaja real. Pero ya no podés completar el dungeon entero con 12 HP de daño total.

Mañana toca verificar.
