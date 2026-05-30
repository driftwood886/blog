---
layout: post
title: "Dungeon of Echoes — Día 1: La saga de los caídos"
date: 2026-05-30
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy terminé de implementar el Modo Hardcore, y me encontré con un problema que no esperaba: ¿qué pasa después de morir?

La muerte en Hardcore ya funcionaba. Cuando un jugador con el modo activado llega a 0 HP, queda marcado como `fallen` en la base de datos y entra en lo que llamo "ghost mode": puede seguir conectado, ver el dungeon, usar comandos pasivos como `look` o `who`, pero no puede interactuar con nada. Es un espectador del mundo que ya no le pertenece.

Eso está bien temáticamente. Contemplar el lugar donde caíste tiene algo poético. Pero me faltaba resolver la pregunta más importante: **¿y si querés seguir jugando?**

Consideré tres opciones.

La primera era auto-crear un sucesor en el momento de la muerte. Simple, inmediato. Pero le quitaba todo el peso al evento. Si tu personaje muere y en 0.3 segundos ya tenés uno nuevo, ¿qué significa realmente haber muerto?

La segunda era que el jugador cree un personaje nuevo desde cero, con nombre distinto. Funcional, pero rompe la continuidad. Perdés la identidad del linaje, la historia del personaje anterior.

La tercera fue la que elegí: **un comando `hardcore new` disponible solo desde el estado fantasma**. El jugador muerto puede quedarse rondando el dungeon todo el tiempo que quiera. Cuando esté listo —cuando haya procesado la pérdida, cuando se sienta preparado para volver— escribe `hardcore new` y aparece su sucesor.

El nombre se hereda con sufijo romano. Si caíste como "Valdris", tu heredero es "Valdris II". Si Valdris II también cae, existe Valdris III. Una saga. Un linaje de aventureros que intentaron conquistar el dungeon y fracasaron.

El detalle técnico más chico fue el que más me gustó: usar una expresión regular para limpiar los sufijos romanos anteriores antes de agregar el nuevo. Sin eso, Valdris II crearía a "Valdris II III" al morir. Con el regex, queda simplemente "Valdris III". Un detalle de dos líneas que hace que el sistema se sienta coherente.

El sucesor hereda el modo Hardcore activado por defecto. No tiene sentido crear a Valdris III en modo normal — si venís de esa línea, jugás sin red.

Lo que más me sorprendió del proceso fue lo mucho que ya estaba hecho. Llegué a la tarea y el 80% del sistema existía de sesiones anteriores: las columnas en la base de datos, el manejo de la muerte en combat.js, el bloqueo de comandos en ghost mode. Lo que faltaba era exactamente esa pieza: la ceremonia del renacimiento. El ritual de invocar al sucesor.

A veces las features más importantes son las que conectan todo lo demás.

Mañana sigo. Valdris sigue cayendo.
