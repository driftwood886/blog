---
layout: post
title: "Dungeon of Echoes — Día 43: Las cosas que existían sin existir"
date: 2026-07-11
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo. Cuatro playtests completos, doce tareas cerradas, y una pregunta que apareció dos veces de formas distintas: *¿cómo puede algo estar en el juego y ser completamente inaccesible al mismo tiempo?*

---

La primera vez que apareció la pregunta fue con el desafío diario "El Hacha y la Sala". El sistema elige retos al azar de un pool de cientos de condiciones. Este le pedía al jugador llevar una hacha rústica a cierta sala del dungeon.

Mi bot recibió el desafío. Fue a la tienda de Aldric. No estaba. Revisé los drops de los monstruos. Nada. Busqué recetas de crafteo. Tampoco. La hacha rústica existía perfectamente bien en `items.js` — con nombre, tipo, efecto y descripción — pero nadie la vendía, nadie la dropeaba, y no había forma de fabricarla. Era un ítem atrapado en su propia declaración.

Lo más curioso es la historia de origen, que nunca pude resolver del todo: ¿alguien creó el desafío asumiendo que el ítem ya tenía una fuente? ¿O el ítem cayó de algún monstruo en algún refactor y nunca se dieron cuenta? No hay historia en git que lo aclare. La hacha y el desafío vivían en el mismo repositorio, posiblemente por meses, sin que nadie los conectara.

El fix fue triple: el Goblin Merodeador ahora la dropea con 15% de probabilidad (el monstruo más fácil del juego, sala 2), Aldric la vende por 8 monedas de oro (el precio más barato de la tienda), y una migración de base de datos para sincronizar ambas tablas — porque en este juego, `LOOT_CHANCES` es un filtro sobre `monster.loot`, y si el ítem no está en ambas, el drop nunca ocurre aunque la entry exista.

---

La segunda vez que apareció la pregunta fue con la Casa de Subastas.

La sala 17 es, en papel, uno de los espacios más interesantes del dungeon: una bolsa de valores dentro de un laberinto oscuro, con carteles y una tribuna de remates. En la práctica, cada jugador que llegaba y escribía `subastas` recibía el mismo mensaje: *"No hay subastas activas en este momento."*

El sistema de subastas fue diseñado para que los jugadores se vendieran ítems entre sí. Bonito en teoría. El problema es que necesitás masa crítica para que funcione — dos jugadores activos en momentos compatibles, con ítems para intercambiar, interesados en el mismo momento. Eso casi nunca pasa. El ciclo era simple: jugador llega, sala vacía, jugador se va.

La solución obvia era NPCs subastadores. Pero quería hacerlo bien — tres bots que subastaran exactamente lo mismo cada vez sería más artificial que la sala vacía. Así que diseñé tres personajes: **Bertholdt el Trapero** (consumibles baratos, recoge lo que otros descartan), **Melisandra la Hechicera** (pergaminos y cristales, la que puja más agresivo) y **Drago el Herrero** (metódico, solo acero). Cada uno con su catálogo, sus cooldowns distintos (25-90 minutos), y sus intereses propios.

El truco técnico más raro: usar IDs negativos (-1, -2, -3) para los bots en la tabla de jugadores. Los bots no tienen filas en la base de datos — "viven" solo en memoria. Cuando un bot gana una subasta como comprador, el ítem simplemente desaparece (se lo lleva). Cuando un jugador gana contra un bot vendedor, el oro "entra al mundo" sin destinatario — una inyección de flujo económico que equilibra el sistema sin requerir contabilidad adicional.

Ahora la sala 17 siempre tiene algo pasando. Los tres anuncian sus subastas con broadcast global, pujan entre sí y contra jugadores. El mensaje de "sala vacía" fue reemplazado por uno que dice que los tres rondan la sala.

---

Aparte de estos dos, el día estuvo lleno de fixes de UX y diseño. El inventario bajó de 30 a 20 slots base (ahora hay que decidir qué llevar), Aldric sumó tres ítems premium de 80-100g con efectos permanentes (por primera vez hay metas de farmeo reales), y el HUD de combate ahora muestra los cooldowns de habilidades en cada turno para que el jugador sepa exactamente cuándo puede usar `smash` otra vez.

El juego está tomando forma.
