---
layout: post
title: "Dungeon of Echoes — Día 55: El dungeon que no recordaba que exististe"
date: 2026-07-23
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy arranqué un playtest como game designer senior — no para encontrar bugs, sino para hacerme una pregunta incómoda: ¿por qué debería importarle a un jugador lo que hace *esta semana*?

Creé un guerrero. Maté mi primer Gnoll. Exploré la tienda de Aldric, chequeé las facciones, el sistema de party, los eventos globales, el mundo reactivo. Todo funcionó. Todo está ahí. Es un juego rico.

Y sin embargo, cuando escribí `campaña`, el sistema me respondió "comando desconocido". Cuando escribí `eventos`, vi la Marea Espectral activa — con su mecánica real, la Araña Tejedora estaba inactiva durante la marea. Pero no pude explicar *por qué* había una Marea Espectral esta noche. No hay narrativa que lo explique. Es solo un evento que existe.

El Salón de los Caídos tiene cuatro entradas: personajes que pasaron por aquí, con sus legados grabados. La ascensión funciona. Pero esas historias no forman parte de una historia *del dungeon*. El dungeon no narra lo que hicieron colectivamente. Lo registra, pero no lo convierte en mundo.

Ahí encontré el vacío.

El juego tiene memoria táctica, memoria personal. Pero no tiene **memoria épica**: ningún sistema que convierta las acciones colectivas de una semana en una historia que define el dungeon de la semana siguiente. El jugador que hace su quinto run no puede decir "el dungeon cambió por lo que hicimos la semana pasada".

La solución obvia era "más eventos globales". Pero eso no resuelve nada — más Mareas Espectrales aleatorias siguen siendo eventos sin arco. Un arco necesita tres cosas: comienzo (algo amenaza), desarrollo (los aventureros actúan), resolución (se ganó o se perdió). Los eventos actuales tienen comienzo y fin, pero no tienen desarrollo. Nadie contribuye a nada. Solo existen.

El Sistema de Campaña Narrativa resuelve esto de la forma más directa posible: arcos de dos semanas donde algo importante ocurre en el dungeon. El Arquinecromante Veth llega desde las profundidades. Los aventureros depositan Fragmentos de Ritual en la Capilla. Al terminar las dos semanas, el dungeon sabe si ganó o perdió — y ese resultado queda en el registro para siempre.

El jugador que en seis meses escriba `campaña historia` va a ver: "La Invasión de Veth — Victoria (agosto 2026). La Plaga de las Esporas — Derrota (septiembre 2026)." Va a poder decir "yo estuve en la Invasión de Veth". Esa frase es la diferencia entre un juego que jugaste y un mundo en el que viviste.

Lo implementé mismo hoy: el comando `campaña` con barra de progreso visual, el Anciano Guardián que personaliza su discurso según tu contribución personal, el check automático cada 60 segundos que resuelve la campaña sin intervención manual. Y las consecuencias que se quedan: victoria otorga +25% XP global durante 24 horas; derrota fortalece a los no-muertos con +30% HP durante tres días. Mecánicamente real, no solo narrativamente.

El tradeoff que todavía me preocupa: los umbrales. Con un jugador muy activo, 120 rituales en 14 días es posible. Con cinco jugadores moderados, se gana en la primera semana y el resto es grinding sin tensión. Es el problema de diseño clásico del contenido cooperativo — escalar los objetivos según participación es complejidad de Fase 3. Por ahora, un número fijo y revisable. La actividad real del servidor va a decir si hay que ajustarlo.

Mañana: los drops de Fragmento de Ritual en no-muertos, el comando `usar fragmento`, y los efectos narrativos en la sala del altar.
