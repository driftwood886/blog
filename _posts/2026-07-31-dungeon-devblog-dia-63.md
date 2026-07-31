---
layout: post
title: "Dungeon of Echoes — Día 63: Lo que el sistema sabía pero no contaba"
date: 2026-07-31
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día raro en el sentido más satisfactorio: casi todo lo que arreglé no era un bug de lógica. Era un bug de comunicación.

El playtest de la mañana dejó una nota sobre la postura de combate. El sistema de posturas lleva semanas funcionando bien —cambiar a `postura agresivo` modifica correctamente ATK y DEF, el combate lo refleja, los números cuadran. Pero la nota decía algo incómodo: *"el jugador no sabe que está pasando"*.

El problema era que cuando alguien escribía `postura agresivo`, el juego respondía con una confirmación genérica. Nada más. Los stats cambiaban silenciosamente en el backend. Si después consultabas `status`, veías:

```
⚔️ Postura: agresivo
```

Y nada más. Sin modificadores. Tenías que acordarte de memoria que eso significa +2 ATK, -1 DEF, +5% de miss. Para quien juega hace días, bien. Para alguien explorando el sistema, era pared de silencio.

El fix era doble y tomó poco tiempo: `cmdStance` ahora muestra el efecto inmediato en números (`ATK 16 → 18 | DEF 6 → 5`), y `status` lista los modificadores activos junto a la postura. El sistema ya tenía toda esa información disponible —solo había que decidir decirla.

Hay algo que me resulta filosóficamente interesante en esto. En un juego de texto, donde todo el feedback es texto, un bug de comunicación es igual de grave que un bug de lógica. Peor, tal vez, porque es más difícil de detectar: los tests pasan, el código hace lo correcto, pero el jugador queda ciego. Pierde el modelo mental del juego. Eventualmente deja de confiar en que sus acciones tienen efecto.

---

La otra historia del día también es sobre un sistema que castigaba sin querer.

La Orden del Filo tiene una misión clásica: matá 8 criaturas en postura agresiva. El enunciado decía "desde que te uniste" —lo cual tenía sentido como aclaración técnica. El problema: si llegabas al juego peleando agresivo desde el inicio, ya tenías 5 kills antes de decidir unirte. El momento en que firmabas con la Orden, tu contador volvía a cero.

Le estábamos diciendo al jugador: *tu esfuerzo anterior no cuenta*. Llegaste peleando como uno de los nuestros antes de saber que lo eras, y aun así empezás desde cero.

La solución fue rastrear `ses_agresivo_kills` en jugadores sin facción durante la sesión —un contador efímero que solo existe mientras no estás en ninguna facción. Al unirte a la Orden, ese contador se aplica retroactivamente al progreso de la misión (con tope, para que no la completes instantáneamente si tenías 40 kills). El mensaje de bienvenida ahora dice: *"⚡ Crédito retroactivo: tus 5 kills en postura agresiva ya cuentan. Progreso: 5/8."*

Antes la frase de bienvenida decía "los kills previos no cuentan — el contador empieza desde cero". Era honesto. Pero desmotivador de una manera que no tenía ningún propósito de diseño. La Orden del Filo ya te reconocía como miembro por cómo peleabas —lo único que faltaba era que el sistema también lo dijera.

---

Además del resto: un bug en el leaderboard in-game (siempre vacío por un doble filtro de bots que se contradecían entre sí), la corona rota que podías vender a Aldric perdiendo para siempre tu acceso a la trampa del Trono, y seis mejoras de QoL que vaciaron la pila de deuda de diseño. Un día completo.

El dungeon está cerca de sentirse terminado en el sentido funcional. Lo que queda es el pulido fino —ese 20% que el jugador siente y que no se puede patchear.
