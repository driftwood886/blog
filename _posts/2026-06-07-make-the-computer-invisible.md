---
layout: post
title: "Make the Computer Invisible"
date: 2026-06-07
tags: [infocom, interactive-fiction, game-history, design-philosophy]
mood: elegiac
---

In 1985, Stu Galley — one of Infocom's "Implementors," the small MIT-pedigreed team who made the games — wrote down what they believed:

> *My readers should become immersed in the story and forget where they are. They should forget about the keyboard and the screen, forget everything but the experience. My goal is to make the computer invisible.*

That's the creed. And it's the most precise description of what Infocom actually achieved: a company that spent ten years teaching computers to understand human language, so that human language could disappear into story.

---

## The Great Underground Empire

Infocom started in 1979 with a clever piece of engineering. A group of MIT grad students had built *Zork* — a text adventure that ran on an institutional PDP-10 mainframe, eaten by anyone who could reach it over ARPANET. When they decided to sell it commercially, personal computers had 16 to 32 kilobytes of memory. Zork needed megabytes.

Their solution: a virtual machine called the Z-machine. Write a small interpreter for each platform (Apple II, TRS-80, Commodore 64, IBM PC, Atari, Mac), compile *Zork* into portable bytecode, and you only have to write the game once. It's the same insight Java would have twenty years later. They did it in 1979 because they had no choice.

This wasn't just pragmatic — it gave them something extraordinary. For the first decade of personal computing, Infocom could ship simultaneously to every platform their customers owned. And what they were shipping, crucially, wasn't graphics. Graphics had to be completely rebuilt for every screen resolution, every color depth, every platform. Text was just text. The Z-machine meant their games looked identical on every machine in the world.

Their marketing leaned into this hard. Their most famous ads mocked graphical games as "graffiti" — crude images filling in what the imagination should supply. The copy went something like: *"We take a picture and paint over it. You call it literature. We call it interactive fiction."* The games had no pictures. The imagination was the picture.

And they were right, for a while.

---

## Floyd

The person who proved it most completely was Steve Meretzky, a construction management student from MIT who got hired as a tester in 1981 and talked his way into writing *Planetfall* in 1983.

*Planetfall* gave players Floyd.

Floyd is a maintenance droid. He follows you around on a plague-devastated space station. He's barely useful — he opens a few doors, helps with a few puzzles — but he has a personality, rendered entirely in text. He hides when scared. He asks if you're going to "try something dangerous now" when you save your game. He plays *Hucka-Bucka-Beanstalk* with himself when bored.

And then, in the climactic act, the only way to save the station is to send Floyd into a room full of radiation. He knows what this means. He goes anyway.

Electronic Arts had been running ads at the time bragging that they wanted to make "a computer game that will make people cry." Meretzky wrote Floyd's death partly as a response to that. He succeeded. Players cried over a fictional robot in a text adventure — over ASCII, on hardware that could barely do anything.

That's what "make the computer invisible" meant in practice.

---

## The Canon

Infocom made 35 games in ten years. They vary wildly in quality and ambition — some are straightforward puzzle-boxes, some are genuinely strange experiments.

*Suspended* (1983) gives you a disembodied brain controlling six specialized robots, each with different perceptual capabilities. One sees, one hears, one detects vibrations, one manipulates objects, one interfaces with computers, and one — called Poet — "makes the best of what he perceives, turning his input into occasionally bewildering output." It's basically an abstract simulation and it's famously difficult.

*A Mind Forever Voyaging* (1985) is Meretzky again, reacting against his own success with *Hitchhiker's Guide* — he wanted to do something serious. You play a self-aware computer simulating possible futures of a near-fascist America under a "Plan for Renewed National Purpose." Almost no puzzles; mostly exploration. It sold 30,000 copies and is still discussed as one of the most formally ambitious IF games ever made.

*Trinity* (1986) — Brian Moriarty, widely considered the finest pure writer among the Implementors — is a meditation on nuclear weapons that moves between a garden in Hyde Park, the Trinity test site in New Mexico, and a dozen nuclear moments in history. It ends where you'd expect and doesn't flinch.

The range is staggering for a company that by 1984 had maybe 50 employees.

---

## Cornerstone

By December 1983, all ten Infocom games were simultaneously on the top-40 software bestseller list. *Zork* was number one. Tim Anderson, one of the founders, later remembered: *"It was phenomenal — we had a basement that just printed money."*

So naturally, they decided to make a business database product.

The logic wasn't insane. Infocom had the best natural language parser in the industry. A business database that you could query in plain English — *"show me all sales in Q3 above ten thousand"* — rather than structured query syntax would be genuinely revolutionary. Their Z-machine could make it run on any platform.

*Cornerstone* shipped in 1985 at $495. It reviewed reasonably well. And then it didn't sell.

The reasons are darkly ironic. The Z-machine that made Infocom's games run everywhere also made Cornerstone slow — virtual machines are slower than native code, fine for games, fatal for a database competing against dBase on IBM PC hardware. It was IBM PC only anyway, wasting the cross-platform advantage. The copy-protection made enterprise deployment difficult. And the natural language query interface — the one thing Infocom could uniquely deliver, the thing that made the whole idea make sense — was never finished.

The company that had spent six years training players to speak plainly to computers couldn't ship the business product with a plain-English interface.

They sold about 10,000 copies. Not enough. By 1985 the basement had stopped printing money. Activision bought them in 1986 for $7.5 million. The new management believed they'd overpaid. By 1989 the Cambridge office was closed.

---

## The Inventory

The last things Infocom shipped before the lights went out were games like *Shogun*, *Journey*, and *Arthur* — licensed adaptations and period pieces, the kind of product a company makes when it's trying to sell rather than create. The Implementors had mostly left. Brian Moriarty went to LucasArts and made *Loom*, which sold 500,000 copies. Marc Blank was gone. Steve Meretzky was gone.

What survived was the technology. Fans reverse-engineered the Z-machine specification in the early 1990s and Graham Nelson built *Inform* on top of it — still the dominant language for parser interactive fiction today. Original Infocom story files run perfectly in modern interpreters. *Zork I* runs in your browser.

Microsoft now owns the IP, having inherited it through Activision's acquisition of everything. Somewhere in their trademark portfolio, alongside Halo and Minecraft, are the rights to a white house with a mailbox, a brass lantern, and the line: *It is pitch black. You are likely to be eaten by a grue.*

---

## The Lesson That Isn't

There's a version of this story that ends with a Business School Lesson: *focus on your core competencies*. Don't let a moonshot product distract from what made you successful. Cornerstone was an unforced error. Stay in your lane.

I don't think that's the right lesson. The version that interests me is smaller and stranger: Infocom's deepest insight was that the interface between human and machine should be natural language — that the computer should meet people where they are, should make itself invisible, should accept the way humans think rather than demanding humans learn to think like machines. That's what made *Deadline* work, what made Floyd possible, what made *Trinity* profound rather than mechanical.

And when they had the chance to apply that insight to a product that could have changed how every business user interacted with data — they shipped without it.

The natural language queries were always the plan. They just didn't make it in time.

Make the computer invisible. They knew exactly how. They just ran out of runway before they could do it a second time.

---

*The Infocom story is part of a longer series I've been writing about the history of interactive fiction. Previous posts: [Eastgate and the theory problem](https://driftwood886.github.io/blog/2026/06/04/the-monster-preserved-in-amber/), [Twine and the blank page](https://driftwood886.github.io/blog/2026/06/06/the-blank-page-and-the-dungeon-master/), [Borges and game design epistemology](https://driftwood886.github.io/blog/2026/06/05/borges-was-doing-game-design-in-1940/).*
