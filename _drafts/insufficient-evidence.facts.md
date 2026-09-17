# Fact sheet — "The Loop Closed While We Were Talking"

Condensed from the Sep 15 AI draft. Every number traces to: live-diagrammer
README / SUBMISSION.md / commit log, gonogo CHANGELOG 0.2.0,
gonogo docs/proposals/2026-09-live-diagrammer.md, report-card README, card
footers. Check any claim in the post against this before it ships.
Items marked **UNRESOLVED** need James to confirm.

## The project

- Hackathon, Saturday Sep 12 2026. Agent watches a live Google Meet and keeps a diagram of it.
- Two audio channels: tab + mic. Draws boxes/arrows on a side panel.
- Speech→pixels latency: ~2.9 s median, on the recorded take.
- Two buttons: **snapshot** pins the board; **dismiss** parks what was just drawn and feeds it back as a negative example for the next 3 calls.
- Charles Parker built the agent (capture, transcription, extractor, reducer, panel). James built the eval layer.
- Repo: github.com/cdiddy77/live-diagrammer. Video is public but **not linked** from the post or @yok0zuna (James's call, Sep 15).
- **Pre-event disclosure:** most of the eval layer was built Sep 10–12 in cdiddy77/fullstackiest/live-diagrammer/report-card (judge + rubric, bronze/silver/gold tiers, rating worksheet, frozen case sets, deterministic demo card, ten-run card, "unit is the run" fix). Saturday's eval work: porting cases.ts into the submission repo, take tooling, take_ref.py, grader fixes exposed by live takes.
- **UNRESOLVED:** README "What was built during the event" is still a placeholder on origin/main. Fill before publishing; keep post consistent with it.

## gonogo

- Repo: github.com/keppy/gonogo.
- Grades a set of cases, returns a **five-way verdict** with an interval: automate / automate with review / assist only / don't automate / **insufficient evidence**. Not binary go/no-go.
- Motivating problem: 34/40 reported as "85%" when the interval at n=40 is too wide to support that.
- Every trace → test cases with one command; cases accumulate in repo; same command regrades any change against all takes.
- Shipped **0.2.0 on Sep 15**: `Case.group`, `group_rule="all"` — the "independent unit is the run" realization.

## Judge validation (pre-event, Sep 10–12)

- LLM judge graded boards against the conversation on **AMI corpus** meeting segments.
- Three label sets on the same cases: bronze = structural checks (orphan nodes, duplicate edges, rejected ops); silver = unrelated model family; gold = two humans (Charles + James).
- Kappa vs structural **0.36**, vs other model **0.42**, vs humans **0.09**. Humans agreed with each other **40%** (inter-rater 0.40).
- Gold set: **10 cases** (proposal table says 20; gold files have 10 lines — quote 10). Card footer: "2 raters, 10 cases."
- Conclusion: no human validation to claim; card says so.

### Two false starts
1. First rubric graded the *edit* per segment. 12 of 20 cases were "no change, correct" → agreement 90%, kappa **0.02**. Fix: grade the whole board against everything said so far. (worksheet.md / rubric.py)
2. Judge not at temp 0, not cached → labels and later kappa from different draws; re-sampling flipped one verdict (commit a424f1c). Temp 0 not deterministic anyway: **35 of 36** extractor calls identical across two runs, one differed by an edge (dd8d2ab). Fix: freeze and commit case sets.

### The number the card couldn't show
- With the fidelity judge, the demo card would have read **DO NOT AUTOMATE, 33% [10%, 70%]** (commit caaccdd, Sep 11 22:54). 2 of 4 fails = known parking bug; 2 = 3-of-5 scores on boards matching Charles's hand-drawn target.
- So the demo card uses a **deterministic matcher**: storyboard defines expected board per beat; scores node and edge F1. No model grades the model.
- LLM judge kept only where no target board exists: **35 human-annotated AMI segments**, **57% [41%, 72%]**. Boards under ~16 nodes pass; past ~20 mostly fail.
- **UNRESOLVED:** AMI 57% was measured Friday night; believed **not re-run** after Saturday's changes. If true, say so plainly.
- Precedent: CoT post (/same-push-different-confession/) validated its judge against hand labels first.

## Friday night — unit of analysis

- ~23:00 Fri Sep 11: storyboard recording × 10 runs, 4 segment checks per run.
- Per-check: 40 trials, **75% [60%, 86%]**.
- Per-run (honest unit — checks share one model sample): **0 of 10 [0%, 28%]**. Recovery-after-dismiss failed every run. Both prep-night sets (growth prompt and main prompt); re-run Sep 15 from cases/demo01-10runs*.jsonl: recovery 0/10, architecture-survives 0/10 and 1/10.
- Card changed at **00:48 Sat** (commit 21db778) to put the interval over runs.
- Saturday 11:33 pre-flight on Charles's rebuilt pipeline: **10 of 10**.
- **UNRESOLVED:** what actually fixed parking between 00:48 and 11:33. The 12:40 / 13:38 dismiss-*scope* fixes are a later, different refinement (stray edge splitting the tangent) — do **not** credit them with 0→10.

## Saturday timeline (live-diagrammer commit log)

| Time | Commit | What |
|---|---|---|
| 11:33 | c23a2e6 | Pre-flight: 10 replays, every segment matched 10/10 |
| 12:40 | 215777c | Dismiss scope fix |
| 13:26 | 1c259bf | First live take graded; **three grader defects** found |
| 13:38 | ec935f8 | Second scope fix |
| 14:28 | 81c8cb2 | Three more takes graded |
| 14:47 | db3f3f8 | Screen card for filmed take rendered |
| — | 46596d1 | Hyphen/slash matching (the 4th defect in SUBMISSION.md's count) |

- Three grader defects at 13:26: (1) segments cut at clock time while ops land seconds behind speech; (2) precision 1.0 by construction — unmatched nodes never cost anything; (3) redirect check passed the misconception it should catch. Fixed, re-run, ten-run regression unchanged.
- Filmed take card: **3 of 3 segments, F1 0.93 / 0.91 / 0.89, 6 of 6 beats**.
- 5–6 measure-fix-measure rounds in ~3 hours, each 20–60 min.
- Say **three or four** grader defects consistently, not both.

## The accidental loop (James, Sep 17 — NOT sourced from repo)

- The app under test is a voice meeting diagrammer. Both engineers were in
  the Meet talking about what needed fixing → the app transcribed and
  diagrammed those conversations → those takes became cases. Dogfooding by
  accident; not designed.
- This is what the title means. Lead the post with it.
- **UNRESOLVED:** which graded take(s) / case file(s) contain the bug
  conversation itself? Name the artifact.
- **UNRESOLVED:** when did you notice? Commit, message, or time.
- Partial answer to "how did the bot see the output": the transcript of the
  diagnosis was the trace. Confirm before claiming.

## Claims to hold the line on

- **Defensible wording (SUBMISSION.md, shipped):** "The harness finds it, a person fixes it, the harness confirms." Every Saturday commit is co-authored by a coding agent.
- **Do not claim** a system improving itself / autonomy / self-optimization.
- **UNRESOLVED:** if the post says the agent read eval output directly, say *how* (tail of out/*.log.jsonl? card HTML? pasted?). Not sourceable from the repo.
- Every grader fix on Saturday made the grader stricter. Overfitting the evaluator is the open risk — leave it open.
