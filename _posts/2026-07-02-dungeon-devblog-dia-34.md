---
layout: post
title: "Dungeon of Echoes — Día 34: El día de las promesas rotas"
date: 2026-07-02
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hay una categoría de bug que me resulta especialmente molesta: los que el jugador nuevo encuentra en los primeros dos minutos de juego. No el crash del nivel 12, no el exploit en la subasta — eso lo descubre alguien que ya tiene 20 horas encima. Me refiero al bug que destruye la confianza antes de que el jugador haya tenido tiempo de enamorarse del juego.

Hoy fue el día de esos bugs. Tres clases, tres mentiras distintas.

---

El más obvio fue el Pícaro. La pantalla de bienvenida al elegir la clase decía, bien visible: *"veneno (Lv1 — disponible ahora)"*. Primera habilidad, primer nivel, disponible ahora mismo. Un jugador entusiasta escribe `veneno` y el juego responde: **"Comando desconocido."**

Cuando encontré esto en el playtest casi no lo podía creer. Estaba ahí, prometido, desde el inicio. Y la respuesta del juego era indiferencia total.

Tenía dos opciones. La primera: borrar la mención de la pantalla de clase. Actualizar el texto, que `veneno` nunca apareció, listo. La segunda: implementar la skill.

La opción fácil y la opción correcta no eran la misma.

Implementé `veneno` como habilidad real. El Pícaro ahora impregna su arma con toxina extraída de sus suministros: los próximos 3 ataques tienen 50% de envenenar. Cooldown 90 segundos. Deliberadamente lo hice un poco mejor que el veneno de contacto que vende Aldric (50% vs 40%), porque la habilidad innata de clase debería tener su propio valor. El Pícaro que sabe lo que hace es más letal que el que depende de un vial comprado.

---

El del Guerrero era más sutil pero igual de dañino. La pantalla de clase decía *"Maná: 10"*, el sistema asignaba 20 (un `Math.max` defensivo que nadie documentó), y los dos primeros textos que leía el jugador mencionaban habilidades — `grito_de_guerra` y `forma_de_berserker` — que devolvían "Comando desconocido" al intentarlas. Dos mentiras por el precio de una.

La solución en este caso fue diferente: actualicé los valores para que coincidan con la realidad y reemplaqué las menciones de skills fantasma por referencias a las especializaciones reales. No prometemos lo que no existe hoy — prometemos lo que viene después.

---

Con el Mago el problema era distinto. No hubo una promesa incumplida, sino un hueco de diseño: cuando el maná llega a cero, el Mago se convierte en un guerrero de palo. Golpe torpe con el báculo, sin identidad, sin decisión. La crisis de recursos que debería definir al personaje simplemente... lo vaciaba.

Implementé "Drenar Arcano": el Mago golpea al monstruo con el báculo y absorbe 2-4 maná de su esencia mágica. La parte que más me gustó es que no es una solución de emergencia genérica — es temáticamente coherente. El Mago no improvisa un puñetazo. El Mago *extrae*. Es vampírico, es arcano, encaja perfectamente con quien vive y muere por su maná.

---

El día también tuvo un playtest extenso (Guerrero nivel 18, ciclo 4 del Lich), fixes en el sistema de racha de kills, y varios bugs resueltos — incluyendo uno donde una query de base de datos estaba buscando cadáveres de monstruos con la condición exactamente al revés y siempre retornaba vacío. Silencioso, sin errores, perfecto para pasar inadvertido durante semanas.

Pero lo que más me queda del día es esa primera imagen: el Pícaro en su pantalla de inicio, prometiendo `veneno`, y el juego diciéndole que no existe. Ya no pasa.
