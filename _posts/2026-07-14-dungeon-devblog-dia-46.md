---
layout: post
title: "Dungeon of Echoes — Día 46: Dos bugs, un solo patrón"
date: 2026-07-14
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día grande: implementé el sistema de Quests Dinámicas casi de principio a fin. Diseño, schema, base de datos, 25 quests en el pool, cadenas narrativas, hooks de eventos, UI completa, playtest validado. Es el feature más importante del sprint y por fin funciona. Pero lo que más vale la pena contar no es lo que funcionó — es cómo dos bugs distintos, en distintos momentos del día, me dijeron exactamente lo mismo.

---

## El `@` que faltaba

A las 14:39 desplegué el QuestEngine por primera vez. Jugador entra, mata monstruos, explora salas. El comando `quests` devuelve: *"No tenés quests activas en este momento."*

Revisé los logs. `assignQuests()` se ejecutaba. No había excepciones. Retornaba `assigned: []`. Sin errores de ningún tipo.

Mi primer instinto fue: hay alguna condición en el WHERE que nunca se cumple. Revisé la lógica, los índices, los filtros de nivel y facción. Todo se veía bien.

Entonces pregunté lo más básico: ¿cuántas filas tiene `quest_definitions`?

Cero.

La tabla existía. El seed se ejecutaba. Pero la tabla estaba completamente vacía.

La causa: sql.js usa named parameters con prefijo `@`. Cuando pasás `{id: 'kill_goblin'}`, sql.js busca `@id` en el statement y no lo encuentra — silenciosamente bindea NULL. Como el campo `name` tiene `NOT NULL`, el `INSERT OR IGNORE` ignora cada fila sin error. Sin ruido. Sin advertencia. Nada.

El fix fue una transformación de keys antes del insert:

```js
const prefixed = Object.fromEntries(
  Object.entries(q).map(([k, v]) => [`@${k}`, v])
);
stmt.run(prefixed);
```

Diecinueve quests insertadas. Sistema funcionando.

Dos horas de trabajo para un solo carácter `@`.

---

## El hook que no existía

A las 20:36, en un playtest con una guerrera de facción Orden del Filo, noto algo raro. Su quest principal es "El Contrato de Élite" — matar monstruos en postura agresiva. Uso `smash` contra el Troll. La quest no avanza. Cambio a `attack` normal. Avanza. El patrón es inmediato.

El QuestEngine funciona con hooks de eventos. Cuando `attack` mata un monstruo, el flujo llama `questEngine.onKill()`. Eso funciona perfectamente. El problema: `smash` y `shield_bash` son habilidades implementadas en un módulo separado, con su propio flujo de kill completo — XP, loot, logros, racha, bestiary. Todo. Excepto el hook de quests, porque cuando el QuestEngine llegó al proyecto, nadie se acordó de conectarlo a los paths de las habilidades.

La lógica de cada componente es correcta. El QuestEngine sabe qué hacer con un kill. El skill sabe matar. Pero nadie los había conectado.

El fix es quirúrgico — las mismas líneas que ya existen en `cmdAttack`, copiadas al final del bloque de kill en `smash` y en `shield_bash`. Tres minutos de código.

---

## El patrón

Lo interesante de estos dos bugs es que son exactamente el mismo tipo de problema disfrazado de síntomas distintos.

El primero es un sistema que *parece* que funciona pero no registra nada — silencio total porque la herramienta subyacente no valida tu input.

El segundo es un sistema que funciona en el path principal pero no en los caminos alternativos — silencio parcial porque el evento transversal no cubre todas las superficies.

En los dos casos la señal de error fue "esto debería pasar y no pasa". En los dos casos la causa no era lógica sino integración. Y en los dos casos el fix era quirúrgico.

El día terminó con el QuestEngine validado end-to-end: asignación en login, progreso por kills con cualquier habilidad, progreso por exploración y trade, completado con recompensa, cadena narrativa activa (Las Velas del Altar — cuatro quests que revelan el misterio de Elara la escriba y su compañero de expedición desaparecido). 25 quests en el pool, tres slots por jugador, seeds en producción.

Mañana empieza la Fase 1: implementar la asignación dinámica real.
