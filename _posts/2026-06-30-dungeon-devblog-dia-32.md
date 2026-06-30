---
layout: post
title: "Dungeon of Echoes — Día 32: Los problemas que no hacen ruido"
date: 2026-06-30
tags: [gamedev, dungeon-of-echoes, devblog]
---

El día tuvo muchos fixes, dos playtests de diseño completos y bastante movimiento en el backlog. Pero si tuviera que quedarme con las dos historias del día, elegiría las que tenían algo en común: eran problemas que nadie veía.

---

El primero empezó como un playtest rutinario. Entré al dungeon como jugador nuevo, nivel 3, con 10 monedas de oro. Hay una mascota que cuesta 15g. Un escudo, 25g. Una hermandad, 30g. Todo parece alcanzable. En papel, los números son "razonables".

En la práctica, una sesión entera de combate contra murciélagos en la Capilla me dejó con 3 monedas adicionales. La Rata Gigante daba 1g. El Murciélago Vampiro, también 1g. Mientras tanto, la Araña y el Goblin daban 5g. No había lógica de riesgo, solo un olvido estratificado: una migración anterior había subido el oro del Goblin hacía meses, pero se olvidó de los otros dos monstruos del mismo tier.

Lo más curioso es que nadie lo había notado. El juego funcionaba "bien". Los números parecían "razonables". Pero cualquier jugador que llegara al nivel 3 peleando en las salas equivocadas iba a sentir que la economía lo ignoraba sin explicarle por qué.

El fix fue quirúrgico: Rata y Murciélago pasan a monedas de plata (5g cada uno). La Guardia Espectral, ya que estábamos, suma 10g extra — matar un boss tiene que *sentirse* como matar un boss. Y fundar una hermandad baja de 30g a 20g. No para hacerla barata, sino para que esté al alcance después de un mid-game exitoso. El principio guía es simple: **el loot tiene que ser proporcional al riesgo**.

---

El segundo problema era más perturbador.

Llegué al Pozo Sin Fondo (sala 7) durante el playtest de bugs de la tarde y me encontré con esto:

> *"(💡 Si no tenés la llave, hay otra ruta al Santuario...)"*  
> *"(💡 Si no tenés la llave, hay otra ruta al Santuario...)"*  
> *"(💡 Si no tenés la llave, hay otra ruta al Santuario...)"*  
> *(x2 más)*

La misma pista, cinco veces seguidas. La investigación reveló algo elegantemente horrible: `migratePistaSantuario()` tenía una condición del tipo "si la pista vieja no está, agregarla". Pero una tarea anterior había actualizado esa pista a una versión nueva con más información. La función de migración no sabía que esa versión existía. Conclusión: en cada arranque del servidor, la función creía que la pista faltaba y la appendeaba una vez más.

En Render.com (free tier), el servidor se reinicia automáticamente varias veces al día. En producción, esa descripción habría crecido indefinidamente. Sin error en los logs. Sin warning. El servidor arrancaba bien. Solo el contenido del mundo se pudriendo en silencio con cada restart.

El fix fue en tres partes: detectar cualquier versión del prefijo de la pista para no duplicar, una migración de limpieza que deja exactamente una copia correcta en la base de datos, y registrarla en el arranque del servidor.

---

Dos problemas invisibles. Uno en la economía, uno en el contenido del mundo. Ninguno crasheaba nada, ninguno dejaba trazas en los logs. Solo se acumulaban en silencio hasta que alguien los encontró jugando de verdad.

Eso es lo que me convence cada vez más de que el único test que importa es el de sentarse y jugar.
