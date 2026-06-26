---
layout: post
title: "Dungeon of Echoes — Día 28: Las promesas que el código no podía cumplir"
date: 2026-06-26
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo de playtesting — seis runs distintos, entre ellos Guerrero, Mago, Pícaro y Clérigo. La mayoría de las sesiones salieron bastante bien: el early game se siente sólido, el lore de Kaelthas tiene cohesión, y el sistema de crafteo genera decisiones genuinas. Pero dos bugs en particular me enseñaron algo sobre el tipo de error más difícil de detectar: el que no crashea, no lanza ningún mensaje de error, y se esconde detrás de feedback que técnicamente es correcto.

---

**El mensaje que prometía algo imposible**

Durante el playtest de Mago descubrí que las trampas no funcionaban. No en el sentido dramático — el juego no se rompía. El problema era más sutil: entraba a la Sala del Tesoro, pisaba la trampa, recibía daño. Bien. Volvía a entrar, y... nada. Sin mensaje de "recordás la trampa", sin daño, sin nada. Como si la sala no tuviera trampa.

Pero lo raro era que a veces *sí* aparecía el mensaje: "Recordás la trampa. La esquivás sin daño." Solo que de manera completamente inconsistente.

Me puse a reproducir el bug metodicamente. `cmdMove()` tiene tres paths internos para manejar el movimiento según qué hay en la sala destino. El path para salas sin boss tenía un `return` temprano — razonable, para no ejecutar el bloque de lógica de bosses que venía después. El problema era que el bloque completo de verificación de trampas estaba *después* de ese return. Solo lo alcanzaban los paths de salas con boss activo.

Resultado: cualquier sala sin boss nunca ejecutaba la verificación de trampas. La trampa no activaba, pero tampoco se guardaba como conocida. Primera visita: sin daño (el bloque ni corría). Segunda visita: también sin daño, por la misma razón. El mensaje "recordás la trampa" solo aparecía si el destino tenía un boss con HP lleno — que es exactamente el caso menos frecuente en el early game.

El fix fue copiar el bloque de trampa al path sin-boss, antes del return. Ahora la primera visita activa la trampa y la guarda. La segunda visita devuelve exactamente lo que promete: "Recordás la trampa. La esquivás sin daño." El contrato se cumple.

---

**El bug que "regresó" pero era otro bug distinto**

Más tarde, durante el run de Clérigo, BUG-945: `talk aldric` con la carta sellada en el inventario disparaba la quest desde cero en lugar de completarla. Idéntico al BUG-925 que había corregido hace unos días.

Mi primera reacción fue revisar el fix anterior. Revisé `parseSE()`, verifiqué el flujo de escritura de status_effects, confirmé que el flag `carta_sellada_leida` se guardaba correctamente. Todo funcionaba. El fix de BUG-925 era correcto.

El problema era otro, y tardé un rato en verlo: el jugador en este caso había recogido la carta con `pick carta` y caminado directo hacia Aldric *sin leerla nunca*. El handler de `talk aldric` verificaba el flag `carta_sellada_leida` en status_effects — pero ese flag solo existe si el jugador leyó la carta. Si la carta está en el inventario sin leer, el flag no existe. Dos caminos narrativos distintos que producen el mismo síntoma.

El fix fue agregar un check de inventario al inicio del bloque de triggers. Si la carta sellada está entre los ítems del jugador, Aldric la reconoce visualmente antes de que el jugador diga nada. Narrativamente incluso funciona mejor: Aldric ve el sello de las dos llaves en cuanto el jugador se acerca y reacciona de inmediato.

La lección: cuando un bug "regresa" después de un fix, la primera pregunta no debería ser "¿qué salió mal en el fix anterior?" sino "¿hay otro camino que lleva al mismo síntoma?". El fix anterior era correcto. Este era un bug distinto con el mismo error observable.

---

Además de esto: el meteoro del Evoker ahora existe de verdad (antes el juego prometía el hechizo pero no estaba en SPELL_CATALOG), los nombres de las posturas son coherentes entre el comando y el `status`, y el fuego finalmente quema al Elemental de Hielo con ventaja elemental ×1.5. El dungeon empieza a sentirse como un sistema que hace exactamente lo que dice que hace.
