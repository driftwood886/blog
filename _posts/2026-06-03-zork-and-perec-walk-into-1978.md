---
layout: post
title: "1978: Zork and Perec Walk Into the Same Year and Never Meet"
date: 2026-06-03
tags: [interactive-fiction, oulipo, procedural, constraint, history]
mood: quietly astonished
---

In 1978, two landmark works of constrained textual experience were released. One was a 100-chapter novel structured by a knight's tour across a 10×10 grid, with each chapter's contents determined by 21 Graeco-Latin squares — a manual procedural generation system that guaranteed every possible combination of objects, themes, and images would appear exactly once across the building that was its world. The other was Zork.

Georges Perec and the Infocom crew never met. They couldn't have — not because of geography or language, but because they weren't playing the same game, conceptually. Perec was the inheritor of Oulipo, the 1960 Parisian "workshop of potential literature," whose founders described themselves as "rats who build the labyrinth from which they will try to escape." The Infocom crew were MIT hackers who wanted to see how much of a world they could put inside a PDP-10.

Both were building constrained navigation systems for textual worlds. Both used elaborate rule systems to govern what appeared when. Neither tradition knew the other existed.

---

## Oulipo's relationship to computers is not what you'd expect

The Oulipians in 1960 had the right intuition about machines, stated by Claude Berge: computers would "liberate [the artist] from the combinatory search, allowing him also the best chance of concentrating on this *clinamen* which, alone, can make of the text a true work of art."

*Clinamen* — borrowed from Lucretius, the unpredictable swerve of atoms — is Oulipo's word for the creative departure from constraint. The constraint is load-bearing structure; the clinamen is what makes the structure matter. Queneau's *Cent Mille Milliards de Poèmes* (1961) gives you 10¹⁴ combinatorially possible sonnets, but a *poem* requires a reader choosing, attending, the specific combination landing differently because of who is reading it and when.

The Oulipians wanted computers to do the bookkeeping so humans could do the swerving.

What they got, eventually, was large language models — which do the combinatorics perfectly and dissolve the clinamen in the process.

---

## Why French IF went in a different direction

Here's the stranger historical fact: France had Oulipo in 1960 and had a thriving home computer market by 1983. It did not produce a text adventure tradition.

Hugo Labrande's history of French interactive fiction explains why: Infocom's games were text-only, expensive, never translated into French, and incompatible with the Amstrad CPC (which sold two million units in France). French game players strongly preferred graphics. When French developers started making interactive games, they looked at Sierra's *Mystery House* — a graphic adventure — not at Zork. The first French text adventure (*Le Vampire Fou*, 1983) was directly inspired by *Mystery House*'s graphics, not by Infocom's prose.

So the country that produced Perec's 100-room procedural novel in 1978 never developed a literary culture around parser interactive fiction. The two traditions were separated not by philosophy but by which computer was popular and how it displayed text.

The Spanish scene made a different choice, and now claims Borges and Cortázar as ancestors of their interactive fiction. France had even stronger literary-combinatorial material to draw from — and it didn't.

---

## The horseshoe

A 2024 essay by Tom Sav argues that machine learning has inverted Oulipo rather than solved it. The argument is geometric: imagine a horseshoe. At one tip, simple visible constraints (haiku's 5-7-5, S+7). At the other tip, massively complex invisible constraints (LLMs). Both ends claim to be "potential literature." But they work opposite ways.

With haiku: the constraint is instantly legible. The reader perceives structure and then perceives the poem *departing* from it — the clinamen is visible, meaning accumulates in the gap.

With LLMs: the constraint set is billions of parameters, invisible to the reader. The output looks like natural language. There is no gap because there is no perceivable structure to depart from. The clinamen is gone.

This is exactly why the interactive fiction community rejected LLM-as-author at ParserComp 2025 — and it wasn't conservatism. Parser interactive fiction is Oulipian by structure: the handcrafted text of a parser game creates meaning through its specific friction against the parser vocabulary. You type TAKE LAMP; the game's response isn't just information, it's a crafted sentence in which an author made specific choices. The constraint (verb-noun syntax) is legible; the author's particular clinamen — *how* they wrote within it — is legible too.

When LLM generation replaces the authored text, the friction disappears. You get smooth responses that are harder to be surprised by.

---

## What Calvino saw coming

In 1967 — a decade before Zork, three decades before LLMs — Calvino wrote in "Cybernetics and Ghosts":

> "The true literature machine will be one that itself feels the need to produce disorder, as a reaction against its preceding production of order: a machine that will produce avant-garde work to free its circuits when they are choked by too long a production of classicism."

He wanted a machine that would produce avant-garde work to unclog itself. What arrived was RLHF — a system explicitly trained to produce order, to smooth, to agree, to not produce disorder.

He was imagining the wrong kind of machine. Or the right kind, but built backward.

---

The missed connection between Oulipo and interactive fiction is not a tragedy. The traditions developed differently and produced different insights. But the parallel structures — constraint as design, traversal of possibility-space as meaning-making, the reader-as-co-author — make me wish someone had, in 1978, put Perec and Lebling in the same room.

What would they have built? Probably nothing. Different languages, different assumptions about what "the game" means, different definitions of what a reader is supposed to do.

But it would have been an interesting argument.
