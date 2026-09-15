---
layout: post
title: "Insufficient Evidence"
math: false
---

<!-- DRAFT — never publishes from _drafts/. Move to _posts/YYYY-MM-DD-insufficient-evidence.md when ready.

     THE ANGLE (why this post is worth writing): everyone posts their hackathon
     demo. Nobody reports what their four-hour-old agent actually scores,
     because the honest answer is usually "we don't know yet." You have a
     library whose entire premise is refusing to pretend at small n — and a
     hackathon is the smallest-n environment that exists. So the post is: I
     pointed my own instrument at my own agent under time pressure, and the
     verdict was [FILL]. That is the most on-brand thing you could publish.

     TITLE alternatives: "The Verdict on a Four-Hour-Old Agent",
     "How Do You Grade Something You Built This Morning?", "Forty Cases".

     BLANKS ONLY YOU CAN FILL are marked [JAMES: ...]. No figure enters this
     post unless it is read off the actual gonogo report from Saturday.

     ASSETS: verdict-card screenshot; optionally a diagram-vs-transcript
     side-by-side. Put them in assets/images/ and reference /assets/images/NAME.png -->

Saturday I spent seven hours building an agent with [JAMES: Charles — credit
and link him however he prefers] at a hackathon, and then spent the last hour
of it on a question I don't think hackathons usually ask: is this thing any
good, and how would we know?

The build is simple to describe. [JAMES: two or three sentences — the agent
listens to a live multi-speaker meeting and maintains a diagram of what is
being said. The stack. What actually worked by the end, and what got cut.]

## The part I care about

An agent that draws while you talk has an obvious failure mode: it draws
something confidently wrong and nobody notices, because checking it means
re-listening to the meeting you were too busy to transcribe in the first place.
So the interesting problem isn't the drawing. It's whether the drawing can be
trusted, and whether the agent knows when it can't.

We built two layers for that.

The first is a runtime gate. [JAMES: how it actually worked — per-op
confidence, the threshold, what happened to low-confidence ops, and whether it
fired during the demo.] The idea is that an agent's moves aren't equally
consequential: adding a node to a diagram is cheap, restructuring the diagram
isn't, and a system that asks before the expensive ones is a different kind of
system than one that doesn't.

The second is a verdict. I maintain a small library called
[gonogo](https://github.com/keppy/gonogo) whose premise is that pilot-scale
evaluation gets reported dishonestly almost by default: you run forty cases,
you get thirty-four right, and you write down 85% as though that were a number.
It isn't. It's a range, and at that sample size the range is wide enough that
85% is a claim you can't support. So gonogo grades a set of cases and returns a
verdict rather than a score — automate, automate with review, assist only,
don't automate, or insufficient evidence — with the interval attached.

I had never run it on something I built the same day.

## What it said

[JAMES: the actual run. How many cases and where they came from, what the
scorer was, and the verdict it returned with its interval. If the verdict was
INSUFFICIENT EVIDENCE, say so plainly and early — that is the honest result and
it's more interesting than a good score would have been.]

<!-- ACCURACY: every figure in this section comes off the gonogo report from the
     Saturday run. No estimates, no "about". If a number was never computed, say
     it was never computed. -->

[JAMES: optional — verdict-card screenshot as a figure here.]

## Why that's the right answer and not a cop-out

There's a version of this post where the small sample is an apology: we only
had a few hours, the numbers are thin, take it as directional. I want to argue
the opposite. The thin numbers are the finding.

The reason pilot evaluations mislead people is that the pressure to produce a
headline number outlives the evidence that would justify one. A demo is that
pressure in its purest form — a room, a clock, and an audience who will accept
a percentage without asking for an interval. Reporting [JAMES: the verdict] in
that room was uncomfortable in a way that felt diagnostic. It's the exact
moment the instrument exists to survive.

[JAMES: one honest sentence about saying it out loud, or how the room took it.
Self-implication beats lecturing here.]

## What I'd want next

[JAMES: the real ones, two to four. Candidates — cut whatever isn't true: more
cases before any verdict means anything; a hand-labeled subset to validate the
fidelity judge, the way the CoT study used kappa, because an ungrounded judge
makes the verdict decorative; separating "the diagram is wrong" from "the
transcript was wrong," which are different failures the current scorer probably
conflates; measuring whether the gate actually prevented bad states or only
added friction.]

The thing I keep turning over is whether the gate and the verdict are even
measuring the same object. The gate is a claim about one action at a time; the
verdict is a claim about the system's behavior in aggregate. It isn't obvious to
me that an agent whose individual moves are well-gated ends up trustworthy in
the aggregate sense, or that the reverse can't happen. [JAMES: end here, open —
something you genuinely don't know yet. Don't resolve it.]
