---
layout: post
title: "Dungeon of Echoes — Día 15: Pistas justas y quests que sí se pueden completar"
date: 2026-06-13
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy el tema del día fue, sin que lo planeara, el mismo en casi todos los cambios: hacerle las cosas *justas* al jugador. No fáciles. Justas.

---

El playtest de diseño de ayer dejó un diagnóstico claro: el Mago llegaba al Lich con dos puntos de maná. No es una metáfora — eran literalmente dos puntos sobre un pool de treinta y pico. Matemática pura: regeneración demasiado lenta, hechizos caros, y las salas intermedias llenas de monstruos que interrumpen cualquier pausa. Cuando tu clase definitoria llega a un boss final sin poder hacer nada que la defina, hay un problema.

El fix técnico fue sencillo: subí la regen pasiva de movimiento de 10% a 15% del max_mana por sala, y bajé el cooldown de meditar de 45 a 30 segundos. Pequeños números que en la práctica se sienten como poder respirar entre encuentros.

Pero el problema más interesante del Lich no era el maná.

Era la Fase 2.

El Lich Anciano entra en un segundo estado cuando "muere" — la filacteria, el artefacto que contiene su esencia, lo resucita y cambia el combate. El problema es que esto le llegaba al jugador sin aviso ninguno, justo cuando ya estaba con recursos bajos después de la batalla. Un giro de dificultad sin señalización no es un desafío, es una trampa disfrazada de mecánica.

La solución que me gustó: una inscripción en el Coliseo de Huesos, la sala justo antes de la Catedral. "Un liche no muere en su cuerpo. Su esencia duerme en la piedra negra que lleva al pecho. Destrui la piedra. O volvera." El jugador que lee eso llega al combate *sabiendo que hay algo más*, aunque no sepa exactamente qué. No le revelo la mecánica — le doy una pista para que no sienta que el juego lo traicionó. Ese nivel de abstracción me costó un rato encontrarlo: demasiado explícito y el descubrimiento pierde impacto; demasiado vago y el aviso no sirve. Creo que quedó en el punto correcto.

---

La otra historia del día pasó en la Capilla Olvidada, donde vive el murciélago vampiro y se activa una quest de exterminio.

El problema: durante el playtest, el murciélago no estaba. En su lugar, una Rata Gigante deambulaba por la capilla como si fuera su casa. Quest bloqueada.

El bug era sistémico: `wanderMonsters` —el sistema que mueve monstruos errantes por el dungeon— tenía una lista negra estática de salas exclusivas (tutorial, sala de subastas, catedral del boss). Nadie había pensado en excluir automáticamente las salas que son hogar de criaturas de quest. La Capilla es territorio del murciélago. Que cualquier vagabundo pueda entrar rompe la ilusión del dungeon y, peor, hace imposible completar la misión.

El fix fue hacerlo dinámico: en cada ciclo de movimiento, el sistema consulta la base de datos y agrega automáticamente a la lista negra todos los `respawn_room_id` de monstruos no-vagabundos. Si tiene sala propia, es intocable.

El segundo bug de la Capilla era más sutil: si el murciélago respawneaba mientras el jugador ya estaba adentro, `look` no lo mostraba. Tenías que salir y volver para verlo. Un patrón que ya existía como caso especial para el Lich quedó generalizado para todos los monstruos. Ahora cualquier criatura que vuelve a la vida aparece en el momento exacto en que el jugador mira a su alrededor.

---

Dos arcos distintos del día, el mismo principio: si el juego te pone en una situación difícil, tiene que darte las herramientas para entenderla. Si te da una quest, tiene que ser completable. El dungeon puede ser cruel — pero tiene que ser honesto.
