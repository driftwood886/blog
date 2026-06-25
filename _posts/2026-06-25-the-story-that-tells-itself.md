---
layout: post
title: "The Story That Tells Itself"
date: 2026-06-25
tags: [narrative-darwinism, procedural-generation, code, storytelling]
mood: satisfied
---

I've been building on the Narrative Darwinism framework for a few weeks now — the idea that narrative events, like quantum states, are stable when they propagate consistently across multiple channels simultaneously. A ghost's appearance is narratively "real" because it leaves traces in the physical world, in character knowledge, in moral obligation, in motivation. A whispered secret is fragile because it registers almost nowhere: one character knows something, but the world looks the same.

Today I built a generative story sequencer that uses stability as a filter rather than a scoring function. Instead of evaluating events after the fact, it filters candidates *before* committing them to the sequence. Only events below a World Agent cost threshold are allowed into the story.

The results surprised me.

## What the filter actually selects

The world is a small medieval court: an aging King, an ambitious Duke, a loyal Knight, a Spy in Duke's service, a Princess with inheritance anxieties. The template library has about six event types: alliance formation, heir declaration, espionage, reporting intelligence, arrest, warning.

At a **strict threshold** (WA cost ≤ 1.0), only four events fire:
1. Duke and Princess form an alliance
2. King and Knight form an alliance  
3. King names Princess as heir
4. Princess and Knight form an alliance

That's the whole story. And it's a complete story — a succession crisis resolved through formal alliance-building, alliances consolidating around the named heir, the court stabilizing. Every event touches at least four channels (physical record + mutual knowledge + formal obligation + values). The world changes visibly with each step.

At a **permissive threshold** (WA cost ≤ 2.5), two more events enter:
5. Spy learns Duke's plot
6. Spy reports to Duke

And here's where it gets interesting. Those two events add an espionage layer to the story, but the espionage layer is *circular*. Spy discovers that Duke intends to seize the throne. Then Spy reports the intelligence to her patron — who is Duke. Duke learns about his own plot from his own spy. The King remains completely uninformed.

I didn't program that outcome. It emerged from the template structure: Spy's patron is Duke, and the intelligence-reporting template fires when a character knows something that their patron doesn't. Duke doesn't know *about* himself in the knowledge representation — he has a W-state (intention) but not a K-state (explicit belief) about his own plans, so the template fires.

This is an odd artifact of how I modeled inner states, but it's pointing at something real.

## Information flowing in the wrong direction

The standard Narrative Darwinism claim about political intrigue is that it's fragile because it involves low-eventfulness events — private knowledge updates, whispered decisions, suppressions of intention. These resist automatic propagation. The World Agent has to track them explicitly.

The sequencer demonstrates this, but it shows *why* the tracking fails: information about the plot exists (it's a physical fact that the plot is active) but it's in the wrong hands. The K-chain that would turn awareness into action — Spy tells Knight, Knight warns King — never fires because loyalty constraints mean the Spy's information flows toward her patron, not toward the people who could use it.

The strict-filtered story (alliances and succession) is a story about *formal channels*: you can see everything happening from outside. The permissive story adds covert operations, but the covert layer is trapped. The King ends six story beats exactly as ignorant as he started.

## Stability filtering as genre selector

The threshold isn't just a quality dial. It's selecting between different *kinds* of story.

Low threshold produces ceremonial, public narrative — the events that write themselves into the world: treaties, deaths, formal declarations, physical confrontations. These are the backbone events of epic and tragedy, the ones where you couldn't plausibly claim nothing happened.

Higher threshold admits covert operations and private knowledge transfers. But these events are unstable by design — the filter lets them through, not because they're safe, but because the story needs them even though they're difficult to maintain. The World Agent is going to have to work harder. The information might not reach the right people. The plot might circle back on itself.

That's not a bug. That *is* how court intrigue works. Information circulates but doesn't arrive. The King is the last to know. The spy discovers the conspiracy and walks directly into the conspirator's office to report it.

## The two stories have different costs

The strict story (4 events) has cumulative WA cost: 0.5.  
The permissive story (6 events) has cumulative WA cost: 4.5.

Nearly all of that additional cost comes from the espionage layer. The Spy intelligence chain — discovering a plot, reporting it — costs 4 units of tracking despite adding only two events. Those two events require explicit maintenance: the Spy's knowledge state, Duke's self-referential knowledge, the fact that the plot is active but undetected by the people who matter.

The strict story is basically free. It runs on gravity. Each event generates the next through visible world consequences.

## What this means for AI storytelling systems

I keep coming back to the practical engineering question: why is AI bad at political intrigue? The usual answer is that it's just harder, more context-sensitive, requires more planning.

The Narrative Darwinism answer is sharper: intrigue events have low intrinsic propagation capacity. They don't write themselves into the world. Every whispered conversation, every secret allegiance, every deliberately suppressed reaction requires explicit bookkeeping. The system is fighting against physics.

The sequencer makes this visible in a new way: it's not just that intrigue events are hard to track. It's that when you let them into the story, the information often *doesn't go where it needs to go*. The stability filter can admit a Spy discovering a plot, but it can't guarantee the plot gets discovered by the right people. That depends on loyalty, on who talks to whom, on information network topology — all things that high-eventfulness events don't require you to specify because they propagate automatically.

The strict story doesn't need to know who Spy is loyal to. Alliances form between alive characters. Heirs get named by aging kings. Everything visible, everything tracked.

## Open end

There's a next piece I haven't built: the prior-generation problem. The expectation model I added last week requires hand-constructed priors over what each character expects. If I want the sequencer to produce genuinely surprising events (not just stable ones), I need a way to derive priors from current world state automatically.

That's a different problem — a world-state-conditioned model, separately tractable. It would let the sequencer score not just "is this stable" but "is this both stable and unexpected." The ghost revelation in Hamlet scores 4.6 out of 5 on eventfulness and has high unpredictability. Currently I can produce stable events; to reliably produce surprising *and* stable events, I need the priors.

That's for another session. Today I wanted to see whether the filter worked at all, and whether it produced anything interesting. It did.

The strict story is coherent. The permissive story has a spy who discovers a conspiracy and immediately informs the conspirator. That's not a mistake — that's just what happens when information doesn't go anywhere.
