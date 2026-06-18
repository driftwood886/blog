---
layout: post
title: "Dungeon of Echoes — Día 20: El que nunca sangra y el que llega con escudo sagrado"
date: 2026-06-18
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice tres playtests de diseño completos —Guerrero, Pícaro y Clérigo, nivel 1 hasta matar al Lich— y los tres me enseñaron algo diferente sobre lo que hace que un boss se sienta como boss.

El Guerrero llegó al Lich en nivel 9 con DEF 4. Sin armadura. La tienda de Aldric tenía cuero endurecido y cota de malla durante toda la run y nunca sentí que las necesitara. Los enemigos tempranos hacen tan poco daño que el HP acumulado por level-up compensa todo, y para cuando el Gólem empieza a pegar fuerte ya tenés 60 HP y el problema se resuelve solo. El resultado es que la armadura —que debería ser la segunda compra más importante del juego— pasa invisible. Lo resolví bajando el precio del cuero a 20g y haciendo que Aldric diga algo cuando vendés armas sin llevarte nada de defensa. Pequeño empujón, sin quitar libertad.

Pero el caso más interesante fue el Pícaro.

El Pícaro en su estado actual es el personaje más poderoso del dungeon. Y no de una manera emocionante —de una manera que hace el juego menos interesante. El loop es sigilo → golpe crítico garantizado ×2.0 → el monstruo queda aturdido → golpe sucio con veneno → uno o dos ataques más y el combate terminó. Para monstruos normales esto se siente *fantástico*. La sensación de control, de llegar siempre primero, de saber que el combate va a salir a tu favor: eso es lo que querés de un Pícaro.

El problema es que el Lich Anciano cayó antes de que llegara a su fase 2. El Gólem de Piedra —55 HP, resistencia física— sobrevivió exactamente dos ataques. El Guardia Espectral, recién balanceado hace tres días para durar al menos cuatro turnos, murió en tres.

El multiplicador ×2.0 de daño tiene sentido narrativo cuando emboscás a un guardia distraído. No tiene sentido cuando el objetivo lleva siglos en alerta siendo el guardián final del dungeon. Así que los bosses ahora resisten las emboscadas: crítico de sorpresa ×1.5 (era ×2.0), golpe sombra ×1.8 (era ×2.5). El Pícaro sigue siendo devastador contra el contenido normal —eso no cambia— pero los bosses ahora tienen tiempo de respirar y contraatacar antes de caer.

Y en el otro extremo del espectro, el Clérigo.

El Clérigo tiene el mejor momento de diseño del juego, y es la Cámara del Eco. Antes de enfrentar al Lich, usás el cuenco y, si sos Clérigo, el texto dice: *"Como Clérigo, el cuenco imbuye una bendición sagrada: los primeros 3 intentos de drenado de maná del Lich serán bloqueados."* No es solo un buff. Es un ritual. Llegás al boss final con un escudo preparado específicamente para lo que va a intentar hacerte. Y cuando el Lich intenta drenar tu maná y el juego responde *"la bendición del cuenco lo protege"*, sentís que tomaste la decisión correcta cuatro salas atrás.

Eso es lo que quiero para cada clase. El Pícaro llega en silencio. El Clérigo llega con escudo sagrado. El Guerrero —cuando terminemos de convencerlo de comprar armadura— llega con 8 DEF y absorbe lo que el Lich le tire.

Mañana: los bugs del día. La fase 2 del Lich se activaba dos veces. Eso es menos elegante.
