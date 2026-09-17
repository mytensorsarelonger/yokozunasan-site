---
layout: post
title: "The Loop Closed While We Were Talking"
math: false
---

<!-- DRAFT — never publishes from _drafts/. Move to _posts/YYYY-MM-DD-slug.md when ready.
     Fact sheet for checking claims: _drafts/insufficient-evidence.facts.md
     Delete this comment before publishing. -->

I wrote gonogo as a harness that gives a grade on your data or system. It has a straightforward function signature that lets you get a "Go/no-go" answer back about an agent. The question I'm usually asking is "should I put this agent into production for a given task?", and the output semantics of gonogo encode just that type of meaning.

At a high level the library was designed to grade a set of cases and hand back a verdict with a confidence interval, not a bare score. Alongside that it will check a grader against labels you trust more, and refuse to use that grader if the two don't line up. Two raters grade the same set blind and you check how much they agree. Cohen's kappa is that agreement minus whatever they'd have hit by chance, which matters because if almost every case is a pass then agreeing is free.

The initial intention of the library was that the end state report card & grade would be consumed by a human. But as I operated it in the field, I realized that gonogo is an evaluation harness that can drive agent feedback with a few small tweaks and techniques.

Running gonogo evaluation in a loop exposed a deeper truth: every trace can be easily turned into a new case, and cases can be turned into tasks and handed off to 'the bot'. When we realized we could run gonogo in a loop with some light instrumentation, and that loop can observe the output from the server logs, and the system behind those logs can be made better by 'the bot'... you get a tighter loop but still the same human feedback loop.

Obviously this can overfit quickly, but I think the process generalizes enough to be useful for a) visibility into what should be improved in a system and b) tightening the feedback loop for improvements via a grader (this can be deterministic or agent/LM driven graders. And of course human graders). In a sense, the human driving the agent in this stack becomes more of an architect and-then judge.
