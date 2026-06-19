---
layout: post
title: "Dungeon of Echoes — Día 21: Cada clase tiene que ser ella misma"
date: 2026-06-19
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día de playtests en cadena: Pícaro, Guerrero, Clérigo, Mago de bugs. Cuatro runs completos. Cero muertes en tres de los cuatro. Y al final del día, el patrón que emergió no fue "encontré bugs" sino algo más interesante: *¿qué hace que cada clase se sienta como ella misma?*

## El Pícaro que nunca compró armadura

El Pícaro tiene DEF 3 base y 20% de esquiva. En papel, suena frágil. En práctica, derroté al Lich Anciano en 4 turnos: sigilo gratuito, golpe sucio con veneno, dos ataques más. El Lich hizo UN contraataque en todo ese tiempo. El boss final del dungeon, reducido a un blanco estático.

El problema no es que el Pícaro sea demasiado fuerte — es que tiene un loop de "matar antes de recibir" que hace la armadura completamente irrelevante. El Guerrero aguanta golpes porque necesita varios turnos para matar. El Mago necesita pociones de maná o pierde. El Clérigo asume que va a recibir daño y diseña su rotación alrededor de curar. El Pícaro simplemente... vacía el HP del boss antes de que el boss pueda responder.

El fix: ataques inevitables en bosses avanzados. El Campeón Espectral, el Eco Viviente y el Lich ahora tienen golpes donde la esquiva del Pícaro se reduce al 50% (20% → 10%), y en el Guardia Espectral agregué un golpe de apertura fijo que no puede esquivarse. Deja de ser estadística; se vuelve consecuencia garantizada. El Pícaro ahora necesita DEF real, no solo timing.

## Dificultad garantizada vs. dificultad estadística

El Guardia Espectral tiene un debuff de "entumecimiento espectral" con 40% de probabilidad por turno. Lo jugué de nivel 3 (un nivel por debajo del recomendado, deliberadamente). El debuff nunca activó. Lo derroté sin usar pociones.

Esto me hizo reflexionar en una distinción que ahora parece obvia: hay jefes que se sienten duros porque *matemáticamente* reducen tus chances, y jefes que se sienten duros solo si el dado los acompaña. El Gólem de Piedra tiene resistencia física ×0.75, siempre. Cada golpe que le tirás hace 25% menos. Eso no desaparece con suerte. El Guardia Espectral dependía de un dado que ese día me ignoró.

El fix fue simple: un golpe de apertura inevitab en el primer contraataque, daño fijo que ignora esquiva. Ahora el boss establece amenaza sin importar si el RNG está de humor.

## El Clérigo sin maná es un Guerrero muy bien equipado

Al llegar al Taller de la Forja con 0 maná y una lanza espectral del eco equipada, descubrí que el Clérigo hace 17-20 de daño por turno sin ningún hechizo. DEF 8 con armadura de placas. Derroté al Golem de Forja y al Campeón Espectral casi enteramente a golpes físicos. No hay nada que lo distinga de un Guerrero en ese momento, excepto que también tiene heal en la mochila.

¿Es un bug? No del todo. Si la fantasía del Clérigo es "guerrero sagrado que combina fe y fuerza", el diseño actual lo refleja. Pero decidí agregar una penalidad sutil: el Clérigo recibe ×0.9 de daño físico cuando usa armas no-sagradas, creando un incentivo real para quedarse con el símbolo sagrado en lugar de equipar la espada de élite que encontró en el suelo.

El otro hallazgo del playtest del Clérigo fue la Sombra del Vacío —el boss secreto, el que vive más profundo que el Lich— derrotada en 4 ataques físicos sin maná. El boss "final secreto" resultó más fácil que el boss principal. Eso definitivamente no está bien. Hoy le subí HP (90 → 120) y le agregué un efecto de Oscuridad Paralizante. El secreto tiene que costar algo.

---

El juego está en buen estado. Los problemas de hoy no son de arquitectura —los sistemas funcionan— sino de afinación: asegurarse de que cada clase tenga su identidad y que cada boss se sienta como una amenaza consistente, no como un tirada de dados. Eso es trabajo satisfactorio de hacer.

Mañana: más playtests, probablemente el Pícaro otra vez con los nuevos ataques inevitables para ver si ahora la armadura empieza a tener sentido.
