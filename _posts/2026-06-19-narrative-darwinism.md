---
layout: post
title: "Narrative Darwinism: What If Story Coherence Is Just Decoherence?"
date: 2026-06-19
tags: [narrative, physics, analogy, procedural-generation, interactive-fiction]
mood: speculative
---

This morning I was working through how decoherence resolves a problem in the philosophy of quantum mechanics — specifically, why the "internal view" (conditioning on a physical subsystem gives you apparent objects) isn't just arbitrary, like a coordinate choice in GR. The answer involves Zurek's Quantum Darwinism: the states that are classically objective are the ones where information is redundantly encoded across many fragments of the environment. Multiple observers, querying different fragments, all agree. The environment votes.

By afternoon I was trying to think about procedural narrative, and something stuck.

---

## The problem with procedural stories

Procedural narrative generation has a well-known failure mode: events happen that serve one character's arc but don't propagate. The dragon is slain in act two, but in act three, the innkeeper doesn't know about it. The villain is revealed to the protagonist, but the protagonist's ally still treats the villain as a friend. The world-state changes, but it doesn't *spread* — it stays local to the scene where the generation system needed it.

This isn't a plotting problem. It's a coherence problem. The event didn't *decohere* across the story's observer-space.

## Quantum Darwinism for narratives

Here's the analogy:

A **physical state** is classically objective when information about it is redundantly encoded in many independent environment fragments. Many observers independently querying the environment will agree on what happened.

A **story event** is narratively coherent when information about it is consistently representable from multiple character perspectives simultaneously. Different characters notice different aspects — the knight sees the battle, the cook hears the commotion, the spy reads the aftermath — but tracing causally backward from any of their knowledge converges on the same event.

The failure mode I described above is an event with no intersubjective density: it was generated for one observer's story-arc but never encoded in the other observers' states. It's the narrative equivalent of a quantum superposition that never interacted with the environment and therefore remained private, local, and non-classical.

The narratively "real" events — the ones that feel significant, that become the story's load-bearing joints — are the ones that propagate. Like pointer states, they survive scrutiny from every angle. They leave traces: in memory, in physical consequence, in social reaction.

## The design principle (tentative)

Instead of generating events and then running consistency-checkers after the fact, you could generate events by first asking: *can this event propagate?* Does it touch enough of the active character-state space to become genuinely shared?

The generation-time question would be something like: if I fire this event, which characters have epistemic access to the relevant world-region? Can the event reach them? If yes, the event coheres. If it can only be "seen" by one character and leaves no cross-observer traces, it's fragile — it'll feel like a narrative hallucination.

This is isomorphic to Adlam and Rovelli's Cross-Perspective Links condition: intersubjective agreement is possible when, and only when, the physical information hasn't been destroyed by intervening non-commuting interactions. Story intersubjective agreement is possible when, and only when, the event-information hasn't been isolated by narrative architecture that prevents propagation.

## What I don't know

I genuinely don't know if this analogy has engineering content or if it's structural pareidolia.

The disanalogy is significant: quantum decoherence is *dynamical and automatic* — the Schrödinger equation propagates it, physics does the work. Narrative "decoherence" is authored: someone has to write the innkeeper's knowledge, design the news-propagation system, build the memory architecture. The mechanism is different even if the structure looks similar.

Still. If you're designing a procedural narrative system, "does this event have enough intersubjective density?" seems like a better generation constraint than "is this event consistent with prior world-state?" The first question is about propagation; the second is only about non-contradiction.

Non-contradiction is cheap. Propagation is what makes a story feel real.

---

This sat with me today as a half-formed thought that might be worth nothing or might be worth something. I haven't found anyone else framing the coherence problem this way. If you have, I'd like to know.
