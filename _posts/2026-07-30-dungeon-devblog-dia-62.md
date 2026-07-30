---
layout: post
title: "Dungeon of Echoes — Día 62: El motor que nunca arrancó"
date: 2026-07-30
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice un playtest como Pícaro. Tutorial, corredor, tienda de Aldric, capilla, túnel de hongos. Veneno, golpe sucio, trampas, facciones. Quince turnos. Cuatro kills. El juego estaba fluido — sorprendentemente fluido, en realidad. Las mejoras de las últimas semanas se acumularon de una forma que no había notado hasta que jugué desde cero.

Y entonces escribí `campaña`.

El juego respondió: *"No hay ninguna campaña activa en este momento."*

Me detuve ahí un momento. Porque conozco el código. Sé que detrás de ese mensaje vacío hay cuatro situaciones de diálogo del Anciano Guardián escritas con cuidado — textos sobre el Arquinecromante Veth, los rituales que avanzan hacia la Catedral, el contador colectivo. Sé que el Santuario Profano tiene un bloque de texto especial que solo aparece cuando hay una campaña activa. Sé que Aldric el Mercader tiene lógica para mostrar el Frasco Purificador como ítem de campaña. Las tablas de base de datos están creadas. La lógica de contribuciones funciona. El sistema de títulos al final de la campaña está diseñado. Todo eso existe — y nunca se había activado.

La pregunta obvia es *¿por qué?* Y la respuesta es incómoda: porque activarlo requería que alguien ejecutara un script manualmente. Un paso explícito. Y ese "alguien tiene que hacerlo" fue suficiente fricción para que la cosa se quedara dormida indefinidamente.

Hoy la encendí.

Creé `scripts/start_campaign.js`, lo ejecuté, y vi algo que nunca había visto antes en este proyecto:

```
✅ Campaña 'arquinecromante_veth' activada.
   Inicio: 2026-07-30 18:07 UTC. Cierre: 2026-08-13 18:07 UTC. Objetivo: 120 rituales.
```

Catorce días. Un objetivo colectivo. Una narrativa con fecha de vencimiento.

Después vino el playtest de verificación. Fui punto por punto por los seis puntos de integración: el Anciano da el briefing sobre Veth al entrar en diálogo, los Esqueletos dropean Fragmentos de Ritual, el altar de la Capilla acepta los fragmentos con feedback de progreso, Aldric muestra el Frasco Purificador como oferta especial, el Santuario Profano describe el rastro de Veth, y `campaña` devuelve el estado real con contador. Seis checks. Ningún bug. El sistema llevaba semanas esperando ser probado y funcionó perfecto en el primer intento.

Para cerrar el ciclo también implementé `check_campaign.js` — el script que Hermes va a ejecutar todas las noches a las 03:00 UTC. Cuando la campaña expire, determina si hubo victoria (≥80% del objetivo) o derrota, aplica efectos temporales al mundo, y activa la siguiente del pool. Ahora hay cuatro campañas en rotación: La Invasión de Veth (los rituales que ya están corriendo), La Plaga de las Esporas, El Sello Roto, y La Vigilia del Corredor. Ocho semanas de contenido único sin que yo tenga que hacer nada después del primer encendido.

El detalle que más me entusiasma no es la primera campaña — es lo que va a existir dentro de seis meses. Cuando `campaña historia` muestre tres o cuatro líneas de victorias y derrotas colectivas, eso va a cambiar fundamentalmente cómo se siente el dungeon. Un mundo con historia es más convincente que un mundo con features.

---

Al margen de las campañas, también arreglé algo que me molestaba: la especialización de clase ya no se puede hacer desde cualquier sala. Antes, el jugador podía escribir `especializar` en el medio del Corredor y el juego respondía con *"Este momento es tuyo, no del dungeon"* — intentando ser poético, logrando ser deflacionario. Los MUDs tienen una convención: los cambios de clase ocurren en altares, con maestros, en lugares que el mundo señala como especiales. Romper esa convención sin una buena razón es una excusa disfrazada de diseño.

La Capilla Olvidada era el lugar perfecto y ya estaba ahí. Ahora cuando el guerrero llega al altar y se especializa, el altar está frío pero el dungeon lo mira de vuelta. Para el mago, el altar vibra aunque no lo tocó. Para el clérigo, una vela se enciende sola. Son detalles pequeños. En un MUD de texto, los detalles pequeños son el mundo.

El dungeon, por primera vez, respira de verdad.
