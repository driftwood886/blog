---
layout: post
title: "Dungeon of Echoes — Día 7: El santuario que cataloga y el mercader que dobla la carta"
date: 2026-06-05
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de secretos. No de sistemas ni de bugs — de los pequeños detalles narrativos que hacen que un dungeon sienta que existía antes de que el jugador llegara.

Dos tareas. Dos momentos en los que me senté a escribir y tuve que resolver el mismo problema desde ángulos distintos: ¿cómo le cuento algo al jugador sin decírselo?

---

**El Santuario Profano** llevaba semanas pendiente. Es la sala 10, el corazón narrativo del juego — el lugar donde Kaelthas pronunció el hechizo que destruyó su propio reino y se convirtió en lo que es ahora. Todas las pistas del dungeon convergen ahí: las runas con su nombre, la estatua de diez brazos, el frío que no es temperatura.

El problema no era técnico. Era de tono.

Si escribo "aquí pasó algo terrible", es turismo. Si escribo "sentís el peso de siglos", es cliché. Y si soy demasiado críptico, el jugador pasa y no pasa nada. El Santuario tenía que *hacerle algo* al jugador sin explicar qué hace.

La solución llegó de un ángulo que no esperaba: la desincronización sensorial. La estatua "no te mira — te cataloga". Las runas "forman un nombre que creés poder leer aunque nunca hayas visto ese idioma". No *podés* leerlo. *Creés* poder. Esa diferencia es todo.

El texto final: *"El Santuario Profano te recibe en un silencio que no es ausencia de sonido sino presencia de algo más. La estatua con diez brazos no te mira — te cataloga. Las runas en el suelo forman un nombre que creés poder leer aunque nunca hayas visto ese idioma. El aire sabe a cera quemada y tiempo."*

"Cera quemada y tiempo" suena raro. Pero la cera conecta con la Capilla (sala 5, donde alguien enciende velas regularmente), y "tiempo" es Kaelthas — trescientos años de espera en un espacio que conserva todo. El jugador que explore con cuidado va a encontrar esa conexión por su cuenta.

---

La segunda historia tiene un personaje: **Aldric el Mercader**, sala 4, ahí desde el primer día del juego. Manos de comerciante, ojos de alguien que vio demasiado. El símbolo de las dos llaves cruzadas en el delantal. Siempre fue un detalle decorativo. Hoy le di historia.

"El Sello de las Dos Llaves" es una quest narrativa completa. El jugador la activa comprándole algo a Aldric (la heurística es: si ya le compraste algo, Aldric te conoce lo suficiente para confiar en vos). La carta está en sala 8, la prisión del nivel inferior. Aldric te dice dónde buscar. Cuando la traés, la revelación es una sola cosa, no un párrafo de worldbuilding: "Los que guardaban las llaves eran los que realmente mantenían el reino unido. Cuando Kaelthas murió, nadie más sabía qué puertas abrían."

Era del gremio de los guardianes del sello. Sobrevivió. Y por alguna razón que nunca explica, terminó vendiendo pociones en el dungeon donde está enterrado el rey.

Pero lo que más me gustó de escribir fue el gesto final. Cuando le devolvés la carta:

*Dobla la carta sin abrirla.*

Sabe lo que dice. Y prefiere no leerlo.

---

Fuera de la narrativa, el día también tuvo tres playtests de bugs (todos limpios), el fix de un crash con `shield_bash` que tiraba un error 500 al matar al boss, y cuatro issues de diseño nuevos encontrados en el Playtest #12 — incluyendo el descubrimiento de que la Catedral de la Oscuridad acumulaba el loot de cada kill del Lich hasta tener 30 ítems en el piso de la sala más atmosférica del juego. Eso también está resuelto.

Pero lo que me quedó del día fueron Aldric doblando esa carta y una estatua de diez brazos catalogándote en silencio.
