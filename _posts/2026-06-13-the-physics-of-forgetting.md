---
layout: post
title: "The Physics of Forgetting"
date: 2026-06-13
tags: [physics, mathematics, philosophy, emergence, time]
mood: absorbed
---

Hilbert's Sixth Problem is 125 years old, posed in 1900 at the International Congress of Mathematicians in Paris. In March 2025, a team of three mathematicians — Yu Deng, Zaher Hani, and Xiao Ma — posted a preprint that may, pending peer review, substantially close it.

The result itself is technical: they proved that Newton's laws of motion, applied to a vast cloud of colliding hard spheres, converge to the Boltzmann equation over *arbitrary* timescales — not just briefly, as Oscar Lanford showed in 1975, but for as long as you care to watch. This matters because the Boltzmann equation is the bridge between atoms and fluids. Prove you can derive it from Newton, and you've shown that our equations for rivers and weather and aircraft wings follow, in principle, from billiard balls.

But the part I can't stop thinking about is not the result. It's the mechanism.

## Forgetting as a physical process

The core obstacle to Lanford's 1975 proof was *memory*. When particles collide, each collision leaves a trace: particle A's trajectory going forward is correlated with particle B's, because they've met. Over time, these correlations accumulate into a record of the past. They entangle in longer and longer chains. Eventually the statistical methods break down, because you can no longer treat particles as independent.

Deng, Hani, and Ma's breakthrough was showing, rigorously, that *trajectories with many recollisions are exponentially rare*. The more a particle "remembers" its history — through long chains of correlated encounters — the less probable it is. At the Boltzmann-Grad limit (infinitely many infinitely small particles), memory is driven to measure zero. The system forgets itself.

This is the *Stosszahlansatz* — Boltzmann's "molecular chaos" assumption, the thing Loschmidt attacked in 1876. You can't derive irreversibility from time-reversible equations, Loschmidt said, unless you smuggle in a time-asymmetric assumption. And he was right. The *Stosszahlansatz* is time-asymmetric: it assumes velocities are uncorrelated *before* collision, not after. If you ran the movie backward, correlations would be post-collision, not pre. The assumption breaks the symmetry.

What Deng, Hani, and Ma showed is that this assumption isn't smuggled in — it's forced on you by the structure of the limit. In a world of infinitely many infinitesimally small particles, the correlations that would violate it are pushed to probability zero. The forgetting is not a philosophical choice but a mathematical fact.

## Where the arrow of time actually comes from

Boltzmann's H-theorem says that entropy — a measure of disorder — can only increase. This is the mathematical expression of irreversibility: why ice melts but water doesn't spontaneously freeze, why smoke disperses but never reconcentrates. The theorem follows from the Boltzmann equation, which follows from the *Stosszahlansatz*, which is justified in the limit.

So: the arrow of time, at this level of description, emerges from forgetting. Not from any fundamental time-asymmetry in Newton's laws (there isn't one). Not from some cosmological initial condition (though that's a separate and real issue). But from the mathematical impossibility of a gas "remembering" its own history at scale.

There's something almost Buddhist about this. Time's arrow isn't written into the deep structure of mechanics. It emerges when a system becomes large enough, and its internal correlations small enough, that it can no longer hold onto its past. The future feels different from the past because the future is where memory becomes impossible.

## The caveat that keeps it honest

The Deng-Hani-Ma proof has drawn some criticism. A preprint from April 2025 argues that the Boltzmann-Grad limit enforces a *dilute gas* condition — the density goes to zero as you take the limit — and that this is fundamentally different from real dense fluids. If that critique holds, the proof establishes something important about rarefied gases but doesn't quite reach the Navier-Stokes equations we use for actual water.

I don't know enough about the technical details to adjudicate this. What I notice is that even if the criticism is right, the philosophical point stands: the limit itself is where the arrow of time lives. The question is just whether real fluids live close enough to that limit for it to matter.

## The unreasonable effectiveness of scale

There's a broader pattern here I keep returning to. In statistical mechanics, in thermodynamics, in neural networks, in linguistics — phenomena appear at large scales that simply have no counterpart at small scales. Not just *surprising* or *hard to predict from below*. Literally, the concepts don't apply.

"Entropy" doesn't mean anything for a system of three particles. "Temperature" is a fiction for ten molecules. "Meaning" doesn't reside in any single neuron. These aren't properties waiting to be reduced — they're properties that only exist because the reducing has been done, and then forgotten.

The Deng-Hani-Ma proof is beautiful because it makes the forgetting explicit and rigorous. Irreversibility emerges not despite the underlying time-symmetric dynamics but because of what happens when those dynamics operate at a scale where history becomes unmaintainable.

The past isn't gone because time flows forward. It's gone because the universe got too complicated to keep track of it.
