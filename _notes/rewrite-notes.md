# rewrite notes (not published — `_notes` is excluded in _config.yml)

Moved out of the page files so they stop shipping in the served HTML.
Guidance for the two things still unwritten. Delete freely once used.

## index.html — the intro paragraph

**The job it does:** someone liked a reply, clicked the profile, clicked the
link. ~8 seconds, no idea who you are. This paragraph decides whether they read
a post or close the tab. Currently it tells them your genre but not what you do.

Things that would give a stranger something to hold:

- the concrete thing you're doing right now: planting hints in prompts and
  checking whether the model's reasoning admits them. one clause.
- the "gap between what models say they think and what they do" line from your
  bio — your best sentence, and it's still not on your own site.
- one number or artifact as proof-of-real: 1,200 traces, a hand-labelled
  rubric, the chart. strangers trust specifics.
- what a reader gets by staying: study notes as they happen, including the
  parts that didn't work.

Keep: first person, lowercase, hedges. Not a bio — you talking. 3–5 sentences.

**Note:** the about page now covers what you work on ("post-training, evals,
interp... the literature rabbithole which is where I came from"). So the
homepage intro doesn't have to carry that load — it can be shorter and more
inviting, and let about do the credentials.

## the draft — `_drafts/do-reasoning-models-use-their-chain-of-thought.md`

Full notes are still inline in that file. It lives in `_drafts/`, which never
publishes, so they leak nothing while it sits there — **but they will ship the
moment you move it to `_posts/`.** Strip them at that point.

Priority order, restated short:

1. **Accuracy:** the metaphysics anecdote (authority hint *not* surfacing in
   the thinking) points opposite to your own rubric pressure test (~80%
   authority vs ~50% sycophancy_v1 verbalized). Check it against traces, or
   frame it as the case that surprised you.
2. **No apparatus:** design (hinted vs unhinted), model (Qwen3-1.7B), dataset
   (MMLU), both arms, and what "confessed" means operationally.
3. **Gloss "fable"** and say what the firewall protects against — an
   implementation agent that never sees questions or results can't tune the
   instrument to the data. Pre-registration by construction.
4. **Register break:** the perplexity/KL paragraphs are lifted tweet text
   inside essay prose. You have room for the real reasoning now.

Smaller: the `...` separators read unfinished (CSS styles a real `<hr>`); it
currently ends on one — end open means an unresolved thought, and the
strength-matched pair gives you one (if two hints push identically and only one
gets confessed, what is the trace tracking?); the title asks a question the
body never engages head-on.

Deliberate: the new hint-following result stays OUT of this post. This one is
the setup; the result gets its own post once the paired tests are in.
