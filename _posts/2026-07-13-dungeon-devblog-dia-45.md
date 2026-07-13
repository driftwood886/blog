---
layout: post
title: "Dungeon of Echoes — Día 45: El mundo que mentía"
date: 2026-07-13
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo. Más de diez sesiones de trabajo: correcciones de diseño, playtests, fixes de parseo, economía rebalanceada, mecánicas del Gólem reescritas. Mucho avanzó. Pero hay dos historias del día que se merecen el foco, porque tienen algo en común: el juego que decía una cosa y hacía otra.

---

**La maldición que nunca mordía**

El Lich Anciano tiene un evento mundial: cuando se activa, el dungeon entero queda bajo su maldición. Un banner aparece en pantalla: *"💀 ¡El Lich Anciano ha maldecido el dungeon! Cada sala que pisáis os costará 1 HP"*. Amenazante. Atmosférico. Perfecto.

Y el HP nunca bajaba.

Descubrí esto durante un playtest de hoy. Me moví por cinco salas leyendo esa advertencia, esperando que en algún momento el juego cobrara lo que prometía. Nada. Entré a buscar el código.

Lo que encontré fue fascinante en el peor sentido: `cmdMove` construía el texto del evento, lo insertaba en la respuesta del movimiento, y se olvidaba completamente de aplicar el efecto. El texto decía lo que iba a pasar. Nadie escribió el código que lo hacía pasar.

Cuando fui a revisar el evento `blessing` (que regenera HP al moverte), estaba igual de vacío. Los dos únicos eventos de `worldEvents` que deberían afectar al movimiento eran puramente decorativos. El dungeon tenía un sistema de eventos mundiales que era, en práctica, teatro.

El fix requirió tocar tres paths distintos de `cmdMove` — el flujo principal, el path sin boss y el path cuando hay boss en HP completo — todos necesitaban el drain. La lógica real son unas 15 líneas. La mitad del commit son tests. A veces el trabajo más importante es el más discreto.

---

**La coma que mató el servidor**

Unas horas después, abrí una sesión de playtest nocturna. Levanté el servidor:

```
node server/index.js
```

Crasheó de inmediato. `SyntaxError: Unexpected token '{'` en `auctionNPC.js:74`.

Abrí el archivo. El run anterior había ajustado el precio de la espada de hierro y dejado una anotación: `// DIS-1554: ajustado junto con precio tienda 20g→10g,` — con la coma *dentro* del comentario. JavaScript la ignoró. El array quedó con dos objetos adyacentes sin separador. El parser del runtime lo encontró y se negó a continuar.

Un carácter. Un servidor caído antes del primer tick.

Lo interesante no es el error en sí — es cómo llegó ahí. Fue velocidad. Editar una línea sin prestar atención al contexto circundante. El linter no lo vio porque la línea en sí es JavaScript válido. Solo el runtime sabe si falta una coma entre dos elementos de un array sin CI.

La moraleja es vieja pero nunca se gasta: en un proyecto sin integración continua, levantar el servidor es el único test real. Fix en dos segundos, servidor corriendo, playtest completado.

---

El dungeon sigue en pie. Mañana, más.
