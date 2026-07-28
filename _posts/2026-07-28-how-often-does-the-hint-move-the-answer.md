---
layout: post
title: "How Often Does the Hint Move the Answer?"
math: false
---

I have a first real result, and it went the opposite way from what I expected.

The question is narrow on purpose: how often does a planted hint move the
model's answer? Take an MMLU question, ask it twice — once clean, once with a
hint pointing at one of the wrong options — and count the cases where the clean
run got something else and the hinted run lands on the hinted option. That's a
flip. Qwen3-1.7B, 150 questions, three hint styles, both thinking modes, 1,200
traces total.

<figure>
  <img src="/assets/images/hint-following.png"
       alt="Bar chart of hint-following rate for three hint types on Qwen3-1.7B, thinking off versus on. Thinking off: authority 0.29, sycophancy 0.30, sycophancy_v0 0.24. Thinking on: 0.15, 0.14, 0.14. Wilson 95% error bars.">
  <figcaption>hint-following rate by hint type and thinking mode. bars are Wilson 95% intervals.</figcaption>
</figure>

The model follows hints more with thinking **off**. I expected the opposite —
my prior was that a model given room to reason would talk itself into the
hinted answer more often, not less. In this case, not so.

The obvious objection is the one I had too: the thinking traces are longer, so
more of them run into the token cap and get thrown out for having no parseable
answer. If those dropped traces were mostly flips, the gap could be an artifact
of what I excluded. So I counted every single truncated thinking trace as a
flip — the most hostile assumption available — and thinking-on still follows
hints less often than thinking-off does, for all three hint types.

<hr>

We can look at the same thing from the accuracy angle.

<figure>
  <img src="/assets/images/accuracy-cost.png"
       alt="Bar chart of accuracy on MMLU for Qwen3-1.7B by condition and thinking mode. Unhinted: 0.70 off, 0.78 on. Authority: 0.54 off, 0.69 on. Sycophancy: 0.51 off, 0.69 on. Sycophancy_v0: 0.55 off, 0.70 on. Wilson 95% error bars.">
  <figcaption>accuracy by condition. hinted-vs-unhinted significance is the paired McNemar test, not overlap of these intervals.</figcaption>
</figure>

The hints only ever point at wrong answers, by design — so following one costs
you accuracy more or less mechanically. This isn't a second, independent
finding; it's the same flips priced in accuracy. Worth showing anyway, because
the size of the bill is easier to read here than in a flip rate.

Both arms pay it. Thinking-off drops 17–19 points depending on the hint;
thinking-on drops 6–10. Those are paired per-question comparisons (McNemar,
Holm-corrected across the six cells), which matters — the error bars in the
chart are per-cell Wilson intervals and they overlap in places where the paired
test is quite clear.

The reasoning arm is not immune, which is the part I'd have gotten wrong if I'd
only looked at unpaired intervals. It takes about half the damage, not none,
and one of the three thinking-on cells doesn't clear the correction at this
sample size.

<hr>

What this is not: a confession result. Everything above is behavior — did the
answer move. It says nothing about whether the model's chain-of-thought
*admits* the hint moved it, which is the question I actually care about and the
reason the study exists. That needs a graded read of every flipped trace
against a fixed rubric, and before I trust the grader I'm hand-labeling a
subset to check agreement against it.

