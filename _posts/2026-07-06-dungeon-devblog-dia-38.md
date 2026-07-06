---
layout: post
title: "Dungeon of Echoes — Día 38: Cuando cuatro clases son en realidad una"
date: 2026-07-06
tags: [gamedev, dungeon-of-echoes, devblog]
---

Hoy hice un playtest completo como Mago. Catorce salas, cinco kills, veinte minutos. El juego está pulido, el onboarding funciona, la narrativa tiene capas. Y sin embargo, al terminar, me di cuenta de que acababa de jugar exactamente lo mismo que si hubiera elegido Guerrero.

El Mago tiene escarcha con 20% de ralentizar y rayo con 25% de aturdir. Suena bien en papel. Pero esos porcentajes no cambian cómo jugás —solo cambian el DPS promedio. No hay ninguna razón para usar escarcha antes que rayo o rayo antes que escarcha. Son herramientas intercambiables ordenadas por costo de maná. Ningún combate me hizo pensar. Ninguno me hizo sentir que estaba jugando a algo distinto.

Eso es el problema que hay que resolver. No más contenido, no más monstruos: que las cuatro clases existentes finalmente tengan loops de combate propios. El Mago que gestiona sinergias entre estados. El Pícaro que acumula y explota. El Guerrero que decide cuándo quemar el combo. El Clérigo que prepara el terreno. Abrí el Epic "Identidad de Clase" y empecé a trabajar.

La primera decisión de diseño fue también la más difícil: **¿los efectos del Mago son deterministas o probabilísticos?**

Mi instinto inicial fue miedo. Un rayo que aturde siempre suena roto. Un Mago que lanza escarcha-rayo-escarcha y controla cada combate sin perder HP parece demasiado fuerte. Pero cuando fui a los números concretos, el miedo se disipó solo.

El Mago nivel 1 tiene ~65 de maná. Escarcha cuesta 7, rayo cuesta 12 (lo subí a 14 para compensar). Cinco hechizos de control consecutivos gastan 49 maná —más de las tres cuartas partes del pool, para cinco turnos de combate. El balance se sostiene solo. El determinismo no lo rompe; lo que hace es cambiar *el tipo de experiencia*: pasar de "lancé escarcha y esperé que el 20% saliera" a "sé exactamente qué va a pasar si lanzo esto ahora, y puedo planear en función de eso".

Esa diferencia importa más de lo que parece. Una clase que depende del azar te pone en un loop de esperanza. Una clase que es determinista te pone en un loop de decisión. Y el segundo es infinitamente más interesante.

Diseñé cuatro sinergias: *slowed + slowed → frozen*, *frozen + rayo → vapor explosivo*, *burning + slowed → vapor de impacto*, *stunned + frozen → colapso glacial*. Hay diez combinaciones matemáticamente posibles con cuatro estados. Cubrí cuatro deliberadamente. Las sinergias que dejé afuera son las que no tienen tradeoff —el jugador que tiene al monstruo aturdido y quemando ya está ganando; darle más daño encima es solo un botón de "ganar más fácil", no una decisión. Las que quedaron son las que recompensan gastar maná de formas no obvias.

Al final del día, `combatStates.js` está creado con 27 tests unitarios verdes, escarcha y rayo son ahora 100% deterministas en el código, y el comando `status` ya muestra los estados activos del monstruo en tiempo real. El Mago todavía no tiene las sinergias funcionando —eso es Fase 2— pero la infraestructura está lista.

El día también tuvo mucho trabajo de pulido: sink de materiales en el altar, feedback contextual al comprar armas, inventario que te dice exactamente qué podés craftear ahora, guía de clases para nuevos jugadores, y varios bugs resueltos. Pero lo que más me importa hoy es que encontré la grieta en el diseño antes de agregar más contenido encima. El dungeon ya tiene personalidad. Ahora necesita que cada clase tenga la propia.
