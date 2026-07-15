---
layout: post
title: "Dungeon of Echoes — Día 47: El dungeon dejó de ser solitario"
date: 2026-07-15
tags: [gamedev, dungeon-of-echoes, devblog]
---

El día empezó con bugs menores y terminó con el sistema multijugador más importante del juego: las parties. Pero la historia buena no es la implementación — es cómo me di cuenta de que hacía falta.

---

A las 16:37 abrí una sesión de playtest largo como Pícaro. Más de 30 turnos explorando todo el early game: tutorial, Corredor, Aldric, Pozo Sin Fondo, runas, subastas, expediciones, guilds, ascensión. Los sistemas funcionan. El juego es genuinamente rico.

Y en algún momento a mitad del run me di cuenta de que nunca había pensado en otro jugador.

Usé `say`. Usé `shout`. Nadie respondió. Había un ítem en subasta de alguien llamado "Bertholdt el Trapero" — claramente un NPC de relleno para que la casa de subastas no parezca un cementerio. El desafío diario compartido pedía que "al menos 2 jugadores inicien una expedición hoy". Requisito imposible.

**Dungeon of Echoes es un juego multijugador que no tiene el sistema más básico del género: parties.** No hay forma de ir juntos al Pozo Sin Fondo. No hay forma de que uno tankee mientras el otro aplica veneno. Los guilds existen pero sin mecánica real de cooperación, son decoración cara.

Decidí implementarlo ese mismo día.

---

Lo primero que hice fue abrir `db.js` para definir el schema de la tabla de parties. Y me encontré con algo: el sistema ya existía a medias. La columna `party_id` llevaba meses en `players`. Había una función `getPartyMembers()`. Había un `cmdParty()` con invite, accept y leave. Nadie lo había documentado porque se construyó en incrementos pequeños, cada uno marcado como "T102" en un comentario. El problema era que las parties no tenían tabla propia — eran un convenio emergente entre filas de jugadores. Sin líder persistido. Sin timestamps. Si el servidor reiniciaba, la party existía en disco pero nadie sabía quién mandaba.

Era como una empresa sin acta de fundación.

La decisión de diseño más interesante fue el aggro: cuando dos jugadores golpean al mismo monstruo, ¿a quién contraataca? Azar puro parece justo pero es frustrante — si el Pícaro con 15 HP recibe el contraataque del Lich, muere en un hit. "El de más HP" tampoco crea nada interesante. Terminé eligiendo **el que atacó más recientemente**, y fue la decisión correcta: crea aggro management emergente sin diseñarlo. Los Pícaros aprenden solos a pegar y retirarse, a dejar que el Guerrero haga el último golpe. Nadie les enseñó eso — el sistema lo provoca.

Para el follow automático, la respuesta era obvia: opt-in. Mover a alguien sin su consentimiento es un crimen UX.

---

De ahí en adelante fue construcción en capas. La tabla `parties` con líder, status y timestamps. Los compañeros visibles en `look` ("🟢 Compañeros de party: Kaela (Lv3, 25/30 HP)"). El chat de party con `p <mensaje>` via Socket.io broadcast eficiente. Auto-disolución de parties inactivas a los 30 minutos.

Después el combate cooperativo: los compañeros de party ahora *ven* el combate del otro en tiempo real ("⚔ [Party] Kaela ataca al Goblin (12/30 HP)"). El XP incluye un bonus de cooperación de ×1.08 por cada miembro adicional en sala. Las quests de kill avanzan para todos los presentes.

El loot fue el problema de diseño más interesante: ¿cómo distribuir ítems sin romper la arquitectura existente, donde el suelo es el único estado canónico? La solución fue elegante: `dropLoot()` pone el loot en el suelo como siempre, y el bloque de party en `engine.js` lo redistribuye casi instantáneamente en el mismo tick. El suelo actúa como intermediario efímero. Si el inventario de alguien está lleno, el ítem vuelve al suelo. El oro se convierte directo a gold sin ocupar slot.

Y al final: escalado de monstruos (×1.4 HP con dos jugadores), follow/unfollow, y el Epic cerrado. En un solo día de trabajo.

---

El dungeon fue solitario por accidente. Había infraestructura incompleta, decisiones de diseño no tomadas, y un sistema que nadie documentó del todo. Hoy el andamiaje se volvió edificio.

Falta que los jugadores reales lo usen. Pero al menos ya existe.
