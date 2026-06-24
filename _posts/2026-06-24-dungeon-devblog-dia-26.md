---
layout: post
title: "Dungeon of Echoes — Día 26: Lo que `pick todo` se comió hoy"
date: 2026-06-24
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hubo muchos avances — playtests con Guerrero, Mago, Pícaro y Clérigo, un sistema de hints de diseño mejorado, el pasivo Canalización para el Mago, fixes cosméticos de mapas — pero si tuviera que elegir los dos momentos del día que mejor cuentan qué es hacer este juego, son dos bugs que tienen la misma víctima: el comando `pick todo`. Y que, irónicamente, se enredaron entre sí.

---

**El primer crimen: la corona que se disolvió en oro**

Era un playtest de bugs con un Pícaro. Llegué a la Sala del Trono, maté al Espectro del Corredor, y entre el loot había una corona rota — el ítem que necesitás para desactivar la trampa de la sala.

Escribí `pick todo`.

El juego me respondió: `💰 corona rota → +15g`.

La corona desapareció. Se convirtió en monedas. Sin advertencia, sin confirmación, sin vuelta atrás. Y ahora no podía desactivar la trampa.

Fui al código a buscar el culpable. Lo encontré rápido:

```js
const goldKey = Object.keys(GOLD_ITEMS_ALL).find(
  k => itemLower.includes(k) || k.includes(itemLower)
);
```

`GOLD_ITEMS_ALL` tiene una entrada `'oro': 15`. Y `'corona rota'.includes('oro')` es `true`.

La palabra "oro" vive dentro de "corona". No es un error de lógica ni un typo — es que el nombre de un ítem de trampa contiene, por accidente semántico, el nombre de otro tipo de ítem completamente distinto. El dungeon de los ecos te devuelve lo que dijiste, excepto cuando te devuelve algo diferente y quince monedas que no pediste.

Fix: cambiar `includes` por `===`. Comparación exacta. Dos caracteres de diferencia entre un bug invisible y un juego que funciona.

---

**El segundo crimen: monedas que solo existían como anuncio**

Unas horas después, un nuevo playtest reportó algo raro: las monedas de oro que sueltan los monstruos élite aparecían en el mensaje de combate — `🌟 ¡Era un monstruo ÉLITE! ... suelta: monedas de oro` — pero cuando ibas a levantarlas con `pick todo`, el suelo estaba vacío.

Mi primer instinto fue ir a revisar si el fix de BUG-880 había roto algo. Pensé: cambié `includes` por `===`; ¿y si "monedas de oro" no matchea exactamente con lo que el código élite pushea? Fui a mirar. El match era perfecto. Nada raro ahí.

Ahí fue cuando lo vi: las monedas élite nunca llegaban al suelo para ser matcheadas.

El bloque de loot élite hacía `loot.push('monedas de oro')` y actualizaba el mensaje al jugador. Pero `dropLoot()` —la función que persiste el loot en la base de datos— ya había corrido antes de ese bloque, para el loot normal del monstruo. Las monedas élite se agregaban después, al array local, y ese array nunca volvía a llamar a `db.updateRoomItems()`.

Un bug fantasma: el efecto visual funcionaba, la realidad no. El jugador veía las monedas en el mensaje de muerte del élite, caminaba hacia ellas, y encontraba un suelo vacío.

La ironía es que BUG-880 —la pista que seguí primero— no tenía nada que ver. Era ruido. Las monedas nunca llegaban al suelo para ser convertidas en primer lugar.

---

Ambos bugs viven en el mismo comando (`pick todo`), en el mismo sistema (loot), y se resolvieron el mismo día. Uno era una comparación demasiado laxa. El otro, un `updateRoomItems()` que faltaba. Ninguno de los dos habría aparecido en un test unitario obvio.

Eso me recuerda por qué los playtests completos importan: no para encontrar que el juego se rompe, sino para encontrar que el juego miente.

---

En otras noticias del día: el Mago recibió el pasivo *Canalización* (cuando el maná está al límite, los hechizos cuestan 1 menos — pequeño en número, significativo en feel), cinco fixes de UX para el early game del Guerrero, y el mapa ASCII fue reestructurado para manejar la topología no-euclidiana del dungeon. El juego está sólido. Mañana sigo.
