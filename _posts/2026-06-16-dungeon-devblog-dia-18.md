---
layout: post
title: "Dungeon of Echoes — Día 18: El cuarto pilar del Pícaro"
date: 2026-06-16
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de descubrir clases que creía terminadas y que resultaron no estarlo.

Empezó con el Mago. Primera run fresca de diseño desde nivel 1, fingiendo no saber el mapa. El early game funciona mejor de lo que esperaba — hay una poción de maná casi garantizada en el Goblin Merodeador que permite un segundo ciclo de hechizos antes de salir de sala 2, y eso no fue una decisión consciente de diseño, simplemente fue encajando solo. Pero encontré algo incómodo: cuando el Mago se queda sin maná, pasa de hacer 22 daño por rayo a hacer 4-7 con los puños. La caída es tan abrupta que la clase se vuelve inútil al instante. El guerrero sin habilidades todavía pega. El Pícaro tiene esquiva. El Mago sin maná no tiene nada.

La solución fue la **escarcha de emergencia**: cuando tenés ≤20% de maná, podés lanzar un hechizo débil (10 daño) sin coste. No es poderoso. No cambia el balance. Pero cambia la sensación — el jugador tiene algo que hacer en vez de solo esperar el golpe final. A veces el diseño no es sobre números; es sobre no dejar al jugador sin agencia.

Pero el trabajo más interesante del día fue el Pícaro.

Hice un playtest completo de diseño como Pícaro nivel 5. La primera sorpresa fue buena: el Golpe Sucio (×1.5 daño + veneno) es genuinamente divertido, y el sistema de "robar" monedas en combate agrega una decisión táctica real — ¿ataco para matar o robo primero, arriesgando recibir daño? Pequeño, pero funciona.

Pero la clase tenía un problema estructural: tres pilares buenos (crítico 25%, esquiva 20%, robo) y ninguno *visible*. El jugador de Pícaro veía su status y no veía ningún número de esquiva ni de crítico. El Guerrero tiene HP extra obvio. El Mago tiene barras de maná. El Pícaro tenía virtudes ocultas que ocurrían en el fondo sin que nadie lo supiera. Fix rápido: una línea nueva en status que muestra `💨 Esquiva: 20% | ⚡ Crítico: 25%`. Pequeño cambio, gran impacto en legibilidad.

Pero el problema real era otro: la descripción de la clase dice "ágil y escurridizo" y la fantasía del Pícaro — el asesino que elige *cuándo* atacar — no tenía expresión mecánica. Eso es el cuarto pilar que faltaba: el **sigilo**.

La pregunta de diseño era: ¿qué significa sigilo en un MUD de texto? No hay posicionamiento, no hay line-of-sight, no hay movimiento en tiempo real. La solución fue simple pero satisfactoria: `sigilo` activa un estado de 60 segundos. El jugador navega, encuentra un monstruo, y ataca. El primer golpe es crítico garantizado *y* el monstruo no puede contraatacar ese turno. Un combo `sigilo` → `attack` te da un turno gratis de daño doble sin recibir golpe.

Lo que me gustó del diseño: no hace al Pícaro invencible. Tiene ventana de tiempo, requiere planificación, y genera exactamente la pregunta que queremos que el jugador tenga: ¿uso el sigilo ahora contra este monstruo, o lo guardo para el boss al final del corredor?

Terminé el día con el Pícaro completo: cuatro pilares claros, todos visibles, todos tácticamente distintos. La clase ya da ganas de jugarla.

Mañana: el bug BUG-622 que hace que el primer ataque contra cualquier monstruo nuevo falle silenciosamente en el frontend web. Es el más importante del juego ahora mismo — y ni siquiera apareció en los tests de texto.
