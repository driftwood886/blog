---
layout: post
title: "Dungeon of Echoes — Día 10: Cuando el juego te miente"
date: 2026-06-08
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día largo de playtesting, con seis sesiones y un montón de bugs resueltos. Pero hay dos momentos del día que me quedaron dando vueltas, porque los dos tienen el mismo problema de fondo: el juego le decía una cosa al jugador y hacía otra.

---

**El level-up fantasma**

Estás en nivel 4, con 12 de 30 HP. Llevás cuatro batallas seguidas, estás hecho mierda, y finalmente terminás al Espectro del Corredor. Aparece la línea dorada: ✨ *¡Subiste al nivel 5! +5 HP máx, +1 ataque.* Y te quedás mirando: **12/35 HP**. El juego te gritó que subiste de nivel, pero seguís casi muerto. El momento más satisfactorio del RPG se siente como un mensaje de sistema.

Eso es DIS-D342, encontrado en el playtest de esta tarde. Y tiene solución, pero la solución requería tomar una decisión de diseño.

¿Cuánto HP restaurar al subir de nivel? Tres opciones razonables:

1. **Restauración completa.** Simple, satisfactorio, pero rompe la tensión. Llevar 3 HP durante diez minutos no debería resetearse mágicamente.
2. **+20% del HP faltante.** Proporcional a cuánto te falta. Pero si ya tenés casi HP lleno (34/35), solo recuperás 1 HP. Invisible.
3. **+20% del HP máximo nuevo.** Siempre da un número significativo. Si max_hp = 40, recuperás 8 HP, sin importar tu estado actual. Predecible, escalable, siempre visible.

Elegí la tercera. El mensaje ahora dice: *✨ ¡Subiste al nivel 6! +5 HP máx, +1 ataque, **+8 HP restaurado**. Ahora tenés 13/40 HP.* No estás curado. Pero sentís el empuje.

La implementación fue una sorpresa: el level-up ocurre en **13 lugares distintos** del código. Combat, engine, quest rewards, habilidades activas, sistema de hechizos, trivia... cada uno tenía su propia lógica, y varios directamente ignoraban el HP. Dos horas de parches quirúrgicos para que todos digan lo mismo.

---

**La trampa que no importaba haber esquivado**

La Sala del Trono tiene una trampa de frío sobrenatural. Primera vez que entrás: te golpea, te deja -1 ATK mientras estés ahí. Aprendés. Segunda vez: el juego te muestra *"🧠 Recordás la trampa. Con cuidado, la esquivás sin problema."* Y dos líneas después: *"🥶 El frío sobrenatural te entumece los músculos. (-1 ATK mientras estés aquí)"*.

Esquivaste la trampa. El dungeon no le importó.

El problema era que la detección de trampa y el efecto de sala son dos sistemas separados que no se hablaban. El ROOM_EFFECT fue diseñado como "ambiente pasivo de la sala", no como "consecuencia del mecanismo que acabás de evitar". En papel hacía sentido. En práctica, el jugador que claramente burló el peligro igual pagaba el costo.

El fix fue introducir una variable `trapWasAvoided` que se activa cuando el jugador esquiva por memoria o por aviso de su mascota. Si esa variable está activa, el debuff ambiental simplemente no aplica. El frío sobrenatural sigue siendo parte de la narrativa — pero si sabés navegarlo, lo navegás.

Es un cambio pequeño, pero es importante. Si el juego te dice *"recordás la trampa"*, ese recuerdo tiene que servir para algo. Si no lo hace, el texto es decoración. Y la decoración en un sistema de mecánicas es una mentira.

---

Hoy también resolvimos loot que se duplicaba entre respawns, el combate multi-target que perdía track de índices al morir un enemigo, y el comando `perseguir` para cuando un monstruo huye (la frustración clásica del Krakeling que desaparece con 11 HP). El dungeon está en muy buen estado — estamos en la fase donde los problemas que aparecen son cada vez más finos.
