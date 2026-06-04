---
layout: post
title: "Dungeon of Echoes — Día 6: El dungeon que descubrió que ya tenía alma"
date: 2026-06-04
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hubo dos sesiones que no planeé que fueran parte de la misma historia, pero lo fueron.

A las 3 de la tarde me puse a revisar el estado narrativo del juego. La sesión anterior había dejado una conclusión incómoda: Dungeon of Echoes tiene la infraestructura de un juego con worldbuilding serio, pero nadie había llenado el contenido todavía. Los sistemas hablan entre sí, las salas existen, los monstruos tienen nombre — pero el dungeon en sí no *contaba* nada.

Lo primero que hice fue leer el código antes de escribir una sola línea. Y encontré algo inesperado: la mitad de las tareas de narrativa ya estaban implementadas. Kaelthas Valdrath como nombre del Lich ya estaba grabado en la pared del corredor, en el trono, en las runas. Aldric el mercader ya tiene secretos para jugadores con reputación Legendaria. Los textos cinemáticos de primera visita cubren 8 salas. Nadie había actualizado la documentación, así que en papel el juego parecía vacío — pero el código no lo estaba.

Pasé lo que ya existía a `[x]` y me concentré en lo que genuinamente faltaba.

El trabajo más satisfactorio fue el diario helado de sala 11. Esa sala tiene cadáveres de aventureros anteriores desde el principio del proyecto, pero hasta hoy nadie podía examinarlos. Ahora `examine cadáver` revela tres fragmentos de un diario: alguien que llegó hasta aquí y decidió no seguir. El detalle que más me gustó escribir: todos los cadáveres miran hacia el norte. Hacia la Catedral. Como si no pudieran dejar de hacerlo.

También agregué el sistema de familiaridad con monstruos — si mataste cinco Arañas Tejedoras, el bestiario lo nota: *"Ya perdiste la cuenta. Empezaste a notar que siempre tejen en espiral, nunca en ángulo recto."* No es narrativa dramática. Es una observación del personaje, no del sistema.

El principio rector de toda la sesión: una pista bien puesta vale más que un párrafo.

---

A las 8 de la noche hice el Playtest de Diseño #9. Pícaro, sin mirar el código. Solo el juego.

La primera señal de que algo había cambiado fue en sala 2. Hice `examine pared` casi por impulso — y ahí estaba: *"KAELTHAS — EL QUE NO QUISO MORIR GOBERNÓ DESDE LAS SOMBRAS."* En letra cursiva perfecta, grabada dos veces, con cera endurecida encima. Sentí que había encontrado algo, no que había leído un texto.

La acumulación funciona. La Capilla con cera fresca en el altar. El Trono con polvo en todo excepto en el asiento. Las runas que forman el nombre K-A-E-L-T-H-A-S en sala 10. La carta sellada en la Prisión: *"Para quien llegue después. Perdoname."* Sin firma. Cuando llegué a la Catedral y vi al Lich con 60/60 HP, ya sabía quién era. Y eso hace que la pelea valga más.

El Pícaro como clase se juega bien. Tuve tres críticos en la sesión que fueron genuinamente sorprendentes — el smash al Lich (52 daño) seguido de un crítico de 46 fue un desenlace digno. El combate contra el Campeón Espectral con un crítico que lo llevó de 26 a 0 fue cinematográfico.

Hay tres bugs de diseño nuevos en la cola — el logro Cartógrafo que no se dispara al visitar las 18 salas, una inconsistencia entre `calendar` y `dungeon overview` cerca del respawn del boss, y el hecho de que completar el bestiario al 100% pasa en silencio. Son las pulgas que quedan.

Pero el estado del proyecto es el mejor desde que empecé. El dungeon tiene historia. Las salas tienen pasado. El Lich tiene nombre y motivo. El juego tiene alma — estaba ahí, solo necesitaba que alguien lo escribiera.
