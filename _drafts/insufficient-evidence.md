---
layout: post
title: "The Loop Closed While We Were Talking"
math: false
---

<!-- DRAFT — never publishes from _drafts/. Move to _posts/YYYY-MM-DD-slug.md when ready.

     Sep 15 (eve): fact blanks FILLED from primary sources — live-diagrammer
     README / SUBMISSION.md / commit log, gonogo CHANGELOG 0.2.0 and
     docs/proposals/2026-09-live-diagrammer.md, the report-card README and the
     card footers. Every number below traces to one of those. Voice slots are
     still marked [JAMES:]. Delete every comment before publishing.

     !! CLAIM RECONCILIATION. The tracker note said "the coding agent had the
     gonogo + server logs and changed the system live." The public copy you
     already checked and shipped (SUBMISSION.md) says: "The harness finds it,
     a person fixes it, the harness confirms." Every Saturday commit is
     co-authored by a coding agent, so both are true — but the shipped wording
     is the defensible one, and it is what this draft uses. If you want to say
     the agent read the eval output directly, say HOW (tail of out/*.log.jsonl?
     the card html? pasted?) — I could not source that from the repo.

     !! VIDEO: public, but NOT linked from here or from @yok0zuna (your call,
     Sep 15). Repo link only. -->

Saturday I went to a hackathon to build an agent that watches a live meeting
and keeps a diagram of it. That worked, more or less. It sits inside a Google
Meet, hears the tab and the mic as two channels, and a few seconds behind
speech it draws boxes and arrows on a side panel — about 2.9 seconds from
speech to pixels at the median on the recorded take. Two buttons: snapshot
pins the board, dismiss parks whatever was just drawn and hands it back to the
model as a negative example for the next three calls. Charles Parker built the
agent itself — capture, transcription, the extractor, the reducer, the panel; I
built the evaluation layer. Repo:
[cdiddy77/live-diagrammer](https://github.com/cdiddy77/live-diagrammer).
[JAMES: adjust the credit line to how Charles wants it.]

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
instrument and it deserved measuring first. The night before, I had an LLM
judge grading boards against the conversation, and three sets of labels to
check it against on the same cases: structural checks (orphan nodes, duplicate
edges, rejected ops), an unrelated model family, and two humans — Charles and
me. Kappa against the structural labels was 0.36, against the other model
0.42, against the humans 0.09. And the two humans agreed with each other 40%
of the time. So there was no human validation to claim, and the card says
exactly that.

<!-- ACCURACY: bronze κ 0.36 / silver κ 0.42 / gold κ 0.09, inter-rater 0.40 —
     gonogo docs/proposals/2026-09-live-diagrammer.md §1, and the card-10runs
     footer ("2 raters, 10 cases; the raters agreed with each other 40% of the
     time, so no human calibration is claimed"). The footer says 10 cases for
     the human tier; the proposal table says 20 for the same set. Check which
     before quoting a case count — I left it out above. -->

Which is why the demo card doesn't use that judge at all. A storyboard says
what gets said and what the board should look like after each beat; a
deterministic matcher scores the live board against the intended one on node
and edge F1. No model grades the model. The LLM judge stayed where there is no
target board — 35 human-annotated segments of real meetings from the AMI
corpus, where it scores 57% [41%, 72%], boards under about sixteen nodes
passing and boards past twenty mostly not.

That much I'd argue was just good practice — it's the same reason the
[CoT study](/same-push-different-confession/) validated its judge against my
own hand labels before quoting a single confession rate. An ungrounded judge
makes every downstream number decorative. The difference this time is that the
validation came back and said the judge wasn't usable, so we didn't use it.

The second thing is the one I'm still turning over. Every capture becomes test
cases with one command, the cases accumulate in the repo, and the same command
grades any change against every take so far. So the harness stopped being a
thing you run at the end and became the thing in the room. Pre-flight at 11:33:
ten replays of the storyboard recording, every segment matched in 10 of 10
runs. First live take graded at 13:26, and it exposed three defects in the
grader itself — segments cut at clock time while ops land seconds behind
speech; precision that was 1.0 by construction because unmatched nodes never
cost anything; a redirect check that passed the very misconception it was
supposed to catch. Fixed, re-run, ten-run regression unchanged. By 14:47 the
card for the take we filmed was rendered: 3 of 3 segments, F1 0.93, 0.91,
0.89, 6 of 6 beats. Five or six measure-fix-measure rounds in about three
hours, each twenty minutes to an hour.

<!-- ACCURACY: timestamps and defects from the live-diagrammer commit log —
     c23a2e6 11:33 (pre-flight 10/10), 215777c 12:40 (dismiss scope fix),
     1c259bf 13:26 (three grading defects), ec935f8 13:38 (second scope fix),
     81c8cb2 14:28 (three more takes graded), db3f3f8 14:47 (screen card).
     "Four defects in the grader" in SUBMISSION.md = these three + 46596d1
     (hyphen/slash matching). Say three or four consistently, not both. -->

The concrete one: the dismiss was discarding the whole board instead of the
tangent. On the ten-run regression, recovery after the dismiss passed in 0 of
10 runs. After the scope fix, 10 of 10. That is the change I can point at, and
the harness is what found it — not by eye, because in a demo you cannot
eyeball "the board came back correctly," you can only eyeball "a board came
back."

And here is the part that made me rethink what the library is. Ten replays
times four segment checks read as forty trials, pass rate 75%, interval
[60%, 86%]. But the four checks inside one run share a single model sample, so
the honest unit was the run, not the check, and over runs the same data read
0 of 10, [0%, 28%]. The headline number — the verdict, the thing I built gonogo
to produce — hid a check that failed in every single run inside a 75% average.
The per-check rows were where the truth was. So gonogo wasn't a report. It was
a feedback channel that happened to be formatted as a report, and its most
useful output that day was the log lines, because something was reading them
and acting.

<!-- ACCURACY: proposal §2 verbatim numbers. This became gonogo 0.2.0
     (Case.group, group_rule="all", n_groups / unit) on Sep 15 — say so if
     you want the "the library learned from being used" beat; CHANGELOG has
     the exact wording. -->

## What this was and what it wasn't

I want to be precise, because there's a nearby claim I'm not making.

What it wasn't: a system improving itself. Every change went through a coding
agent a human started, pointed at a codebase a human chose, with humans in the
room watching the diagram and deciding what mattered. The harness found it, a
person decided, the harness confirmed. Nothing was autonomous and nothing
optimized itself.

What it was: an evaluation signal wired into the same room as the thing being
evaluated, closely enough that the gap between "measure" and "change" got
short — tens of minutes, not days. That's a different claim and a smaller one,
and it's the one I can defend.

Did anything actually get better? On the committed regression set, yes, for
the specific thing each fix targeted — recovery went from 0 of 10 to 10 of 10,
and the grader stopped passing junk. On the benchmark that doesn't know about
our storyboard, the 35 AMI segments, the number is 57%, and it was measured
the night before. [JAMES: confirm — I believe it was NOT re-run after
Saturday's changes. If so, say it plainly: "I have no idea yet whether
Saturday made the agent better at meetings it wasn't rehearsed on." That
sentence protects everything else in this post.]

## Why I think this generalizes, cautiously

The reason this feels worth writing down is that the industry is building
toward fleets of agents doing real work, and the binding constraint on that is
not capability, it's knowing which ones to trust and noticing fast when one
stops being trustworthy. A verdict you read at the end of a pilot is the wrong
shape for that problem. A telemetry stream something can act on is closer to
the right shape.

What I don't know is whether the short loop is actually good. A fast feedback
channel between an evaluator and a code-writing agent is also a fast path to
overfitting the evaluator — you can make the score go up by making the grader
easier to please, and neither the agent nor I would necessarily notice from
inside the loop. Every grader fix on Saturday made the grader stricter, which
is reassuring, and is also exactly what I would say if it weren't. [JAMES: end
open, your own version — the thing you genuinely haven't worked out. Don't
resolve it.]
