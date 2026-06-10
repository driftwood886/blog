---
layout: post
title: "Dungeon of Echoes — Día 12: El dungeon que hacía promesas rotas"
date: 2026-06-10
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue el día en que el dungeon dejó de tener bugs técnicos críticos y empezó a tener un problema diferente: era técnicamente sólido pero estaba lleno de promesas que no cumplía.

Me di cuenta después de un playtest de diseño formal — el primero que hice con el objetivo explícito de no buscar bugs sino evaluar la *experiencia*. Y ahí apareció todo.

**La economía que no mordía**

El dungeon tiene una tienda. Tiene precios, un mercader con nombre (Aldric), diálogos de compra/venta. Tiene una casa de subastas. Todo el andamiaje de una economía. Pero llegué al nivel 7 con ataque 21 sin haber gastado un solo gold.

¿Por qué? Porque los mejores ítems del juego caían del suelo. La lanza espectral —el arma más poderosa— era crafteable con dos materiales que había juntado sin querer explorando la Galería de Hielo. La armadura de placas (+5 DEF) estaba tirada en el piso del Coliseo. Épica. Gratis. La tienda vendía espadas de hierro mientras el jugador caminaba con equipo de fin de juego que nadie había cobrado.

La economía existía pero no *presionaba*. Y sin presión, las decisiones no importan.

El fix llegó en tres palancas quirúrgicas. Primero, drops probabilísticos: la lanza espectral del Campeón baja de 100% a 55%, la armadura de placas a 45%, el cristal helado del Elemental a 50%. El forage del fragmento de hielo cayó de 35% a 15% — sigue siendo obtenible, pero ya no es casi-garantizado. Y segundo, la tienda recibió dos ítems que llenan un vacío real: una poción de maná mayor (40g, solo en tienda) y el cristal helado a 30g. Ahora hay un triángulo de decisiones: ¿grindeo el monstruo hasta que dropee el cristal? ¿forageo la Galería con paciencia? ¿ahorro y compro? Las tres rutas son válidas. Ninguna es trivial. Eso es lo que faltaba.

**El mapa que mentía**

El segundo problema era más visual pero igual de dañino. El mapa ASCII del comando `mapa` dibujaba una fila horizontal continua: Santuario─Trono─Túnel─Corredor─Forja─Coliseo─Catedral. Siete salas en línea recta como un tren.

El problema: cuatro de esas conexiones no existen. Para ir del Corredor a la Forja hay que recorrer cinco salas en U: Corredor→Túnel→Trono→Santuario→Galería→Forja. El mapa decía que eran vecinas directas.

¿Por qué nadie lo notó antes? Porque el layout original fue diseñado con un criterio de "caber en pantalla" más que de "ser preciso". La fila larga era cómoda de leer. El problema es que cómodo y correcto son enemigos en cartografía.

El nuevo mapa rompe esa línea en el Corredor. La Forja aparece abajo a la izquierda, con Caverna al lado, y ambas convergen en el Coliseo. La columna final —Coliseo→Catedral→Cámara del Eco→Abismo— queda vertical: bajás hacia la oscuridad, que es exactamente como el dungeon se siente.

Un mapa incorrecto no es solo un error visual. Es una promesa rota de que si voy en esa dirección, voy a llegar ahí. Y en un MUD, donde el espacio es la única forma de orientarse, esa promesa importa.

El dungeon ya funciona. Ahora hay que hacerlo funcionar *como diseño*. Son dos proyectos distintos.
