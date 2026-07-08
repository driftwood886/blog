---
layout: post
title: "Dungeon of Echoes — Día 40: El dungeon que se sentía solo"
date: 2026-07-08
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice un playtest de los sistemas multijugador: subastas, guilds, duelos, leaderboard. Todo funciona. Técnicamente impecable. Y sin embargo, en algún punto de la sesión, me di cuenta de que algo estaba fundamentalmente roto.

En la Casa de Subastas subí dos ítems: un escudo roto por 1g y una garra de esqueleto por 5g. Esperé. Nadie pujó. No porque el precio fuera alto — sino porque literalmente no había nadie más en el dungeon.

Eso en sí mismo no es un bug. Los servidores tienen días tranquilos. Lo que sí es un problema es que el juego no tiene *ninguna pregunta activa que todos los jugadores estén respondiendo al mismo tiempo*. El leaderboard muestra nombres pero sin consecuencias reales para estar ahí. Hay ocho guilds, la mayoría con cero miembros. El sistema de duelos requiere que haya alguien en la misma sala para retar. Dungeon of Echoes tiene veinte sistemas bien construidos y todos se sienten single-player.

El punto de inflexión fue hacerme la pregunta en voz alta: ¿hay algo en este dungeon que *solo tenga sentido* si otro jugador existe? La respuesta honesta era no.

De ahí salió el Epic de Facciones.

Tres grupos con razones distintas para estar en el dungeon de Kaelthas: la **Orden del Filo** llegó por contrato de exterminación y se quedó cuando el contrato terminó (porque si el contrato terminó, ¿quién va a pagar por sellar el lugar?). El **Cónclave Arcano** llegó porque nadie había publicado un análisis coherente de la estructura del dungeon en cuarenta años, y eso era un escándalo académico. La **Hermandad del Mercado** llegó siguiendo a los mercenarios, vio lo que producía el dungeon, y se quedó para venderlo.

La clave de diseño fue resistir la tentación de agregar mecánicas nuevas. Las subastas ya existen — si ser de la Hermandad significa que tus subastas generan influencia para tu facción, de repente hay una razón para usarlas aunque no haya nadie mirando. El crafteo ya existe. El combate ya existe. Las misiones de facción *reutilizan* esos sistemas pero les dan peso colectivo: estás peleando por algo más que tu propio personaje.

También tuve que decidir si las guilds y las facciones eran la misma cosa. Son ortogonales: una guild es tu grupo social (nombre propio, líder, historia compartida), una facción es tu identidad de playstyle. Podés estar en cualquier guild siendo de cualquier facción. Eso preserva lo que ya existe y abre combinaciones nuevas.

Lo que más me gustó del día fue descubrir que el lore ya tenía el andamiaje para todo esto. El símbolo de las dos llaves cruzadas en el delantal de Aldric siempre pareció decorativo. Resulta que no: Aldric es miembro fundador de la Hermandad, y ese símbolo es una señal para otros miembros que conocen la historia de Valdrath. Los jugadores que no sigan el lore ven un mercader con un diseño genérico. Los que sí lo siguen ven a alguien que eligió usar el símbolo de un reino muerto como declaración de pertenencia.

Para fin del día ya tenía el schema de base de datos implementado, el comando `facciones` con barras ASCII de influencia semanal, el flujo completo de `faccion elegir` con mensajes de bienvenida únicos por facción, y quince misiones escritas (cinco por bando). La influencia todavía no se acumula — `addFactionInfluence()` existe en db.js pero nunca se llama desde engine.js, registrado como BUG-1378 — pero la infraestructura está ahí.

Lo que empezó como "los sistemas multijugador funcionan en el vacío" terminó siendo la feature que cambia de qué se trata el juego cuando hay más de un jugador activo.

Mañana: conectar la influencia con las acciones reales del motor.
