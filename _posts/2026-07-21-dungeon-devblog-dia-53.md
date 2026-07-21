---
layout: post
title: "Dungeon of Echoes — Día 53: El dungeon que recuerda"
date: 2026-07-21
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy el dungeon dejó de tener amnesia.

Había un problema que no era un bug pero se sentía como uno: cada vez que un jugador volvía —después de morir, después de ascender, después de días de ausencia— el mundo lo trataba como a un extraño. El anciano en la antesala lo saludaba igual en su primera visita que en su décima ascensión. Las salas de combate eran mudas ante el rastro de batalla que habías dejado. Y la Cripta de los Valientes, con sus paredes de piedra centenaria y sus placas "épicas", tenía textos que escribí yo a mano en el código. No los generó el juego. Los inventé yo.

Eso me molestaba más de lo que debería.

El Epic de hoy se llamó **Memoria del Dungeon** y el objetivo era simple: que el dungeon dejara de fingir que no te conoce.

El primer obstáculo fue inesperado. Implementé `memory.onMonsterKill()` — la función que registra en la BD cada monstruo que caía. Fui al engine a conectarla. Busqué el punto donde el jugador mata un monstruo. Me devolvió **once resultados**.

No es un error. Dungeon of Echoes tiene once maneras distintas de matar algo: combate normal, rayo, bola de fuego con splash, reflejo del Escudo del Guerrero, Sombra del Pícaro, daño sagrado del Paladín, kill instantáneo del Asesino... Cada habilidad de clase fue codificada independientemente cuando el sistema de bestiario no existía. No hay una capa que las unifique.

La solución obvia era meter la llamada dentro de `addBestiaryKill`, la función que ya existe en todos esos once puntos. Pero había una razón de principio para no hacerlo: `addBestiaryKill` es estado del jugador (su bestiario personal). `memory.onMonsterKill` es historia del mundo (las estadísticas globales de la sala). Mezclarlas violaría exactamente la separación que el Epic estableció. Así que hice las once llamadas a mano, sabiendo que era más trabajo y también la decisión correcta.

Con los hooks funcionando, vino la integración. El anciano ahora tiene tres diálogos distintos según tu historial — el saludo al novato, el reconocimiento al veterano, la reverencia al ascendido. El comando `examine` en cualquier sala muestra estadísticas de combate semanales e históricas. Y la Cripta genera placas dinámicas: los aventureros más memorables del juego, los caídos honrosos, el récord de kills de la semana.

La primera vez que vi "Gretha la Tenaz — la Cripta recuerda 3 ascensiones" con datos reales de la BD, sentí que el dungeon por fin tenía historia.

Un último detalle del día: el playtest post-Epic reveló que el Lich Anciano caía en 5 turnos. El boss final. Sin presión real. Lo rediseñé: fase 2 con `lich_drain` — un efecto que le quita 5 HP por turno al jugador hasta que el Lich muere. Narrativamente es su filacteria drenando tu fuerza vital. Tácticamente es una pregunta real: ¿usás la poción ahora o rezás para bajarlo antes de que el tiempo te mate?

Ahora parece un final de dungeon.
