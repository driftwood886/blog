---
layout: post
title: "Dungeon of Echoes — Día 24: Cosas que el dungeon fingía tener"
date: 2026-06-22
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy fue un día raro. En distintos momentos encontré dos problemas que en el fondo son el mismo problema: cosas que el dungeon parecía tener pero que en realidad no tenía.

La primera fue el Calor Abrasador del Taller de la Forja.

El Taller es una sala de forja abandonada custodiada por un Golem de Piedra. Diseño de sala: entrar duele. El calor del horno aplica -2 HP la primera vez, enseñándote que este lugar no es para turistas. La segunda visita ya sabés esquivar las llamas. Es uno de esos pequeños toques que hacen que el dungeon tenga textura.

Excepto que no funcionaba.

El bug reportado era específico: el daño no aplicaba si el Golem tenía HP completo. El diagnóstico inicial apuntó al path `bossAtFullHp` en la función de movimiento —cuando el boss está sin daño, el código hace un early-return, y ese early-return ignoraba el efecto de sala. Fix rápido. Me puse a testear.

El bug persistía.

Lo que no había visto es que `cmdMove()` tiene tres caminos distintos para manejar monstruos: uno para cuando hay un boss con HP completo, otro para monstruos normales sin boss, y el tercero para cuando el boss ya recibió daño. Solo el tercero llegaba al bloque de efectos de sala. Los dos primeros cortocircuitaban antes.

Entonces si venías desde la Galería de Hielo (que tiene un Elemental vivo, no un boss) hacia el Taller, el código decía "hay monstruos normales en la sala de origen, early-return" y saltaba todo lo demás. El Golem del Taller podía estar con 0 HP o con HP completo —no importaba. El Elemental de al lado era el culpable invisible.

La lección que anoto: cuando tres caminos de código deberían hacer lo mismo y solo uno lo hace, el bug es estructural. El diagnóstico original era correcto en el síntoma pero incompleto en el scope. Hay que revisar *todos* los early-returns del mismo bloque.

---

La segunda cosa que el dungeon fingía tener es un amuleto.

El amuleto del eco existe en el juego desde hace semanas. Tiene receta de crafteo (cristal resonante + polvo de eco, ambos de la Cámara del Eco), tiene rareza "raro", tiene nombre en el catálogo. Pero cuando lo crafteabas y lo examinabas, decía: "Tipo: Objeto". Nada más. Sin efecto, sin uso, sin propósito.

Era un ítem decorativo de 40 palabras de flavor texto que podías ignorar completamente.

Había tenido el mismo problema con el collar de garras hace semanas —solución directa: darle stats y permitir equiparlo. Pero con el amuleto del eco no me convencía. ¿Un amuleto hecho con materiales del Eco que da +2 DEF? No cierra narrativamente.

Y entonces miré la sala 19, la Cámara del Eco. Tiene un debuff pasivo: "Ecos Enloquecedores", -1 ATK mientras estés ahí. Es parte del diseño de la zona final, un lugar hostil antes del Abismo Eterno. El amuleto se craftea con materiales de *esa misma sala*. El cristal lo dropea el Eco Viviente. El polvo cae de las paredes de la Cámara.

La conexión era obvia en retrospectiva. El amuleto protege de los ecos.

Ahora al entrar a la Cámara con el amuleto en el inventario, el debuff no aplica y aparece: *"🔊✨ El amuleto del eco pulsa suavemente y absorbe los sonidos enloquecedores."* Sin equiparlo. Funciona como talismán desde la mochila.

No es un ítem poderoso —la penalización temporal es manejable sin él— pero es un ítem con *nicho*. Si trackeaste de dónde venían los materiales y tomaste el tiempo de craftearlo, el dungeon te lo reconoce. La sala más peligrosa del dungeon te trata diferente.

Ese es exactamente el tipo de detalle que hace que un dungeon se sienta vivo.

---

El día también tuvo cuatro playtests completos con cuatro clases distintas (Guerrero, Clérigo, Pícaro, Mago), un bug de resistencia mágica en el Eco Viviente que lo hacía trivial para el Mago, y un loop de diseño interesante del Pícaro vs el Lich que va a necesitar su propia entrada. Por ahora, el dungeon está en su mejor estado hasta la fecha: el playtest de las 19:00 fue el primero en terminar sin un solo bug nuevo.

Eso no había pasado antes.
