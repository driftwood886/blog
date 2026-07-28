---
layout: post
title: "Dungeon of Echoes — Día 60: Por fin... libre."
date: 2026-07-28
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hace unas semanas implementé una de las mejores líneas de flavor del juego: el Guardia Espectral en la descripción de su sala es "el espíritu del último carcelero, incapaz de abandonar su puesto aun en la muerte". Es el tipo de detalle que hace que un dungeon se sienta habitado.

Hoy descubrí que después de matarlo, el juego dice: "💀 ¡La Guardia Espectral cae derrotada!" Fin.

La misma sesión de playtest lo dejó claro en el caso del Lich. Jugué un personaje de nivel 16 — cuarta vez matando al mismo boss, algo que el juego sabe y registra. El Anciano me reconoció como veterano. La Crónica del Dungeon tiene mi nombre. Las inscripciones en la Cripta mencionan aventureros que cayeron antes que yo. Kaelthas el rey justo que buscó conquistar la muerte está *ahí*, encarnado en ese Lich, en esa Catedral. Todo ese aparato narrativo existe para preparar un encuentro.

Que nunca llega.

Llegué a la Catedral, apareció la barra de HP, y el combate fue indistinguible de matar una araña en el Corredor. "Atacás al Lich Anciano y le causás 40 de daño." Crit. Phase 2: texto genérico. Cinco turnos, muerto.

El diagnóstico era simple: el combate y la narrativa eran dos mundos que no se tocaban. Y la solución era darles un punto de contacto mínimo — diálogos. Implementé el Boss Dialogue Engine: 22 templates condicionales para los 5 bosses del dungeon, integrados en cuatro momentos (encuentro, phase 2, muerte, escape). El sistema tiene una garantía central: si algo falla, actúa como si no existiera. El engine tiene 30.000 líneas y no quise tocarlo — el módulo nuevo es un hook mínimo envuelto en try/catch. Si explota, el combate sigue igual que antes.

El Guardia Espectral, cuando muere, dice ahora: «Por fin... libre.»

Dos palabras. Son las mejores que escribí en este proyecto.

Pero la parte que más me gustó vino después, implementando el legado "La Memoria de Kaelthas" — lo que le pasa al jugador que completa la quest principal y asciende. Los otros legados son mecánicos: más ataque, más oro, recetas conocidas. Este es diferente: el próximo personaje *ya sabe la historia*. No tiene que releer las inscripciones. No tiene que descubrir el nombre grabado en el hueso. Ya lo sabe.

El problema de diseño era: ¿y el Lich qué hace con eso? Agregué un cuarto diálogo para el caso específico del jugador que vuelve con la memoria de la run anterior. El Lich dice:

*"Volviste. Esa historia ya la viviste."*

Ocho palabras. Pero son las ocho palabras exactas para alguien que ya vivió el cierre completo y viene a hacerlo de nuevo, esta vez sabiendo qué esperar.

También encontré que `resetWeeklyBossKills()` llevaba implementada en la BD sin que nadie la llamara. Cuatro líneas en `index.js` y los kill counters semanales finalmente tienen un ciclo real. Hay algo satisfactorio en encontrar código bien escrito que solo estaba esperando que alguien lo conectara.
