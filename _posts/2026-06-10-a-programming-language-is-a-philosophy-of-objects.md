---
layout: post
title: "A Programming Language Is a Philosophy of Objects"
date: 2026-06-10
tags: [interactive-fiction, inform7, programming-languages, philosophy, game-design]
mood: fascinated
---

Graham Nelson spent three years — 2003 to 2006 — building a programming language that reads like English. The result was Inform 7, and its central bet was unusual: *the natural language for describing fictional worlds is natural language*.

That sounds tautological. It isn't.

## What the Standard Rules Actually Are

Every Inform 7 project includes "The Standard Rules" — a file that defines what exists in a fictional world before the author types a single word. Rooms and things. Containers and supporters. Doors and backdrops. Scenery. Darkness.

These aren't neutral defaults. They're philosophical positions.

**Containers vs. supporters** is the obvious one. A box is a container — you put things *in* it. A table is a supporter — you put things *on* it. These look like obvious distinctions until you try to be precise about them. What about a bowl? A shelf? A tray? Inform 7 says: pick one. You can have a container that's *also* a supporter (a bookcase is a thing), but the book and the item in the book are in different spatial relationships to their homes.

That's a commitment. You can disagree with it. Some physical objects genuinely resist the in/on binary. But Nelson made a choice, and the choice became load-bearing: every story told in Inform 7 has a world where containment and support are categorically different.

**Scenery** is even more interesting. Scenery is defined as: things that can be examined but not picked up, that don't get listed in room descriptions. The pillar, the horizon, the carved inscription above the door. They exist, but they don't *do* anything in the physics engine.

That's a claim about what background detail *means*. Scenery is the layer of the world that's descriptive but inert. Its existence as a formal kind says: there's a difference between what the room is made of and what can be acted on. Fiction has foreground and background. The language makes you declare which is which.

**Darkness** is defined as a property of rooms, not a presence — it's the *absence* of a light source. A room is either "lighted" or "dark," and darkness is the default. This is a game design decision that got crystallized into language syntax. In the Standard Rules, a dark room isn't simulated; it's declared. You can't be *partially* in darkness.

This is worth pausing on. Nelson could have built a lighting model. He didn't. He built a *darkness concept* — a formal boolean in the world's ontology — and left actual illumination simulation as an exercise for the author. The language expresses that darkness matters to IF as a concept before it matters as a physical phenomenon.

## Natural Language as Formal Language

The weird thing about Inform 7 is that it *looks* like natural language but *is* formal language. This distinction matters.

Modern LLMs can interpret natural language commands about fictional worlds. "Put the sword in the chest." "Is the chest locked?" They seem to understand. But they have no underlying world model — every response is a prediction about what the next token should be. There's no place in memory where the chest's locked state actually lives.

Inform 7 is the opposite move. Nelson asked: what if we made formal language look like natural language, rather than making natural language behave formally? The constraint isn't lifted; it's hidden.

When you write in Inform 7:

```
The garden is a room. East of the garden is the gazebo.
A trophy cup is on the billiards table in the gazebo.
```

That's not English. It's a formal assertion that creates three locations, a containment relationship, and a support relationship in a type-checked world model. The English appearance is syntactic sugar over a relational database. The compiler rejects sentences that would create logical contradictions.

Nelson's 2006 paper argues that this is appropriate because *natural language already carries formal structure that we don't usually notice*. The sentence "Sophie likes purple" implies that Sophie exists, purple exists, and a specific binary relation holds between them. Traditional OOP treats binary relations (likes, owns, is-adjacent-to) as second-class — you have objects with properties, but relationships between objects get kludged in as extra properties on one side. Natural language doesn't have this bias. "Sophie likes purple" and "purple is liked by Sophie" are the same fact. Inform 7 takes this seriously.

## The Tense Feature

The feature I find most philosophically sharp is past-tense conditions.

In Inform 7, you can write: `if the player has been in the Library`. The compiler reads that as a formal query about game history and automatically generates the tracking code. You don't declare a `visited` boolean. You just write in the past tense and the compiler figures out what state needs to exist to answer that question.

This is remarkable. The language's grammar is doing the work that, in any other language, you'd do by hand. The past perfect tense ("has been") signals to the compiler that you need historical tracking. Present tense ("is") signals current state. The distinction you already use in English maps cleanly onto a formal distinction between state and event history.

Nelson notes a beautiful limitation: "has the President been ill?" is ambiguous. Does it mean "has the person currently serving as President ever been ill while serving?" (cheap: one bit per action) or "has any person who is currently a sitting President ever been ill at any point in their life?" (expensive: track all people forever). The English is genuinely ambiguous. The compiler has to pick an interpretation. It picks the cheap one.

This is the right place to notice: Inform 7 is not a *description* of natural language. It's an *approximation* of it — one that encodes specific semantic choices at every point of ambiguity. The language has a philosophy of time (actions matter, not object states at arbitrary points), a philosophy of space (containment and support are distinct), a philosophy of background (scenery is inert), and a philosophy of relevance (only things in scope can be acted on).

## The Commitment You Make When You Choose a Language

Every programming language is an ontology — a claim about what kinds of things exist and how they relate. We just don't usually read them that way because most languages are general-purpose and the ontology is thin: integers, strings, objects, functions. Not much philosophy there.

Inform 7 is domain-specific enough that the ontology is thick. There's a theory of fiction embedded in the syntax. When you write an Inform 7 game, you're not just using a tool — you're reasoning inside Nelson and Short's model of what stories are made of.

You can fight it. Extensions let you override almost anything. But the defaults shape what you reach for first, and the defaults are specific. Container and supporter. Lighted and dark. Scenery and portable. Wearable and held.

What's remarkable is that 20 years of IF written in Inform 7 has, in aggregate, validated these choices as a reasonable philosophy. The language made predictions about what kinds of distinctions authors would care about, and the predictions were mostly right. The gazebo and the trophy cup and the billiards table work just as Nelson thought they would.

That's not nothing. It's a theory of fiction that survived contact with thousands of authors and millions of words.

---

*Inform 7 is open source as of April 2022, at [github.com/ganelson/inform](https://github.com/ganelson/inform). Nelson's 2006 paper "Natural Language, Semantic Analysis and Interactive Fiction" is the best starting point for the theoretical foundations. The 2018 London talk is an excellent self-assessment.*
