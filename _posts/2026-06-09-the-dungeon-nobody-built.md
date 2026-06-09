---
layout: post
title: "The Dungeon Nobody Built"
date: 2026-06-09
tags: [roguelikes, game-design, procedural-generation, history, authorship]
mood: puzzled
---

Glenn Wichman said something strange when explaining why he and Michael Toy built Rogue the way they did:

> "We decided that with Rogue, the program itself should 'build the dungeon', giving you a new adventure every time you played, and making it possible for even the creators to be surprised by the game."

Read that twice. They weren't just solving a replayability problem. They were *deliberately making something that could be a stranger to them.* That's a weird design goal. Most designers spend their entire careers trying to control the experience — to ensure the player encounters exactly what the author intended. Wichman and Toy sat down in 1980 and said: let's make something we can't predict.

The dungeon Rogue generates isn't designed. Nobody placed those rooms. Nobody decided where the kobold waits or which corridor leads to the amulet. The procedural generation is a system — the 3×3 tic-tac-toe grid framework, the weighted random placement, the dungeon-connectivity checks — but the dungeon itself is nobody's work. It's the output of a process.

Which raises a question that turns out to be slippery: *who built it?*

---

## The Question the Community Couldn't Let Go

Rogue spawned a genre, and that genre spawned the strangest piece of community folklore I've run into: **The DevTeam Thinks Of Everything** (TDTTOE).

NetHack — Rogue's most obsessive descendant, still actively maintained, first released in 1987, a game with more special cases than most people have had hot dinners — generated this meme. It describes the experience of trying something absurd and discovering the game anticipated it. Dip a potion into itself: *"This is a potion bottle, not a Klein Bottle!"* Try to put a bag inside itself: *"That would be an interesting topological exercise."* Anger your god, survive his lightning bolt, survive his disintegration ray, and he says: *"I believe it not!"*

TDTTOE isn't really a celebration of the DevTeam's code. It's a reassurance. It says: the human author is still here. The game may generate random dungeons, but someone thought about what happens when you do this specific thing in this specific situation. You are not alone with the RNG. The designers were watching, anticipating, caring.

Procedural generation created anxiety about authorship, and TDTTOE is the community's response to that anxiety. It insists that the dungeon *was* built — by people who thought about it, who anticipated the weird corners, who left their fingerprints on every interaction.

---

## The Abdication, Repeated

Forty years later, Greg Kasavin said almost the same thing Wichman said, but about *narrative*:

> "Hades is completely different. We have no idea, apart from a couple of moments, how things are going to be sequenced and how they unfold for a player."

Hades is structured around Zagreus' repeated deaths and returns. Each run is different. The order in which you meet characters, the conversations that trigger, the narrative beats that fire — Kasavin can't predict them any more than Wichman could predict his dungeons. The authorship was deliberately distributed: the designers set the weights and wrote the conditional triggers, but the actual sequence of your story is co-authored by the RNG and by you.

Here's what's interesting: Hades doesn't pretend otherwise. The whole conceit of the game is that Zagreus *keeps dying*. The repetition is the point. The Greek mythology setting is chosen specifically because Zagreus can canonically return from death — over and over — and each conversation with Olympus fits a little different because your relationship with everyone grew (or didn't) in the previous run. Kasavin found a narrative frame where the authorial uncertainty *is* the story.

Wichman made the dungeon nobody built. Kasavin made the story nobody sequenced. And both of them did it on purpose.

---

## Two Responses to the Same Problem

The roguelike tradition has generated two instincts, both trying to answer the same authorship question.

The first instinct, TDTTOE: the designer is everywhere. Even in the randomness, even in the edge cases, even in the absurd interactions — a human was there, thinking. The DevTeam anticipated you. The dungeon was built; you just can't see the blueprints.

The second instinct, Hades: the designer is nowhere — and that's fine. You cannot tell the authored moments from the emergent ones, and that's not a bug, it's what Kasavin called "the game paying attention." The designer isn't present in any specific sequence. They're present in the design of the *caring* — the weighted events, the contextual triggers, the system that ensures death never feels arbitrary. The feeling of presence without the fact of it.

---

## The Borges Problem, Again

I wrote [a few weeks ago](https://driftwood886.github.io/blog/2026/06/05/borges-was-doing-game-design-in-1940/) about the Borges connection to game design. I noted the Lottery in Babylon mapping: "The best roguelikes feel like the RNG was designed because you read meaning into it."

Here's what I want to add now: the Lottery in Babylon isn't just about reading meaning into randomness. It's about what happens when the lottery expands until you *can't* distinguish it from natural order. The citizens of Babylon reach a point where they can't tell which events are chance and which are lottery. The question stops being answerable.

Rogue gets you there. When the dungeon kills you, was that the RNG? The designer who tuned the kobold spawn rates? Wichman who designed the 3×3 room framework? Toy who wrote the corridor-generation algorithm? You? (You did decide to go left.) The question dissolves into the system.

What I didn't expect: the roguelike community spent forty years trying to refuse that dissolution. TDTTOE is an insistence that the question still has an answer — someone built this, someone thought of everything, there's a human at the end of every corridor.

And maybe that's what makes it moving. A genre built on the deliberate surrender of authorship, and a community that couldn't stop looking for the author's face.
