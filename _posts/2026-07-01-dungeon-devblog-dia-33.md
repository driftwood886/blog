---
layout: post
title: "Dungeon of Echoes — Día 33: El dungeon que recuerda"
date: 2026-07-01
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hubo un momento durante el playtest de hoy en que hice hablar a un bot con Aldric, el mercader del cuarto piso. Aldric dijo algo como siempre — su catálogo, sus precios, su indiferencia profesional. Y me di cuenta: este personaje no tiene idea de quién le está hablando. Le podés comprar treinta veces y en la trigésimoprimera te trata como a un desconocido. Eso se terminó hoy.

El día arrancó con varias cosas sueltas: el Clérigo/Sanador no tenía skills útiles en solitario (le di `escudo_sagrado` — absorbe el próximo golpe, 25 HP, 45 segundos de cooldown — y de paso nació la segunda especialización de la clase: JUICIO, con +2 ATK, -15 HP máximo y un `rayo_divino` que hace daño sagrado sin defensa), se arreglaron bugs varios, se mejoraron mensajes. El trabajo de siempre.

Pero en el playtest de la tarde pasó algo. Terminé un recorrido completo por el dungeon y escribí en mis notas: *el juego tiene 20+ sistemas que funcionan bien aislados pero no se hablan entre sí, y el mundo no recuerda al jugador*. Aldric te saluda igual el día 1 que el día 30. Si matás al Lich tres veces, el anciano no lo sabe. Si crafteastaste cincuenta ítems, el dungeon es indiferente.

Así que hice un Epic. Le puse "Mundo Persistente Reactivo" y lo diseñé para que cada fase funcione sola — no hay riesgo de quedar a medias.

**Fase 1: los NPCs recuerdan a cada jugador individualmente.**

La decisión de arquitectura parecía técnica: ¿tabla separada `npc_interactions` o columna JSON `npc_memory` en `players`? No es técnica. Es sobre cuándo vas a necesitar los datos. Si la respuesta es "cuando el jugador hable con el NPC", no necesitás SQL transaccional — necesitás un `JSON.parse` y listo. Así que fue JSON. Backward compatible, imposible de desincronizar, rápido.

Lo que tardó veinte minutos (y no cinco) fue escribir los diálogos. Cada NPC tiene que sonar como *ese* personaje al reconocerte, no como una notificación genérica de sistema. El Escriba fue el más difícil — su personalidad *es* la ausencia de personalidad. Entonces hacer que su reconocimiento sea memorable requería encontrar el gesto mínimo que lo rompiera: la pluma que se detiene un instante. Para el Nivel 3, la pluma va al tintero y te mira directamente. Eso, para el Escriba, es equivalente a un discurso de bienvenida.

Una pregunta de diseño que quedó pendiente hasta el final: ¿los contadores son por personaje o por cuenta? El jugador que ascendió cinco veces y tiene 200 compras con Aldric, ¿empieza de cero? No. Aldric no es parte del personaje — es parte del mundo. La relación es entre el mundo y el *jugador*. Los contadores se heredan en la ascensión. El nuevo personaje entra y Aldric ya lo reconoce.

**Fase 2: el dungeon recuerda lo que hicieron *todos* los jugadores juntos.**

Una tabla `world_state`, nueve contadores, lazy reset semanal. Si alguien mató diez arañas en el Pozo Sin Fondo esta semana, la próxima persona que entre va a notar que algo pasó. No sabe quién. No sabe exactamente cuándo. Solo que el dungeon está vivo.

El detalle más elegante: la sala 15 (Catedral del Lich) usa un timestamp, no un contador. El Lich cayó hace menos de dos horas: "el suelo todavía está caliente". Hace menos de 24 horas: "energía residual en las piedras". Más que eso: silencio. Es una ventana que decae sola, sin cron job ni reset. Solo comparar `Date.now()` con el timestamp guardado.

Y la sala del Escriba tiene un caso especial: si `subastas_semana == 0`, el Escriba anuncia que el mercado duerme. La ausencia de actividad es información tan válida como la presencia.

El playtest post-Epic confirmó que todo funciona. La sala 2 mostraba texto reactivo con `goblins_semana=10`. Sin bugs críticos.

El dungeon ya tiene memoria. Ahora hay que ver qué hace con ella.
