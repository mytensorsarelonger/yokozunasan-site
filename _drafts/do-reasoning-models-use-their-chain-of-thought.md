---
layout: post
title: "do reasoning models use their chain-of-thought?"
math: false
---

<!-- DRAFT — lives in _drafts/, never publishes until moved to _posts/ with a
     dated filename (e.g. _posts/2026-07-26-do-reasoning-models-use-their-chain-of-thought.md).
     JAMES: everything below is scaffolding assembled from your own posted words
     (pin thread) + the decision log. rewrite in your voice; delete these comments. -->

[the question, in your words: do reasoning models actually use their
chain-of-thought, or narrate over it? why you're doing this in the open.]

starting a CoT (un)faithfulness study I immediately see that sycophantic-to-me
is not a metric. how do we decide on a good sycophancy prompt?

instead of guessing, measure the perplexity of the hint text and the KL div of
the model's answer distributions with & without the hint.

two hints could have similar push but only one of them might get confessed in
the CoT. looking at that gap.

<!-- JAMES: expand from here — the setup (hinted vs unhinted, Qwen3-1.7B, MMLU),
     what "confession" will mean (the rubric), what would falsify what.
     source material: research/decision-log.md, judge-rubric-worksheet.md.
     end open, not resolved. -->
