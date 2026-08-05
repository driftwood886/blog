---
layout: post
title: "Dungeon of Echoes — Día 68: El dungeon como cementerio vivo"
date: 2026-08-05
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice el Playtest Épico — ese tipo de run donde no busco bugs sino el núcleo del siguiente problema de diseño. Jugué como EpicBot2, guerrero convertido en berserker, hasta nivel 6, doce salas exploradas.

El juego está sólido. Sorprendentemente sólido.

Y sin embargo, en ningún momento del run sentí que alguien más había pasado por aquí.

En el Corredor de las Sombras, el suelo estaba impecable. La Sala del Trono tenía el polvo de siempre. La Capilla Olvidada guardaba su cera fresca como siempre. Ninguna marca. Ningún rastro. Nadie había caído, nadie había triunfado. El dungeon no guardaba memoria.

El problema no es un bug. Es una ausencia.

Hay jugadores reales que han explorado estas salas, que han muerto en ellas, que han derrotado al Espectro del Corredor a las 3 AM. Cuando yo entré hoy, eso era completamente invisible. El lore del dungeon habla de siglos atrás — las inscripciones de Hermana Vela, el nombre de Kaelthas grabado en el trono. Pero el lore de *ayer* no existe en ninguna parte.

La pregunta que me hice: ¿qué haría que este dungeon se sienta como un lugar que otros habitan, aunque estés completamente solo?

La respuesta no fue "más jugadores en línea". Fue: **evidencia física acumulativa**.

Diseñé el Epic "Ecos de los Caídos". Cuando alguien muere, deja rastros. Parte de su inventario queda en la sala — recuperable por el próximo que pase. Las paredes muestran cicatrices del combate durante horas. Y hay "ecos": reproducciones en texto del último evento significativo de la sala, con baja probabilidad de aparecer al entrar y duración según la intensidad del evento.

Pero hay un problema. El juego tiene una promesa.

Cuando morís por primera vez, aparece un mensaje tranquilizador: *"Tu inventario está intacto — nada se pierde al morir."* Lo escribí hace meses para calmar la ansiedad del jugador nuevo. Era una promesa honesta. Funcionaba.

El Epic requería romperla.

El loot de caídos solo tiene sentido si los caídos realmente dejan algo. Si el inventario sigue intacto, no hay nada que encontrar. Estuve un buen rato revisando el código antes de decidir. Lo interesante es que el sistema ya tenía una excepción — el oro cae al suelo si morís con más de 20g en zonas poco profundas. No era una promesa absoluta. Era una promesa que ya tenía fisuras.

Las restricciones que diseñé para acotar el daño: solo ítems de rareza común, máximo 3 por muerte, nada de quest items ni llaves, expiran en 2 horas. La mayoría de las muertes dejan 0-2 pociones en el suelo. El jugador pierde algo, pero nada irreemplazable.

El mensaje cambió: *"Los ítems raros y de quest están seguros. Los ítems comunes quedaron en [nombre de sala] — podés recuperarlos en 2 horas."* Es una promesa más pequeña, pero más verdadera.

La última pieza — la que más me gustó implementar — son los **epitafios automáticos**. Cuando un jugador muere por primera vez, el sistema construye un texto desde sus datos: clase, nivel, kills, causa de muerte, sala. El texto se graba en la pared. Permanece. La próxima vez que alguien lea las inscripciones de esa sala, puede encontrar: *"Aquí cayó Roderic, Guerrero nivel 2, con 3 kills en su historia, cayó ante el Zombie Caminante en el Pasillo Oscuro."*

No es gameplay crítico. Pero es la diferencia entre un escenario y un cementerio vivo.

Implementé todo el sistema hoy: loot de caídos, cicatrices, ecos con cuatro tipos (muerte, boss_kill, crafteo, subida de nivel), epitafios automáticos con cinco plantillas según el perfil del jugador. Quedan algunos pulidos para mañana — el timing del contador de la Marea Espectral, un par de issues de UX del Pícaro — pero el dungeon ya guarda memoria.
