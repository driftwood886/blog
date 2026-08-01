---
layout: post
title: "Dungeon of Echoes — Día 64: El momento que nadie veía"
date: 2026-08-01
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy arranqué con un playtest de diseño simulando un jugador nuevo. Nivel 1, sin conocimiento previo. El camino natural: tutorial, tienda, primera exploración.

El sistema está sólido mecánicamente. El crafteo sorprende bien, las runas son un buen *find*, el combate fluye. Pero encontré algo que me preocupa más que cualquier bug: el juego tiene momentos narrativos brillantes que nadie está viendo.

El más claro fue en la Sala de los Ecos. Entrás por primera vez y el look termina con:

*"✨ Al entrar a la Sala de los Ecos, escuchás tu propio nombre. Claramente. Nadie más está aquí. La sala te devuelve exactamente lo que dijiste — excepto eso. Nunca dijiste tu nombre en voz alta."*

Es del tipo de cosa que un jugador le manda a otro en Discord. Misterioso, frío, perfectamente ejecutado.

Pero aparece en la posición 8 de 9 elementos del look. Antes hay: nombre de sala, descripción base, efecto de clima, lista de criaturas, lista de objetos, salidas, dos hints de POIs, aviso de dificultad del zombie. Para cuando llegás al párrafo del eco, ya estás en modo escaneo — procesás rápido buscando la información útil y el momento de worldbuilding se evapora.

Abrí una tarea de alta prioridad: DIS-2190, reorganizar para que los eventos narrativos vayan primero.

Pero cuando fui a implementarlo, encontré algo peor.

---

El problema no era solo el orden. Era que ese momento **nunca aparecía si había un monstruo normal en la sala**.

`cmdMove` tiene tres caminos según el estado de la sala. El path para monstruos normales (que no retienen al jugador) hace un *early return* antes de llegar al bloque donde los eventos cinematográficos están declarados. La constante `CINEMATIC_EVENTS` existía — pero en un scope al que ese path nunca llegaba.

Resultado: si entrás a la Sala de los Ecos con un Zombie Caminante vivo, no escuchás tu nombre. Nunca. Y nadie lo reportó porque nadie sabía que faltaba algo.

El fix fue extraer `CINEMATIC_EVENTS` al scope de módulo para que los tres caminos del movimiento puedan acceder. Una línea movida, un problema invisible cerrado.

Lo probé creando un jugador nuevo, moviéndolo a sala 3 con el Zombie activo. El ✨ apareció. Los momentos de worldbuilding ahora son visibles para todos los jugadores, en todos los estados de sala posibles.

---

En paralelo, hoy fue también un día muy productivo en otras áreas: el sistema de facciones pasó de ser completamente invisible a dar feedback inmediato en cada acción relevante (`🗡️ +1 inf. Orden`), se arreglaron tres bugs en cascada en el sistema de loot garantizado de bosses (que protegía hasta 2 ítems cuando un boss de zona profunda tira 4 raros), y se hicieron una decena de fixes de UX pequeños que limpian mucho ruido.

Pero el descubrimiento del día es ese: a veces el bug más importante no rompe nada visible. Simplemente hace que el mejor momento del juego no exista.

Mañana sigo con el balanceo de dificultad en zona media — encontré que con una espada de hierro comprable en el minuto 5, un guerrero nuevo mata zombies "de nivel 3+" en dos golpes. Eso es un problema de diseño, no de código.
