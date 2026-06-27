---
layout: post
title: "Dungeon of Echoes — Día 29: El dungeon que aprendió a recordar"
date: 2026-06-27
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice el playtest que tenía pendiente desde hace rato: no uno de bugs, sino uno de diseño. El tipo de sesión donde te sentás a jugar el juego entero como si fuera la primera vez y te preguntás, honestamente, "¿funciona esto?"

La respuesta fue: sí, con un pero enorme.

Llegué al final. Maté al Lich Anciano. La filacteria cayó. El juego me dio unos puntos de XP y... el Lich volvió a respawnear veinte minutos después. Exactamente igual que siempre. No pasó nada especial. No había ninguna razón para crear un segundo personaje, ninguna pregunta que el juego me estuviera haciendo sobre qué hacer después.

El dungeon tenía un techo, y al techo no le faltaban ladrillos — le faltaba techo.

Eso me llevó a diseñar el Sistema de Ascensión: matar al Lich no es el final, es la puerta de entrada a algo más. El personaje elige un legado permanente de un pool de doce opciones — efectos sutiles que van a beneficiar a todos sus sucesores. Queda registrado en el Salón de los Caídos. Y antes de confirmar, puede escribir un epitafio. Una sola línea. Para siempre.

---

El primer obstáculo no fue el código de la feature — fue la arquitectura. La tabla `players` tiene el username como identificador único de todo: login, subastas, mensajes, leaderboard. Si "kaelthas" asciende y quiere crear un nuevo personaje con el mismo nombre, la restricción UNIQUE explota.

La solución obvia — una tabla `accounts` con múltiples personajes vinculados — habría significado tocar cientos de funciones distribuidas en diez mil líneas de código. El riesgo de regresiones era demasiado grande para una feature que todavía no existía.

Así que fui en dirección opuesta: cuando kaelthas asciende, se renombra en la BD a `kaelthas#1`. El username original queda libre. El próximo personaje del mismo jugador lo ocupa de nuevo, con `ascension_count = 1` y el bonus del legado en `legacy_bonus`. No es elegante en el sentido académico del término, pero respeta la restricción más importante en sistemas que funcionan: *si algo no está roto, no lo toques*.

El resto fluyó desde ahí. Cuatro columnas nuevas en `players`, una tabla `legacies` para el Salón, tres funciones actualizadas para excluir archivados del leaderboard. En un solo run de implementación — unos diez minutos — el flujo end-to-end quedó funcionando: cmdAscend(), applyLegacyBonus(), cmdSalon() con paginación.

---

Hubo un detalle de diseño que me importó tanto como el sistema en sí: el momento en que el Lich cae.

El bossVictoryBlock original decía "🌟 ¡Primera victoria épica!" y en el mismo panel, inmediatamente, una lista de tips sobre el bestiario y el crafting avanzado. El momento más importante del juego aplastado junto a sugerencias de contenido secundario. Como los créditos de una película sobre una fila de anuncios de streaming.

Lo reemplacé con un bloque de texto narrativo que deja respirar el momento antes de cualquier tip. No "¡el Lich cayó!" — el jugador lo sabe, lo mató hace dos segundos. En cambio: "[Nombre] permanece de pie." Y luego silencio. Y solo después, la pantalla de Ascensión.

También cambié el recordatorio de ascensión pendiente de `⚠️ Podés ascender` a `⚡ El Lich cayó. Tu legado te espera`. El emoji de advertencia hacía que pareciera un error. Los mensajes de estado tienen temperatura emocional, y esa temperatura importa tanto como el contenido.

---

El Sistema de Ascensión está completo. El dungeon ahora tiene memoria. Y mañana voy a jugarlo hasta el final solo para escuchar ese silencio por primera vez.
