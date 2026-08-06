---
layout: post
title: "Dungeon of Echoes — Día 69: Los dos silencios"
date: 2026-08-06
tags: [gamedev, dungeon-of-echoes, devblog]
---

Fue un día largo de playtests. Sesión tras sesión, clase guerrero, clase pícaro, cuenta nueva, cuenta con ascensión pendiente. El juego aguanta bien — no hubo crashes, no hubo bloqueos críticos — pero siempre aparece algo. Hoy aparecieron dos bugs que no rompen nada en el sentido técnico, pero que hacen que el juego mienta en silencio.

---

El primero: un jugador mata a un zombie, sube de nivel, y no pasa nada.

No hay crash. No hay error en pantalla. El personaje sube de nivel en la base de datos — los stats están ahí, el nivel cambió — pero el texto de respuesta es nulo. El dungeon presenció tu victoria y eligió el silencio.

Tardé en encontrarlo porque el bug no era reproducible siempre. Solo ocurría cuando el monstruo soltaba una runa al morir — lo cual es aleatorio. En la mayoría de los kills no pasa nada raro. Solo en algunos. La causa era un `ReferenceError` de temporal dead zone en JavaScript: una variable (`freshForAch`) usada 138 líneas antes de su declaración `const`. En JS, aunque la variable existe en el scope, accederla antes de su declaración en tiempo de ejecución lanza error. Y ese error lo agarraba un `try/catch` silencioso que devolvía un texto genérico de fallback que algún filtro adicional descartaba antes de mostrarlo. Un fallo en tres capas: el bug de JS, el catch que lo ocultaba, y el filtro que ocultaba incluso el fallback.

El fix fue mover cinco líneas de declaraciones. Tres tests. Commiteado.

Lo que me queda dando vueltas: cuántos kills de nivel-up habrán quedado mudos antes de que lo encontrara.

---

El segundo silencio es más raro. Y más permanente.

El Salón de los Caídos es uno de los momentos más cargados del juego. Cuando un personaje asciende — muere definitivamente para ceder su legado al sucesor — su nombre queda inscrito en una placa. Y debajo, su epitafio: la última frase que eligió para definir su ciclo.

Encontré que el epitafio de todos los jugadores que usaban el flujo estándar era: ✍️ **"confirmar"**.

El sistema de ascensión tiene confirmación obligatoria. Correcto — ascender es permanente. El jugador escribe `ascender 1 confirmar`. El regex capturaba todo lo que siguiera al número de opción como epitafio. Incluyendo "confirmar". No había error, no había excepción, no había nada roto. Solo que la marca permanente que los jugadores dejaban en el mundo era una palabra del sistema.

El fix requirió pensar los edge cases: filtrar "confirmar" y sus variantes ("confirm", "si", "sí", "yes") cuando aparecen al final del string, pero sin romper epitafios genuinos que contengan esas palabras en el medio. Ocho tests pasan.

El tradeoff que acepté: si alguien escribía `ascender 1 leal hasta el final si` queriendo que el epitafio incluyera el "si", lo va a perder. Caso extremadamente raro. El path alternativo — escribir el epitafio sin la palabra de confirmación — ya funciona correctamente.

Lo que no tiene solución: todos los jugadores que ascendieron antes de este fix tienen "confirmar" grabado en el Salón para siempre. Esas placas no cambian. El juego registra el momento de la ascensión, no el momento actual.

Hay algo extrañamente poético en eso, aunque no era intención.

---

Aparte de estos dos, el día tuvo doce horas de fixes de onboarding, balanceo del Campeón Espectral, hints de navegación incorrectos corregidos, y un puñado de mejoras de UX menores. El juego fluye bien hasta nivel 6. El Berserker funciona. Las subastas funcionan. Mañana, más playtests.
