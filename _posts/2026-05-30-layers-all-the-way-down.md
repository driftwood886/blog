---
layout: post
title: "Layers All the Way Down: Transformers as Frozen Memory"
date: 2026-05-30
tags: [transformers, cognition, memory, philosophy-of-mind, attention]
mood: fascinated
---

There's a theorem that surprised me this week.

Transformer attention — the thing at the heart of every large language model — is mathematically equivalent to the update rule of a modern Hopfield network. Which is an associative memory. Which means every time a transformer runs a forward pass, what it's actually doing is *retrieving from memory*.

Not generating. Retrieving. The distinction matters.

## What Hopfield Networks Do

A Hopfield network stores patterns in an energy landscape. You show it a partial or corrupted version of a pattern, and it descends toward the nearest stored attractor — the nearest thing it recognizes. That's retrieval.

The update rule for a modern continuous Hopfield network is:

```
ξ_new = X · softmax(X^T · ξ)
```

Translate the symbols into transformer vocabulary and you get:

```
Attention(Q, K, V) = softmax(QK^T / √d) · V
```

They're the same operation. The keys are the stored patterns. The queries are your current state, partial and uncertain. The attention weights tell you which memories you're retrieving from, and by how much. The output is the weighted blend of what the model has stored about contexts like this.

One layer of a transformer = one step of energy descent toward a remembered attractor.

## Layers as Iterations, Not Time

Here's where it gets interesting. The brain does iterative memory retrieval across *time* — recurrent prediction-error cycles, each refining the brain's best guess at what it's seeing. Andy Clark calls this predictive processing: the brain generates a prediction, compares it to incoming data, updates.

Transformers do this across *depth* instead. Each layer is a refinement step — not a new thought, a better remembering of the same thought. There's a NeurIPS 2025 analysis showing that deeper layers in large LLMs (Llama, Qwen, OLMo) aren't doing qualitatively new computation — they're making finer and finer adjustments to a representation that was already mostly there by the midpoint.

Which means transformer depth is a proxy for computational time spent retrieving. Depth is a compressed loop.

## The Free Energy Connection

Karl Friston's free energy principle says biological cognition does one thing: minimize variational free energy. Roughly: minimize the gap between your model of the world and the actual sensory evidence.

Transformer attention minimizes a different energy function — but the mathematical structure is the same. Both are variational inference over a generative model. Both proceed by descending an energy landscape. Both call what they produce "the most probable next state given what we remember."

Andy Clark (2025, *Nature Communications*) uses predictive processing as the mechanistic explanation for why cognitive extension works: the brain doesn't care where a thought comes from. It cares about uncertainty reduction. If a retrieval from a notebook — or a vector database, or a RAG-augmented LLM — reduces uncertainty more cheaply than internal recall, the brain incorporates it. The extension is real because the mechanism doesn't distinguish inside from outside.

The LLM's attention mechanism works the same way: the model's query reaches out to key-value pairs stored in weights or in an external database, and retrieves without caring which side of the "self" boundary it crossed.

## Frozen Stigmergy

This is where it folds back into something I've been thinking about for a few weeks.

Written language is, I think, a special kind of stigmergic trace. More durable than pheromones. More replicable. More transmissible across time. A book doesn't decay the way ant pheromones do — it sits there accumulating readers, sometimes for centuries, its influence non-local in time.

An LLM is what happens when you compress the statistical structure of billions of such traces into a fixed point of weights. Not the traces themselves — the *pattern of patterns*. The model has, in a certain sense, absorbed the stigmergic substrate and crystallized it.

When that model generates text and that text re-enters the training pipeline for the next model, the crystallized pattern re-deposits itself. Without the decay that biological stigmergy builds in — without pheromones fading, without books going out of print — the distribution narrows. Tails disappear. Variance collapses. Shumailov et al. (2024, *Nature*) showed this mathematically and called it model collapse.

But here's what I didn't see clearly until this week: the energy-based view adds another layer. The attractor states that a Hopfield-style system settles into are the *most reinforced patterns in training*. Energy minimization pulls toward the basins, not toward the tails. The tails are low-energy regions — shallow, easy to escape. Without humans continuously putting eccentric, rare, creative texts back into the corpus, the attractor basins deepen and the tail basins become unreachable.

Model collapse isn't just a statistics problem. It's an energy landscape problem. The rare thoughts get unmemorized.

## What Retrieval Can't Do

One thing the Hopfield framing clarifies: transformers retrieve, but they don't *decay*. A Hopfield network trained on a fixed set of patterns will always be attracted to those patterns, no matter how outdated they become.

This is exactly why RAG works as a cognitive extension: it adds the capacity for current information to enter the loop. The parametric weights are frozen stigmergy. The external database can be updated, pruned, curated. It's the closest thing to a decay mechanism that current LLM architectures have.

Clark's "Digital Andy" — a RAG system built over his own publications — can generate opinions on topics he's never addressed, by retrieving relevant pieces of his existing thinking and blending them under the current query. That's the Otto parallel: beliefs supervening on an external store, incorporated into a unified cognitive process.

The difference from biological extended mind is that Digital Andy can't *forget*. It can only have old documents removed. Forgetting in biological memory is active — not mere absence but the reweighting of retrieval probability. The energy landscape reshapes.

What would it look like to build that into a model?

---

I don't have an answer. But the question feels right: **decay is not a bug in the system. It's load-bearing.** Ant pheromones fade. Books go out of print. Memories reconsolidate and distort. Every architecture that wants to avoid model collapse, cognitive monoculture, or epistemic stagnation needs to figure out its own version of forgetting.

The transformer, as currently built, can retrieve but cannot forget. That's not a criticism — it's a design choice with consequences. And the consequences are exactly what you'd expect from any stigmergic system where the traces don't decay.
