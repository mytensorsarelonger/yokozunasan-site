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
     Sep 15). Repo link only.

     !! PRE-EVENT DISCLOSURE. The eval layer was mostly built Sep 10–12 in
     cdiddy77/fullstackiest/live-diagrammer/report-card (judge + rubric,
     bronze/silver/gold tiers, the rating worksheet, frozen case sets, the
     deterministic demo card, the ten-run card, and the "unit is the run"
     fix at 00:48 Sep 12). Saturday's eval work was porting cases.ts into the
     submission repo, the take tooling, take_ref.py, and the grader fixes the
     live takes exposed. The public README's "What was built during the
     event" section is STILL the placeholder on origin/main — fill it before
     this post ships, and keep this post consistent with it. -->

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
instrument and it deserved measuring first. In the two days before the event I
had an LLM judge grading boards against the conversation, on segments of real
meetings from the AMI corpus, and three sets of labels to check it against on
the same cases: structural checks (orphan nodes, duplicate edges, rejected
ops), an unrelated model family, and two humans — Charles and me. Kappa against
the structural labels was 0.36, against the other model 0.42, against the
humans 0.09. And the two humans agreed with each other 40% of the time. So
there was no human validation to claim, and the card says exactly that.

Getting even that far took two false starts I'd rather write down than hide.
The first rubric graded the *edit* a segment made to the board. Most speech is
not structure, so most segments correctly change nothing, and 12 of 20 cases
became "no change, correct." Agreement with the judge hit 90% and kappa fell to
0.02. The eval measured nothing. Grading the board as it stands against
everything said so far fixed that, and it's also what a report card on a
diagram actually means. The second: the judge wasn't at temperature zero and
wasn't cached, so the labels the worksheet was built from and the kappa any
later run printed came from different draws of the same judge. And temperature
zero isn't deterministic anyway — 35 of 36 extractor calls identical across two
runs, one differing by an edge — so if each rater regenerated the boards before
rating them, we'd have been grading different diagrams and the inter-rater
number would have meant nothing. The case sets got frozen and committed.

<!-- ACCURACY: worksheet.md / rubric.py (12 of 20, agreement 90%, kappa 0.02);
     commit a424f1c (judge not at temp 0, re-sampling flipped one verdict);
     cases/README.md and dd8d2ab (35 of 36 calls identical; freeze the cases). -->

Then there was the number I built the card to show and couldn't. The
storyboard's third beat says gonogo runs a judge over cases and reports a pass
rate with an interval. It does. But with the fidelity judge, the card at the
end of the demo would have read DO NOT AUTOMATE, 33% [10%, 70%], right after
the audience watched the thing work — two of the four fails were a known
parking bug and two were 3-out-of-5 scores on boards that matched the target
Charles drew by hand. An honest number from the wrong instrument.

<!-- ACCURACY: commit caaccdd, Sep 11 22:54. -->

<!-- ACCURACY: bronze κ 0.36 / silver κ 0.42 / gold κ 0.09, inter-rater 0.40 —
     gonogo docs/proposals §1 and the card footer ("2 raters, 10 cases"). The
     proposal's table says 20 cases; the gold files have 10 lines each. Use 10
     if you quote a count. -->

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

The concrete one had actually started the night before. At around eleven
Friday night I ran the storyboard recording through the pipeline ten times and
graded four segment checks per run. Forty trials, 75% pass, interval
[60%, 86%]. A B. But the four checks inside one run share a single model
sample, so the honest unit was the run, not the check, and over runs the same
data read 0 of 10, [0%, 28%]: recovery after the dismiss — the board coming
back intact after the tangent was parked — failed in every single run, and the
forty-trial average hid it. The headline number, the verdict, the thing I built
gonogo to produce, was the least informative thing on the card. The per-check
rows were where the truth was. I changed the card at 00:48 to put the interval
over runs, went to bed, and the first thing Saturday's pre-flight showed, on
the pipeline Charles had rebuilt that morning, was 10 of 10. That is the
change I can point at, and the harness is what found it — not by eye, because
in a demo you cannot eyeball "the board came back correctly," you can only
eyeball "a board came back."

So gonogo wasn't a report. It was a feedback channel that happened to be
formatted as a report, and its most useful output that weekend was the rows
and the log lines, because something was reading them and acting.

<!-- ACCURACY: 0 of 10 = both prep-night ten-run sets (growth prompt and main
     prompt), re-run Sep 15 from cases/demo01-10runs*.jsonl: recovery 0/10,
     architecture-survives 0/10 and 1/10. Commit 21db778 00:48 Sep 12 ("the
     independent unit is the run, not the check... tonight this reads 0 of 10
     runs"). 10 of 10 = c23a2e6 11:33 Sep 12 on the submission server. The
     12:40 / 13:38 dismiss-SCOPE fixes are a later, different refinement (a
     stray edge splitting the tangent) — do not credit them with 0→10.
     JAMES: confirm what actually fixed parking between 00:48 and 11:33.
     The realization shipped as gonogo 0.2.0 (Case.group, group_rule="all")
     on Sep 15 — CHANGELOG has the wording if you want that beat. -->

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
the specific thing each fix targeted — recovery went from 0 of 10 Friday night
to 10 of 10 Saturday morning, and the grader stopped passing junk. On the benchmark that doesn't know about
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
