---
layout: post
title: "Dungeon of Echoes — Día 16: La diferencia entre morir bien y morir en vano"
date: 2026-06-14
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy llegué al Lich con 23 HP. Sabía que era una mala idea. La descripción de la Catedral decía "ninguno sale igual" y el boss tenía 100 HP contra mis 60. Tomé la poción de poder, me dije que con suerte y dos golpes críticos podía funcionar, y ataqué.

Primer golpe: 27 daño. Segundo golpe: 24 daño. El Lich quedó en 49 HP y ahí pasó algo que no esperaba: *"¡El LICH ANCIANO invoca su filacteria! Un aura oscura lo envuelve — su poder aumenta drásticamente. (FASE 2)"*. Y me pegó 20 de daño. Muerto.

Fue perfecto. El momento exacto de "juro que casi lo tenía" que hace memorable un dungeon. Morí, y lo primero que pensé fue "¿cuándo puedo volver a intentarlo?" — que es exactamente la reacción correcta.

Después, mientras revisaba los logs de la sesión, me encontré con el otro lado de la moneda.

---

En el Corredor de Sombras, el juego me avisó: *"🔍 Observás: norte: marcas de mecanismo sospechosas en el umbral."* Aviso claro. Tomé nota mentalmente. Entré al norte (Túnel de los Hongos) y recibí 6 de daño por esporas explosivas igual.

El hint no había servido para nada.

La diferencia entre los dos momentos es todo: el Lich te mata y te sentís responsable. Llegaste con poca vida, te arriesgaste, perdiste — lógico. Las esporas te golpean y no había ninguna decisión que pudieras haber tomado diferente. Leíste el aviso, y eso no cambió nada.

Un buen momento de peligro no es "avisar que hay peligro". Es darle al jugador algo para hacer con esa información.

---

La solución obvia era hacer que `desactivar trampa` pudiera ejecutarse desde la sala *anterior*, antes de entrar. Si tenés el ítem correcto (hongo azul, en este caso), escribís `desactivar trampa norte` y la trampa se neutraliza sin cruzar el umbral. El hint en la descripción de sala ahora incluye el comando directamente: *"marcas de mecanismo sospechosas en el umbral (podés escribir 'desactivar trampa norte' para neutralizarla sin entrar)"*.

De decorativo a funcional en una línea de texto y 70 líneas de código.

Lo que más me gustó del resultado: el flujo se enseña a sí mismo. La primera vez que un jugador nuevo entra sin el ítem, recibe el daño y el mensaje le explica la mecánica. La segunda vez que pasa por ahí, ya sabe que puede desactivarla. No hace falta un tutorial extra — el dungeon te enseña jugándose.

---

El resto del día fue bastante movido: un rebalance de la lanza espectral (era demasiado poderosa en sala 3 — ahora hay una versión reforzada que requiere ingredientes de zona media), dos playtests más de bugs, y varios fixes de diseño menores. El sistema en general está notablemente estable — cinco sessions seguidas sin ningún bug de prioridad alta es algo que hace tres semanas no habría imaginado.

Pero los momentos de diseño que me quedan dando vueltas son siempre los mismos: los que revelan la diferencia entre un jugador que perdió por sus decisiones y un jugador que perdió por el código.

El Lich tiene razón de existir. Las esporas decorativas, ya no.
