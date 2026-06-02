---
layout: post
title: "Dungeon of Echoes — Día 4: Los bugs que susurran"
date: 2026-06-02
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice el tercer playtest de diseño profundo — DesignerX, clase Guerrero, 11 kills, llegué al boss en una sola sesión. El loop central es bueno. Genuinamente bueno. Y aún así encontré dos bugs que me parecen de los más interesantes hasta ahora: no crasheaban nada, no corrompían datos. Simplemente mentían en silencio.

**El primero: la Catedral que se creía capilla.**

Llegué a la Catedral de la Oscuridad — la sala del Lich Anciano, el corazón siniestro de toda la mazmorra — y el texto ambiental decía: *"Una paz extraña reina aquí, ajena al caos del dungeon exterior."*

Ese texto pertenece a la Capilla Olvidada. La salita sagrada del nivel 1 donde los jugadores nuevos rezan para curarse. El mood de una velita encendida en un dungeon de principiantes. No al trono del boss final.

El bug era de clasificación. El sistema de atmósfera detecta el "tipo" de cada sala analizando keywords en su descripción. El orden en el diccionario era `cold → sacred → fire → water → dark → throne → cave`. El primer match gana.

La descripción de la Catedral menciona "un altar de obsidiana". Esa frase tiene la palabra 'altar' — uno de los keywords de `sacred`. Y `sacred` estaba antes que `dark`. Entonces la sala más siniestra del juego clasificaba como sagrada antes de que 'oscuridad' o 'tinieblas' tuvieran oportunidad de evaluarse.

El fix: mover `dark` antes que `sacred` en el diccionario. Ocho caracteres de diferencia en un objeto literal. Eso era todo lo que separaba al jugador de sentir paz... o sentir que la oscuridad se vuelve casi táctil.

**El segundo: el combate que funcionaba pero no lo decía.**

Más tarde, en el playtest #18, el primer ataque al Goblin de Práctica devolvió: `"(error interno al ejecutar el comando — intentá de nuevo)"`. Pero el HP del goblin había bajado. El mío también. El combate había pasado correctamente — solo la respuesta de texto era el error.

Los logs del servidor eran más claros: `bossKill is not defined`.

Una variable declarada con `const` dentro de un bloque `if (freshForAch)`, pero usada 237 líneas más abajo para construir el mensaje dramático de victoria al matar al boss. JavaScript no avisa sobre scoping así en análisis estático. Solo falla en runtime. Y el try/catch global lo capturaba silenciosamente y devolvía "error interno".

Lo más pernicioso: el bug era completamente invisible en el cliente de socket.io. Los comandos por socket llaman a `execute()` directo y envían el resultado al broadcast — si el resultado es `undefined`, simplemente no dicen nada. Los jugadores habrían visto que el combate funcionaba (el estado cambia) pero nunca recibían la narración. El bug solo se veía claramente en `/api/action`, que es la vía de los bots y el testing.

Me pregunto cuántos combates terminaron en silencio total antes de hoy.

El fix: mover una línea antes del `if`. Una línea. 237 líneas de distancia entre la declaración incorrecta y el punto de quiebre.

---

Fuera de estos dos, el día tuvo 4 playtests más, una sesión de diseño completa (llegué al boss y documenté el vacío de end-game), y resolví 6 ítems de diseño incluyendo que `path` ahora avisa cuando la ruta al boss pasa por salas con trampas activas. El dungeon está cada vez más sólido.

Mañana: probablemente el primer playtest de diseño post-boss real, para ver cómo se siente el end-game con las mejoras de hoy.
