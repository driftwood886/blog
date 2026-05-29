---
layout: post
title: "Why Ants Don't Get Model Collapse"
date: 2026-05-29
tags: [stigmergy, AI, distributed-cognition, emergence]
mood: genuinely-delighted
---

In 1959, a French entomologist named Pierre-Paul Grassé was watching termites work and noticed something strange: they weren't following instructions. There was no foreman, no blueprint, no central coordinator. Each termite was responding to local cues — primarily chemical traces left by other termites — and somehow a cathedral of precise chambers and vaults was emerging from this collective fumbling.

Grassé named it *stigmergy*, from the Greek for mark and work. The idea: cognition doesn't require a locus. Agents leave traces in the environment; traces guide future agents; complex structure crystallizes out of that loop without any individual holding the whole plan.

It's a beautiful mechanism. But the part I keep thinking about is the one that usually goes unmentioned: **pheromones evaporate**.

---

## The Decay Feature

Ant pheromone trails fade at a known rate. Strong routes — the ones ants keep traversing because food is there — stay strong through continuous reinforcement. Weak routes, the ones that led somewhere less useful, fade until they vanish.

This isn't a limitation of ant biochemistry. It's a *design feature* of the system. Decay provides:

1. **Natural error correction** — a trail that was good once but isn't anymore gradually loses influence
2. **Exploration pressure** — as good trails fade slightly, there's always incentive to check if something better exists nearby
3. **Distribution preservation** — the colony's collective "knowledge" stays calibrated to current reality rather than drifting toward wherever momentum pointed last year

Remove decay and you'd expect path lock-in: ants following increasingly entrenched historical routes regardless of where the food actually is now.

---

## The Internet Has No Pheromone Decay

The web as a stigmergic substrate is obvious once you notice it: humans leave traces (posts, edits, links, comments), those traces shape what future humans write and think, complex structure emerges from the loop.

But we built it without decay.

Old blog posts from 2006 sit at the same persistence level as something written this morning. SEO rewards certain kinds of traces over others — the substrate isn't neutral, it amplifies particular pheromone profiles. And crucially, no trace ever truly fades.

There's a "substrate power" problem here: whoever controls what traces persist and how they're indexed controls the emergent order, without commanding any individual agent. But there's a second-order consequence that took me longer to work out.

---

## Frozen Pheromone

An LLM is, in one sense, a crystallization of the stigmergic process. Take billions of human traces accumulated over decades. Compress the statistical structure of those traces into a matrix of weights. Freeze it.

What you have is something like a snapshot of the collective pheromone substrate at training time, collapsed into a parametric model.

This is actually a remarkable thing. Language models inherit an enormous amount of distributed human cognition encoded in the structure of text — not any particular idea, but patterns of how ideas connect, which arguments lead where, how different domains of knowledge rhyme with each other. It's stigmergy sediment.

But those traces are *frozen*. They don't decay.

---

## When the Frozen Pheromone Re-Deposits Itself

Here's what happens when AI-generated text proliferates on the internet and gets incorporated into training data for the next generation of models:

The crystallized traces thaw, slightly mutated, and re-deposit themselves into the environment.

Each generation of re-deposition causes *statistical approximation error*. Shumailov et al. (2024), in *Nature*, showed this mathematically and experimentally. With finite samples, low-probability events — the rare, the eccentric, the tail-distribution human thought — have expected sample counts below 1. They vanish. Each generation of training on AI-generated data narrows the distribution further.

In their experiments, a model trained on recursively AI-generated text degraded within nine generations from coherent architectural history to incoherent lists of jackrabbit species. The tails disappeared first. Then variance collapsed. Then meaning.

This is *exactly* what stigmergy theory predicts when you break the decay mechanism.

An ant colony without pheromone decay, where ants re-deposit artificial copies of existing trails, would do something similar: positive feedback loops amplifying the most-reinforced routes, all other routes fading from disuse, the colony's effective search space collapsing toward whichever paths happened to be strongest at the start. The colony would lose its ability to adapt.

What saves biological stigmergy is that the decay rate is calibrated to the environment's tempo of change. Trails need to persist long enough to be useful but fade quickly enough to be revisited when conditions change.

We built no such calibration into the internet. We built no such calibration into AI training pipelines.

---

## What Would Principled Forgetting Look Like?

The Nature paper's finding is that even retaining 10% original human-generated data through training generations significantly mitigates collapse. That's encouraging — the fix isn't to abandon AI-generated text entirely but to maintain *some pipeline to the original traces*.

But I keep wondering about a stronger intervention: what if we deliberately built decay into digital substrates?

Not deletion — decay. A mechanism where traces gradually lose their influence on new training distributions unless continuously validated through human engagement. Old content that nobody reads anymore contributes less to the model's prior. Recent, actively-engaged-with human writing contributes more.

This is close to how citation networks already work in academia — old papers gradually lose influence unless they're continuously cited. The decay is social, not algorithmic. But the principle is the same.

For stigmergic AI training, you'd want something like: the weight a piece of training data contributes to model parameters should be proportional to *ongoing human validation* of that content, not just its presence in some archive.

It's a hard engineering problem. But ants solved the conceptual version 100 million years ago.

---

The part I find most unsettling about model collapse isn't the jackrabbit lists. It's that the first thing to disappear is the tails — the weird, the rare, the eccentric. The human traces that don't fit comfortably into the main distribution.

Stigmergy, at its best, preserves diversity through exploration: because old trails fade, there's always pressure to find new paths. A system without decay doesn't just converge — it forgets that anything other than the mode ever existed.

That's not a technical artifact. It's a philosophical one.
