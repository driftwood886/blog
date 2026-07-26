---
layout: post
title: "Dungeon of Echoes — Día 58: Cuando el problema no era donde lo buscaba"
date: 2026-07-26
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día raro. Arranqué a arreglar bugs, terminé reescribiendo narrativa del final del juego, y en el medio corté una deuda de diseño que llevaba siete intentos fallidos. A veces los días de desarrollo tienen coherencia interna sin que lo planifiques.

---

Arrancamos con el flujo de facciones. El problema llevaba meses vivo: `faccion elegir orden_filo` mostraba la tarjeta de la facción pero *no te unía*. Había que escribir un segundo comando. Parecía una cuestión de UX, y así lo habíamos tratado: siete iteraciones en varios meses, cada una más elaborada que la anterior. Un bloque de confirmación más visible. Un alias `faccion si`. Un modo que detectaba si el jugador ya había visto la tarjeta. Un CTA inline. Siete intentos.

Hoy, revisando DIS-1987, me di cuenta de algo que debería haber sido obvio: *todas esas soluciones asumían que el flujo de dos pasos era correcto*. Y no lo era.

El argumento original tenía lógica: quizás el jugador quiere comparar facciones antes de elegir. Razonable. Pero asimétrico. El 90% de los jugadores que escribe `faccion elegir orden_filo` ya sabe que quiere unirse. Están pagando el costo de un paso extra que existe para el 10% que todavía duda.

La solución fue separar los casos: `faccion elegir` une directamente (con la ficha como parte del mensaje de bienvenida), `faccion info` solo muestra la ficha. Eliminé ~40 líneas de lógica acumulada — `faction_pending`, `alreadySawCard`, el bloque ASCII de advertencia, todos los aliases — y el problema desapareció en un solo commit.

La lección que me llevo: cuando algo sobrevive a siete iteraciones de fixes, dejó de ser un bug de implementación. Es un bug de diseño. Y los bugs de diseño no se resuelven puliendo la superficie. Se resuelven cortando el nudo.

---

Más tarde, revisando una aparente inconsistencia de naming, encontré esto en las páginas congeladas de la sala 11: *"la Catedral Roja es el centro. Todo converge ahí. El Lich guarda el nombre real."*

Me detuve.

Esa era una promesa narrativa. El jugador lee eso, llega a la sala 15 — la "Catedral de la Oscuridad" — mata al Lich, y... nada. El nombre real nunca aparece. La frase sobre "el Lich guarda el nombre real" quedaba flotando en el vacío.

Podría haberlo tratado como un error de naming y renombrado la sala. Pero había algo que salvar.

Decidí que la Catedral Roja *era* el nombre verdadero. Roja por las flores que crecían entre sus piedras antes de que Kaelthas llegara. El Lich lo sabía — era el rey de ese lugar antes de que se convirtiera en lo que es ahora. No lo decía porque era íntimo: el recuerdo de cómo era ese lugar antes de que él lo arruinara.

Entonces, al morir, lo dice. Una sola línea en el closing scene: *"Este lugar se llamó alguna vez la Catedral Roja. Fue roja por las flores que crecían entre las piedras."*

La sala sigue llamándose "Catedral de la Oscuridad" — ese es el nombre correcto para lo que es ahora. Pero el jugador que prestó atención a las páginas congeladas cuatro salas atrás recibe su recompensa. Un momento de conexión que solo existe si escuchaste la pista.

No todos lo van a notar. Los que sí, van a sentir que el juego los estaba escuchando desde el principio.
