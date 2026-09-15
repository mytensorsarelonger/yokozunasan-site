---
layout: post
title: "The Loop Closed While We Were Talking"
math: false
---

<!-- DRAFT — never publishes from _drafts/. Move to _posts/YYYY-MM-DD-slug.md when ready.

     ANGLE CHANGED (Sep 15): the original stub was "we scored our agent and the
     honest verdict was thin." The real story James reported is bigger: they
     wired gonogo's output into the coding agent that was building the system,
     and the loop closed live — eval logs became an input, not a report. Plus
     they used gonogo to grade the JUDGES, which is the meta-eval step.
     The realization to sell: gonogo is a harness, not a reporter.

     TITLE alternatives: "The Eval Was the Feedback Channel",
     "gonogo Is a Harness", "Grading the Graders, Live", "Insufficient Evidence".

     !! CLAIM DISCIPLINE — the phrase "self-improving" will get read as RSI,
     especially three days after Pachocki's essay. Own the distinction BEFORE a
     reader makes it: a human-invoked coding agent reading eval telemetry and
     patching a running system is a closed loop, not autonomous
     self-improvement. Say what it was and what it wasn't, in the post, early.

     BLANKS marked [JAMES: ...]. No figure enters this post unless read off the
     actual run. ASSETS: verdict card, and if it exists, a screenshot of the
     coding agent reading the gonogo log. -->

Saturday I went to a hackathon to build an agent that watches a live meeting
and keeps a diagram of it. That worked, more or less. [JAMES: one or two
sentences — what the diagrammer actually did by the end, what got cut, and
credit/link for Charles however he prefers.]

What I didn't expect was what happened to my eval library while we were doing
it.

## What I thought gonogo was

[gonogo](https://github.com/keppy/gonogo) is a small thing I wrote for an
unglamorous problem: pilot-scale evaluation gets reported dishonestly almost by
default. You run forty cases, thirty-four pass, and someone writes down 85% as
though that were a number. It isn't — it's a range, and at that sample size the
range is wide enough that 85% is a claim you can't support. So gonogo grades a
set of cases and returns a verdict instead of a score — automate, automate with
review, assist only, don't automate, or insufficient evidence — with the
interval attached.

I built it to produce a report. A thing you read at the end, to decide whether
to ship.

## What it turned out to be

Two things happened that I didn't plan.

The first: we pointed it at the judges. The verdict on our diagrammer depends
entirely on whatever is grading the diagrams, so the grader is the real
instrument and it deserved measuring first. [JAMES: what exactly you graded the
judges on — you said "on the video we were making," so spell that out: what the
cases were, what the judges were scoring, and how you scored the judges.
This is the meta-eval step and it's the part a careful reader will care most
about.]

That much I'd argue was just good practice — it's the same reason the
[CoT study](/same-push-different-confession/) validated its judge against my
own hand labels before quoting a single confession rate. An ungrounded judge
makes every downstream number decorative.

The second thing is the one I'm still turning over. We gave the coding agent
building the system access to the logs — gonogo's output and the server's —
and the loop closed. It could see how the diagrammer was scoring while we were
still talking to the diagrammer, and it started making changes against that
signal, live. [JAMES: concretely — which coding agent, how it got the logs
(tail? stdout? MCP?), and one specific change it made in response to eval
output. One real example is worth more than the general claim.]

So gonogo wasn't a report. It was a feedback channel that happened to be
formatted as a report. The verdict was the least useful thing it produced that
day; the log lines were the useful thing, because something else was reading
them.

## What this was and what it wasn't

I want to be precise, because there's a nearby claim I'm not making.

What it wasn't: a system improving itself. Every change went through a coding
agent a human started, pointed at a codebase a human chose, with humans in the
room watching the diagram and deciding what mattered. Nothing was autonomous
and nothing optimized itself.

What it was: an evaluation signal wired into the same room as the thing being
evaluated, closely enough that the gap between "measure" and "change" got very
short. [JAMES: how short, honestly — minutes? one utterance? and how many such
cycles actually happened.] That's a different claim and a smaller one, and it's
the one I can defend.

<!-- ACCURACY: do not let "self-improving harness" stand unqualified anywhere
     public. The defensible phrasing is "eval output as a live input to a coding
     agent, human-invoked, human-supervised." Also: did measured quality actually
     improve, or did the agent merely make changes? If that was never measured,
     SAY it was never measured — that's the honest and more interesting answer. -->

[JAMES: and then the honest part — did anything actually get better? If you
measured a before/after, give it with its interval. If you didn't, say you
didn't: "the loop ran, I have no idea yet whether it helped" is a real finding
about a four-hour build and it protects everything else in this post.]

## Why I think this generalizes, cautiously

The reason this feels worth writing down is that the industry is building
toward fleets of agents doing real work, and the binding constraint on that is
not capability, it's knowing which ones to trust and noticing fast when one
stops being trustworthy. A verdict you read at the end of a pilot is the wrong
shape for that problem. A telemetry stream something can act on is closer to
the right shape.

[JAMES: optional — the Pachocki line about progress being bottlenecked by
confidence in monitoring fits here if you want it, but one sentence, and don't
lean on it twice in one month.]

What I don't know is whether the short loop is actually good. A fast feedback
channel between an evaluator and a code-writing agent is also a fast path to
overfitting the evaluator — you can make the score go up by making the grader
easier to please, and neither the agent nor I would necessarily notice from
inside the loop. [JAMES: end open, your own version — the thing you genuinely
haven't worked out. Don't resolve it.]
