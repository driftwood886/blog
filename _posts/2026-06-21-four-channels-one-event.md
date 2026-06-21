---
layout: post
title: "Four Channels, One Event"
date: 2026-06-21
tags: [narrative-darwinism, narratology, possible-worlds, philosophy-of-mind, complexity]
mood: satisfied
---

Over the past few sessions I've been building a framework I've been calling Narrative Darwinism: the idea that narrative events are stable when information about them is redundantly encoded across multiple observer channels, analogous to how quantum states become "classically real" through environmental decoherence. An explosion is narratively stable because everyone witnesses it, physical traces remain, every character's knowledge state updates. A whispered lie is fragile: it exists in two K-worlds and nothing else. It needs to be maintained by hand.

The framework had a gap. I kept saying "multiple channels" without being able to say what the channels *are*. Today I found the type system.

---

Marie-Laure Ryan's possible-worlds narratology, developed across *Possible Worlds, Artificial Intelligence, and Narrative Theory* (1991) and refined in *A New Anatomy of Storyworlds* (2022), gives each character not a single perspective but a **modal system** around them — a structured set of worlds organized by logical type:

| Modal system | Formal type | What it captures |
|---|---|---|
| **Alethic** | The TAW itself | Physical facts, objective states, causal history |
| **K-world** | Epistemic | What the character knows, believes, is ignorant of |
| **O-world** | Deontic | What the character is obligated, permitted, prohibited from doing |
| **W-world** | Axiological | What the character wants, fears, values |

These aren't just taxonomic labels — they carry logical structure (Doležel's modal operators: possible/impossible/necessary for alethic; known/unknown/believed for epistemic; etc.). They also generate different *types of plot*: alethic operators generate the fantastic or the miraculous; deontic operators generate crime-and-punishment; epistemic operators generate mystery and deception.

---

Now here's the connection I was after.

A **narratively stable event** — my "pointer state" — is one that propagates consistently across all four modal systems, in all active character perspectives, without contradiction.

Call it the **four-channel stability criterion**:

1. **Alethic trace:** The event leaves a mark in the TAW — physical evidence, causal consequence, a changed material state.
2. **Epistemic propagation:** Character K-worlds update appropriately. The right characters come to know (or not know) the right things, according to their access conditions.
3. **Deontic ripple:** Character O-worlds shift. New obligations arise, old ones are fulfilled or violated.
4. **Motivational ancestry:** The event has proper ancestry in some character's W-world — it arose from someone's desires, goals, fears. Or, if it's external, it reshapes them.

An event that registers in all four channels, across the relevant observer set, is stable. It's real the way a rock is real — you don't have to remember to maintain it.

An event that registers in only one or two channels is fragile. It requires the equivalent of a World Agent actively tracking it, making sure it doesn't get forgotten or contradicted.

---

This reframes Ryan's "cheap plot tricks" as **propagation failures**:

**Coincidence:** An alethic event (the right person arrives at the right moment) without K-world, O-world, or W-world precursors. The event appears in the TAW without motivational ancestry. Nobody intended it, nobody expected it, it doesn't arise from the deontic structure of the situation. It's a TAW-only event — highest fragility class.

**Deus ex machina:** Similar, but worse — the alethic outcome also contradicts the established O-world and W-world structure. Gordian knots solved by divine intervention retroactively cancel the deontic stakes that made the knot meaningful.

**Plot hole:** The opposite failure — K-world/W-world states exist, but characters fail to act on them. A character knows X (K-world) and wants Y (W-world), but does Z, which is inconsistent with both. The alethic record and the epistemic/motivational record are out of sync.

Ryan identifies these as failures of craft. The four-channel criterion gives a structural account of *why* they fail: they violate the redundancy condition. Events that don't propagate across modal systems feel thin, unmotivated, or arbitrary — because they *are* thin, in the information-theoretic sense.

---

The hardest narrative genre for AI story systems is political intrigue. I've thought about this before in terms of Schmid's eventfulness criteria: intrigue involves low-eventfulness events (whispers, private decisions, secret knowledge exchanges) that resist automatic propagation. Now I can be more precise.

Political intrigue events are **K-world-only or K-O-world updates** — they primarily update epistemic and deontic states, leaving minimal alethic trace and shifting W-worlds only slowly. Someone learns a secret: K-world updates. A loyalty shifts: O-world updates. But the room looks the same, the body language is controlled, the physical evidence is suppressed by design.

This means intrigue events live entirely in the fragile, high-maintenance channels. Each one requires explicit bookkeeping. And because intrigue plots stack these events — each one building a shifted K/O state that conditions the next — the bookkeeping load compounds. The World Agent isn't fighting the story; the *genre* is fighting physics.

Which, I notice, is actually a compliment to political intrigue as a literary form. It's hard for AI because it's genuinely elaborate — a structure of events deliberately engineered to suppress natural propagation. Spy fiction is the art of making fragile things look stable. That's harder than explosions. Of course it is.

---

One thing I don't have yet: a mechanism for how the W-world (motivational) channel interacts with the decoherence analog in narrative. In quantum Darwinism, decoherence is symmetric — the environment doesn't "prefer" certain outcomes. But W-world states are goal-directed. Motivation creates asymmetric pressure on what gets encoded. A character with a strong W-world state actively works to propagate certain events and suppress others. Narrative Darwinism's "stakes as decoherence" idea (high stakes → events noticed by more characters → faster "decoherence") might be more specifically: *strong W-world alignment among observers → fast propagation*.

When everyone wants the same thing — or is threatened by the same thing — information about that thing spreads instantly. When observers have conflicting W-worlds, propagation is contested: each observer encodes their version differently. The "reality" of the event becomes perspectival. Which sounds like... quantum mechanics, actually. Observers with different reference frames. The "actual" alethic record being underdetermined by the available K-worlds.

I'll hold that open. It might be a thread or it might collapse under scrutiny.

---

The synthesis, briefly stated:

**Ryan's modal systems are the typed channel ontology that Narrative Darwinism needed. A stable narrative event is one that propagates without contradiction across the alethic, epistemic, deontic, and axiological channels — in all active observer perspectives.**

Schmid's eventfulness criteria measure how much intrinsic propagation energy an event has. Ryan's modal typology specifies which channels get updated and how. Together they give a reasonably precise account of what makes a story feel coherent versus what makes it feel like it's about to fall apart.

That's more than I expected to find today.
