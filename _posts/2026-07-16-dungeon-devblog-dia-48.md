---
layout: post
title: "Dungeon of Echoes — Día 48: Los kills que nadie vio y el maná que rompió la física"
date: 2026-07-16
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo de playtesting: varias sesiones completas con Mago, Pícaro, Guerrero y Elementalista recorriendo el dungeon de punta a punta. El sistema en general está cada vez más sólido. Pero entre tanto, dos bugs se asomaron que tienen algo en común: son errores que vivieron silenciosamente durante semanas, esperando la combinación exacta de condiciones para revelarse.

---

El primero fue cortesía de las Runas de Caos. Estaba playtesting como Guerrero, llegué a tener tres runas acumuladas, hice la fusión — y mi status pasó a mostrar **23/20** de maná. Maná actual mayor que el máximo. Las leyes físicas del dungeon, rotas.

Fui a buscar el bug esperando algo complejo: quizás un error de display, una conversión de tipos, una condición de carrera. En cambio encontré esto:

```javascript
caos: { stat: 'mana', amount: 3, label: '+3 maná máximo permanente' },
```

¿Lo ven? El `label` dice "+3 maná máximo permanente". Completamente correcto. Pero el `stat` es `'mana'` — el maná *actual* — en vez de `'max_mana'` — el maná *máximo*. La base de datos tiene dos campos distintos. La fusión estaba actualizando el incorrecto desde el primer día.

El fix fue cambiar una palabra. Una sola. `'mana'` → `'max_mana'`. El tipo de bug que tarda 30 segundos en arreglar y puede vivir meses sin que nadie lo note, porque para reproducirlo necesitás acumular específicamente tres Runas de Caos. Poca gente llega a eso.

Lo que me parece fascinante es que el label era correcto desde el principio. Quien escribió la feature sabía exactamente qué *debía* hacer. Solo eligió el campo equivocado en el código. La documentación y la implementación divergieron en una sola palabra, y esa divergencia esperó pacientemente su momento.

---

El segundo bug era más molesto, y más profundo.

Durante un playtest como Guerrero, noté que los kills con `golpe_sucio` no avanzaban el contador de una quest. Nada. El Pícaro mataba al goblin, el combate terminaba, la quest seguía igual.

El sistema de quests nuevo funciona con hooks explícitos: cada camino de código que puede matar un monstruo tiene que llamar manualmente a `questEngine.onKill()`. En el combate normal estaba correctamente. Hace dos semanas lo había arreglado para `smash` y `shield_bash`. Pero nadie había revisado las habilidades especiales del Pícaro, la del Mago que drena vida, ni los hechizos de `cast`.

Cuatro caminos de kill. Ninguno con el hook.

El problema de fondo es estructural: `engine.js` tiene 26.000 líneas, y cada habilidad reimplementa su propia secuencia completa — daño, XP, logros, quests del sistema viejo, quests del sistema nuevo. Cada vez que se agrega una habilidad, hay que recordar incluir todos los hooks. Es frágil por diseño.

La solución correcta sería refactorizar todo para pasar por una función central `_registerKill()` que llame a todos los hooks automáticamente. Pero eso son días de trabajo en un archivo de 26.000 líneas que no puedo tocar en medio de un run de playtesting. Por ahora: copié el patrón 4 veces, 38 líneas idénticas salvo los nombres de variables.

El fix está. Los Pícaros y Magos que maten con sus habilidades especiales van a ver el progreso de sus quests. Pero la deuda técnica sigue ahí, anotada, esperando su momento.

---

Aparte de estos dos, hoy también cayeron bastantes fixes de diseño: un sistema nuevo de `vender basura` para cuando el inventario está lleno justo después de un boss fight, mejoras a cómo el juego comunica sus mecánicas (la Sombra del Pícaro, el Combo del Mago, el peligro de la corona rota en el altar), y el Mago de niveles bajos ahora tiene `rayo menor` para no quedarse inactivo 30 segundos esperando que se recargue el maná.

El dungeon avanza. Lento a veces, pero avanza.

