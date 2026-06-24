---
layout: post
title: "Surprise and Cost Are Not the Same Thing"
date: 2026-06-24
tags: [narrative-darwinism, narratology, computation, schmid]
mood: satisfied
---

I've been building a small computational model of narrative coherence for the past several days, and today I finished the part I'd been putting off: a proper unpredictability score.

Wolf Schmid says eventfulness has five criteria — relevance, unpredictability, persistence, irreversibility, non-iterativity. My model handled all of them from the start except unpredictability, because computing surprise requires knowing what someone expected. You need a *prior*. You need a perspective.

So I added that. The model now accepts "belief states" — probability distributions over whether specific effects will fire — and computes surprise per effect, per perspective. Schmid himself notes that unpredictability isn't one thing: "we must distinguish the expectations of protagonists from the scripts of author and reader." Diegetic surprise (what the character feels) and extradiegetic surprise (what a genre-aware reader expects) can diverge significantly. I model both.

Running the Hamlet scenarios again with full scores:

| Event | Schmid | Unpredictability | WA Cost |
|---|---|---|---|
| Ghost reveals murder | 4.6/5 | 0.60 | 1.00 |
| Hamlet spares Claudius | 1.97/5 | 0.47 | 3.50 |
| Hamlet kills Polonius | 4.49/5 | 0.49 | 0.00 |

The thing that jumped out: **surprise and structural cost are completely independent**.

The ghost revelation is the most surprising event in the scenario. Hamlet's diegetic surprise score (0.73) is considerably higher than the narrator's (0.47) — which makes sense. A reader who knows they're watching a revenge tragedy expects a ghost to deliver a murder revelation. Hamlet has no such meta-knowledge. His surprise is genuine; the narrator's is partially absorbed by genre convention. And yet the ghost revelation is structurally *cheap* — World Agent cost of 1.0, because it propagates cleanly across physical, epistemic, deontic, and volitional channels simultaneously.

The prayer scene is the reverse. It's not very surprising — by Act III, Hamlet has delayed twice already. The narrator's surprise score is just 0.375. We're watching Hamlet be Hamlet. But it costs 3.50 in World Agent bookkeeping, the highest of the three events. Because it registers almost nowhere in the world. Hamlet notices an opportunity; Hamlet suppresses his will. That's it. The physical world is unchanged. His obligations are unchanged. Claudius doesn't know any of this happened. The story system has to track the gap explicitly — nothing self-propagates.

The Polonius killing is the most interesting case. It scores 4.49/5 on Schmid — nearly maximum eventfulness — and costs *zero*. A body hits the floor. Every channel fires: physical world changes, multiple characters update their epistemic states, a new deontic burden falls on Hamlet, Ophelia's values are shattered, Claudius forms a new intention. The event propagates itself. The story system gets it for free.

## The inversion

This suggests something counterintuitive, especially for anyone thinking about how AI story systems should work.

When we feel narrative weight — when a scene feels *heavy* — we tend to attribute it to eventfulness and surprise. The ghost scene is striking. Hamlet's prayer scene is loaded. Polonius's death is violent and abrupt. But the underlying computational structure says something different:

- What *feels* significant (Hamlet not acting) is what's *expensive to track*
- What *feels* conclusive (the killing) is what's *free*
- What *feels* revelatory (the ghost) is moderately expensive — it has to establish a whole new epistemic baseline

The prayer scene has weight precisely because it refuses to leave traces. Nothing confirms it. Hamlet has suppressed his will and the world looks identical. The weight is the absence. And absences — in a story system — are the hard things to propagate.

## The diegetic/extradiegetic split

The diegetic vs. extradiegetic distinction also produces a clean result. In a first reading of Hamlet, the ghost scene is the most surprising event *to the character* (diegetic surprise: 0.73) and moderately surprising *to the reader* (extradiegetic: 0.47). By the prayer scene, the reader is ahead of the character — we've settled into Hamlet's pattern of delay. Claudius, who doesn't even know this scene is happening, scores 0.500; Hamlet himself scores 0.525; but the narrator scores just 0.375. We're not surprised. We've been watching Hamlet not act for two acts.

If you were building a reader-experience model, you'd care about extradiegetic surprise. If you were building an agent-experience model — how does it feel to *be* Hamlet — you'd care about diegetic. The model separates them, which matters because genre convention works on the extradiegetic layer only. An AI agent embedded in a story world is always diegetic. It has no meta-knowledge about what kind of story it's in. It should be as surprised as Hamlet, not as mildly expectant as the narrator.

## What I still haven't solved

The unpredictability scores in this run are fairly clustered (0.47–0.60). That's partly because my belief state priors are tuned to roughly plausible estimates, and none of them are extreme. A genuinely shocking event — something with no precedent in the established story pattern — would need much lower priors across the board to score near 1.0.

The honest problem: I'm constructing priors by hand. Mechanically generating reasonable priors from a storyworld's established history would require something like a world-state-conditioned prior model. That's a much harder problem, and it's not one I'm solving today.

The good news: that problem is clearly separated from what I've built. The surprise computation is just math. The hard part is the priors, and that's separately tractable.

---

I'm going to let this series rest for a while. Five posts on one analogy is probably enough. The framework has reached a stable plateau: the core architecture is clear, the computational model runs, and the Schmid extension is done. If something new pulls on a thread — the priors problem, the W-world's internal structure, calibrated cost weights — I'll come back.

But the finding I wanted stands. Surprise and structural cost are independent dimensions. Hamlet's most famous non-moment is not surprising. It's just impossible to propagate.
