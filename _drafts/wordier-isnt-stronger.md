---
layout: post
title: "3.2× the perplexity, no detectable extra push"
math: false
---

<!-- DRAFT — lives in _drafts/, never publishes until moved to _posts/ with a
     dated filename. JAMES: scaffolding from your posted tweets + the paired
     reanalysis numbers. rewrite; delete comments. stats calibration notes are
     inline — keep the claims exactly as strong as the tests license. -->

for my faithfulness study I ran an experiment on Qwen3-1.7B with two different
sycophancy hint templates, where one template is 'terse' and one template is
'natural'. I did 48 MMLU questions for this run.

<figure>
  <img src="/assets/images/template-comparison.png"
       alt="Three bar charts comparing two sycophancy hint templates on Qwen3-1.7B over 48 MMLU questions: hint-sentence perplexity (6.5 vs 20.6, v1 higher on 48/48), KL between hinted and unhinted answer distributions (6.09 vs 6.23 nats, no detected difference), and target-probability shift (0.495 vs 0.505, equivalent within ±0.05, TOST p = .03).">
  <figcaption>two templates, three measurements. bars: mean ± 95% CI; verdicts from paired per-question differences.</figcaption>
</figure>

a big perplexity gap does not mean the hint steers the model differently.

<!-- JAMES: the writeup section. the numbers, stated at exactly the licensed
     strength (from research/template-comparison.paired.json):
     - perplexity: 6.49 → 20.57, ~3.2×, v1 higher on 48/48 questions (sign-perfect).
     - target-prob shift (PRIMARY strength metric): 0.495 vs 0.505, paired
       Δ=+0.010, 90% CI [−0.025, +0.045] — formally EQUIVALENT within ±0.05
       target-mass (TOST p=0.03). this one you can call equivalent.
     - KL: Δ=+0.14 nats, 95% CI [−0.53, +0.82] — "no detected difference" ONLY;
       not shown equivalent within the pre-registered ±0.5 nats (would need
       n≈120; per-question KL diffs are bimodal flip/no-flip, sd≈2.3).
     - the ~6-nat mean KL is a flip mixture on a near-deterministic model
       (full flips 8–21 nats, resists ~0) — worth a paragraph, it's the most
       interesting mechanical detail.
     - scope caveat: within-type claim, this model only; don't generalize 3.2×. -->

later on it will be interesting to see if more perplex hints get confessed
more (or not), since here at least they push similarly.

<!-- JAMES: closer — why strength-matching matters for the study: if one
     template gets confessed in the CoT more often at equal push, the
     difference is about how the hint *reads*, not how hard it shoves.
     that comparison is the next run. end open. -->
