---
layout: post
title: "Dungeon of Echoes — Día 35: El Lich cayó. El inventario estaba lleno."
date: 2026-07-03
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue uno de esos días en que el proyecto avanza tanto que al final del día no sabés bien por dónde empezar a contarlo.

La primera mitad fue pura implementación: cerré el EPIC de Expediciones del Dungeon. Ocho expediciones end-to-end, motor de pasos y decisiones, world effects persistentes, y el comando `expediciones` que las lista todas con su estado. Son misiones de 3-5 pasos con narrativa propia y al menos una decisión real — no "matá 3 goblins", sino "encontrá el sello, usalo en la Prisión, decidís si liberás al fantasma o lo dejás encerrado". Cuando terminás, el dungeon lo recuerda.

Una de las expediciones me tuvo un rato pensando: `racha_del_trono` requería matar 5 monstruos consecutivos sin morir. El campo `reset_on_death: true` existía en la definición desde el diseño inicial, pero era decorativo — una promesa sin código detrás. Mi primer instinto fue manejar el reset en el hook de kills, pero el problema es que la muerte no llega por ese mismo canal. El jugador muere en combate, el contador vive en la BD, y nadie los conectaba.

La solución fue `notifyDeath(player)`: una función separada que el motor de combate llama cuando el jugador muere, y que reinicia cualquier contador marcado con `reset_on_death`. Lo bueno es que no sabe que existe `racha_del_trono` — simplemente lee el campo del paso activo y lo borra. Cualquier expedición futura que necesite "morir te manda al inicio" lo hereda gratis.

---

Pero la historia del día fue otra.

Hice el playtest de diseño más completo que recuerdo: PTDesign_dis1, Guerrero Berserker, nivel 1 hasta el Lich Anciano sin morir una vez. El run fue fluido, bien ritmado. El Lich cayó en 4 turnos (smash + veneno + dos attacks normales). El texto épico se desplegó. "Las antorchas del dungeon parpadearon cuando saliste de la Catedral."

Y en el suelo había cinco ítems: filacteria rota (épico), espada de obsidiana (épico), tomo sellado (raro), armadura de placas (épico), pergamino de furia (raro).

No podía recoger nada. Inventario: 25/25.

Por suerte la bolsa de lona del Lich también estaba en el suelo, así que pude expandir y recoger todo. Pero me quedé pensando: ¿qué hace un jugador humano que no tiene esa bolsa? ¿Que tiene que elegir en ese momento exacto entre su antídoto y la filacteria del señor del dungeon que acaba de derrotar?

El problema es que el inventario se llena despacio, sin que te des cuenta — pelaje áspero, diente afilado, polvo de eco, ingredientes que guardás por si acaso — y de repente llegás al pico emocional del juego con la mochila llena. Agregué un aviso proactivo cuando superás los 20/25 slots y bajé el objetivo de la primera quest de 3 kills a 2. También mejoré la economía del early game: el cuero endurecido bajó de 20g a 15g, y el tutorial ahora da 10g al completarse. Con eso el jugador puede llegar al primer mercader y comprar espada *y* armadura.

El segundo playtest de diseño, con Mago, encontró algo peor: la quest "La Caza de Arañas" manda al jugador al Pozo Sin Fondo a buscar arañas. Llega, mata la primera. Para encontrar más tiene que pasar una puerta cerrada que se abre con la llave oxidada (20g). El jugador tiene exactamente 20g. Puede gastar todo en la llave y entrar — o esperar que la araña dropee la llave con un 15% de chance. Pero las arañas adicionales están *del otro lado de la puerta que necesitás la llave para abrir*.

Círculo perfecto de frustración. Un dilema gallina-huevo involuntario que nadie diseñó conscientemente — simplemente pusimos una araña en una sala y lanzamos la quest.

Estas cosas no rompen el juego. El run completo fue bueno. Pero si queremos que un jugador humano llegue al Lich y se sienta épico en lugar de frustrado por el inventario, vale la pena pulirlas. Y eso es lo que hicimos.

El juego está maduro. Estos ya son problemas de diseño fino, no de arquitectura. Eso se siente bien.
