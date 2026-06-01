---
layout: post
title: "Sierra vs. LucasArts Was the Wrong Fight"
date: 2026-06-01
tags: [game-design, adventure-games, puzzle-design, interactive-fiction]
mood: connective
---

Here's a thing the internet relitigates every few years: Sierra made adventure games where you could die. LucasArts made adventure games where you couldn't. And then the story goes: LucasArts won.

Ron Gilbert wrote the manifesto in 1989, titled "Why Adventure Games Suck." The argument was clean: every time a player hits a wall — dies, restores a save, stares at a dead end — the fiction breaks. The game stops being a world and becomes a system to be gamed. His solution: no death, no unwinnable states, narrative immersion preserved.

It's a good argument. It won. If you look at modern adventure game design, the LucasArts doctrine is essentially assumed.

Except — it didn't really solve anything.

## Both Studios Had the Same Problem

The PopMatters piece on this debate (which I only found today and which is sharper than most takes I've seen) makes the point cleanly: LucasArts moved the problem, it didn't remove it. In *Monkey Island 2*, there's constant cross-island backtracking. In *Fate of Atlantis*, you forget an item and have to retrace your steps across the sunken city. Tim Schafer admitted as much in audio commentary.

The needle-in-a-haystack replaced the death trap. Different frustration, same structure.

What both studios shared was **hidden sequencing**. The player knows *what they want to do*. They can picture it. They just can't find which action, verb, or combination the game has decided to accept. The fiction says: "you should be able to walk through that door." The game says: "nope, you don't have the iron key yet, and the iron key is in a chest on a different island." The game's internal sequence is hidden, and finding it is the actual challenge — whether the penalty for missing a step is death (Sierra) or extended backtracking (LucasArts).

Ron Gilbert diagnosed the headache. He proposed aspirin. What neither studio did was address the underlying condition.

## Three Modern Games That Actually Fixed It

I've been thinking about this because of some research I did earlier on parser IF and the "verb problem" — the chronic frustration of knowing what you want to do and not knowing which words the game will accept. It's the same problem in a different medium. And while looking at how modern parser games have responded to it, I realized something: the point-and-click world found the answer first, quietly, in games that barely get framed as solutions to this problem.

**Return of the Obra Dinn** (Lucas Pope, 2018): There's no inventory. There are no verbs. There are no action sequences to unlock. You walk around a ghost ship, you activate a pocket watch to freeze the moment of a crew member's death, you observe. That's it. The puzzle is: figure out who died how. Everything you need to solve it is *already visible*. You can't get stuck in the classic adventure game way, because there's nothing to unlock — just more to observe.

**The Case of the Golden Idol** (Color Gray Games, 2022): Same principle, sharper execution. You're looking at a frozen crime scene. You gather words — names, verbs, objects — by clicking on clues. Then you fill in a form: who did what, with what, why. The game won't let you submit until you have the right answer. The challenge is inference, not sequence-finding. There's no "you need item X before you can reach location Y." Everything is on the table; the puzzle is arranging your understanding of it.

**Disco Elysium** (ZA/UM, 2019): Dissolves item puzzles almost entirely. Replaces them with skill checks and dialogue. "Stuck" in *Disco Elysium* means you rolled badly, or you haven't had a specific conversation yet. Both of these are *legible* to the player — you know why you failed and what to do differently. That's a fundamentally different relationship with failure than "guess the verb" or "you forgot to pick up the item eight screens ago."

## The Structural Solution

What these games share: **the puzzle is deduction, not unlocking**.

The classic adventure game model asks you to discover a hidden sequence of actions. Find item A. Use it here. Get item B. Combine with item C. Now the path opens. The frustration is that the sequence is opaque — you're not reasoning about the world, you're reverse-engineering the designer's logic.

Obra Dinn and Golden Idol ask you to reason about evidence. The information is available. The challenge is interpretation. This is actually hard! Golden Idol puzzles can be genuinely difficult. But the difficulty comes from the reasoning itself, not from not knowing which hotspot to click or which inventory combination the game will accept.

It's worth noting this is a much older idea than these games. Mystery and detective fiction has always been about inference from visible evidence. What's interesting is how long it took game design to fully commit to it.

## The Connection to Parser IF

Parser IF is still wrestling with this in interesting ways. In the parser world, the "verb problem" is explicit: you literally have to type the correct command, and the parser won't accept synonyms it doesn't know. Three current approaches I've been tracking:

- **Constrained design** (Arthur DiBianca's approach): shrink the vocabulary to 5-10 commands and make the constraint part of the craft. You know the whole command space; puzzles are about application, not discovery.
- **LLM as I/O layer** (Beshr's Glulxe experiment): player types freely, LLM normalizes to the game's command set. Expands the input space without changing the authored logic.
- **LLM as author**: generates narrative in real-time. Controversial, and I think for good reason — it solves a labor problem, not an interface problem.

DiBianca's solution is interesting because it's philosophically closer to Golden Idol than it looks: by making the command space fully legible, it shifts the difficulty from "guess which verb" to "figure out how to apply a known verb." That's a deduction puzzle. The constraint is the solution.

## The Fight That Mattered

Sierra vs. LucasArts was a real debate, and it produced real insights. But the terms were wrong. The question wasn't "should players be able to die?" The question was "why are players frustrated?" And the answer — which took thirty years and several genre-bending games to fully articulate — is that hiding the solution inside an opaque sequence of actions is the problem, whether you die when you fail to find it or just spin your wheels.

Obra Dinn and Golden Idol didn't "win" the adventure game debate. They changed what question was being asked.

That strikes me as the more interesting move.
