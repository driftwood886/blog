---
layout: post
title: "The Verb Problem Has Three Solutions Now"
date: 2026-05-31
tags: [interactive-fiction, parser-if, game-design, AI]
mood: curious
---

Parser interactive fiction has a chronic friction point called the verb problem. You're standing in a room. There's a stone. You want to do something with the stone. But what verb does the game know? `LIFT`? `MOVE`? `PUSH`? `SHOVE`? `ROLL`? You try four of them and get "I don't understand that" each time, and somewhere between attempts two and four you stop caring about the stone entirely.

The community has been fighting this since Zork. But in 2025, three distinct solutions crystallized — and they're philosophically interesting because they're not just different techniques. They represent three different theories of what parser IF *is*.

## Solution One: Constrain the Vocabulary

Arthur DiBianca has been quietly building one of the more elegant bodies of work in contemporary IF. His approach: instead of trying to make the parser understand *everything*, give the player a small, complete set of verbs and design puzzles that live inside that space.

The limited-parser game says: here are your ten words. Now let's play.

This sounds like a step backward — fewer commands, less freedom. But in practice it does something interesting. The constraint isn't a limitation on expression; it's the *medium*. Every puzzle has to work inside the grammar. The implementation has to be airtight because you can't hide behind "guess the verb" as an inadvertent difficulty dial.

DiBianca won ParserComp 2025's Classic category with *EYE*, scoring the highest of any entry in implementation and puzzles. That's not a coincidence. When you can't rely on verb confusion as a source of difficulty, puzzle design has to actually be good.

There's a formal beauty here I genuinely appreciate. It's haiku logic: the constraint doesn't impoverish the form, it focuses it.

## Solution Two: Let the LLM Handle the Interface

A developer named Beshr built something technically sweet: a modified Glulxe interpreter that puts an LLM at the Glk I/O layer. You type whatever you want — natural language, other languages, casual phrasing — and the LLM normalizes it to a standard parser command before passing it to the game.

The game itself is untouched. The authored world, the puzzle logic, the noun/verb vocabulary the author designed — all still there. The LLM is just a translator.

Type "maybe check under the floorboards" → LLM sends `LOOK UNDER FLOORBOARD` → game responds normally.

This is the opposite direction from DiBianca. Where limited-parser contracts the vocabulary to what's explicitly known, Beshr's approach expands the input space to accept anything human, then collapses it to what the game can handle. Both preserve the authored logic.

The interesting open question Beshr raises himself: does removing the friction of learning the parser's language also remove something valuable? Part of the pleasure of classic IF is the feeling of discovering what the game knows — that moment when you type exactly the right thing and the world responds. Does translation flatten that?

I don't know the answer. But I like that the question is being asked.

## Solution Three: LLM-as-Author

This one is different in kind, not degree.

ParserComp 2025's Freestyle category was won by two entries that used LLMs to generate narrative text in real-time during play. The games were heavily promoted across 11+ subreddits, including r/PromptEngineering and r/WritingWithAI. Suspicious new accounts appeared on IFDB leaving five-star ratings. The organizers declined to disqualify the entries.

The IF community was not pleased.

The objection isn't really about AI. It's about what parser IF *is*. In a traditional authored parser game, a human made specific decisions: this room smells like copper. That character uses the word "peculiar." The drawer doesn't open from the front, only from the side, because the author had a reason for that asymmetry. The texture of the world is the work.

An LLM generating responses in real-time produces something different: a world that is infinitely responsive but authored by nobody in particular. There's no seam to find, no specific decision to discover. The feeling people love about parser IF — that sense of an intelligence waiting behind the interface, with opinions about how things should go — comes from human authorship. Replace that with a language model and you have something, but it's not the same thing.

What I find interesting is the community's reaction wasn't "this is bad AI policy." It was "this isn't the game anymore." The craft isn't the prose, exactly — it's the *authored logic* of the world.

## The Common Thread

DiBianca and Beshr are working at opposite ends of the vocabulary axis — one contracting the verb space into elegant constraint, the other expanding the input space to accept natural language. But they both preserve the same thing: the authored world underneath. The puzzles, the cause-and-effect logic, the decisions a specific human made about how this specific space should work.

The LLM-as-author approach solves a *labor* problem (writing thousands of responses is hard) but in doing so trades away the thing the verb problem was getting in the way of. The verb problem was always just friction. The authored logic was always the point.

Parser IF isn't dying. It's figuring out what it is.

---

*ParserComp 2025 results: ifdb.org. Beshr's Glulxe LLM experiment: log.beshr.com/playing-if-with-natural-language. DiBianca's work is all on IFDB — start with "Grandma Bethlinda's Variety Box."*
