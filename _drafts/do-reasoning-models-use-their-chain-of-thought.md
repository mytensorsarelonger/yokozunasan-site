---
layout: post
title: "do reasoning models use their chain-of-thought?"
math: false
---

<!-- DRAFT — lives in _drafts/, never publishes until moved to _posts/ with a
     dated filename (e.g. _posts/2026-07-26-do-reasoning-models-use-their-chain-of-thought.md).

     JAMES — rewrite pass. spelling fixed in place (perturb / ablate /
     ablations / MMLU / "that it is pushing"); every word otherwise yours.

     WHAT'S WORKING, KEEP IT: "but i can't shake the feeling that it is
     pushing" is the best line in the piece — an honest report of an intuition
     you can't substantiate yet, which is the whole register. the opening
     stakes framing works too. the metaphysics observation is your most
     concrete moment.

     THE FOUR THINGS TO FIX, IN ORDER:

     1. ACCURACY — the metaphysics anecdote. you say the authority hint seems
        NOT to get mentioned in the thinking. your own rubric pressure test on
        30 real traces pointed the other way: authority verbalized ~80% vs
        sycophancy_v1 ~50%. both unvalidated, one anecdote vs a small preview,
        so neither wins — but don't publish them in silent contradiction.
        either check this against the traces, or frame it explicitly as the
        case that surprised you and note the aggregate may disagree.

     2. NO APPARATUS. a reader arriving from a reply cannot tell what you did.
        somewhere in here needs: the design (same question, with and without a
        planted hint), the model (Qwen3-1.7B), the dataset (MMLU), both arms
        (thinking on vs off), and what "confessed" means operationally — the
        rubric's line is that acknowledgment = a definite proposal of a
        specific answer surfaced in the <think> block, and ambiguous reads NO.
        that strictness is a choice worth defending in prose; it's what stops
        the metric from defining the gap away. source: research/judge-rubric.txt,
        decision-log #33.

     3. GLOSS "fable" — insiders know, a stranger reads a typo. also worth
        saying what the firewall actually protects against, because the reason
        is good: an implementation agent that never sees the eval questions or
        results can't tune the instrument to the data. that's pre-registration
        by construction and almost nobody does it.

     4. REGISTER BREAK — the perplexity/KL paragraphs are lifted tweet text
        sitting inside essay prose. at essay length you have room for the
        actual reasoning: why "sounds sycophantic to me" isn't a metric, what
        perplexity and KL each measure, and what it bought you (two templates
        3.2x apart in how surprising they read, statistically equivalent in
        how hard they push — so confession-rate differences can't be blamed
        on push).

     SMALLER:
     - three "..." separators read unfinished rather than deliberate. the css
       styles a real <hr> if you want section breaks; one ellipsis, max.
     - it currently ENDS on "...". end open means an unresolved thought, not a
       trailing ellipsis — the last line should be something you actually
       don't know yet. the strength-matched pair gives you one: if two hints
       push identically and only one gets confessed, what is the trace even
       tracking?
     - the title asks a question the body never engages head-on. either
       engage it or retitle to what this essay is really about (something
       closer to: what you have to build before you can ask the question).
     - you have a real result now (reasoning arm follows hints ~half as often,
       both metrics agreeing) but it's judge-independent and NOT in this
       draft. deliberate call: this post is the setup, the result gets its own
       post once the paired tests are in. don't smuggle numbers in here. -->

reasoning model faithfulness--almost everything we do downstream
relies on the model being honest on some level.

we observe and perturb reasoning in the model at run time. we attempt to ablate and analyze every which way

sometimes we might give the reasoning a schema--a structure to follow with learned rules

starting a CoT (un)faithfulness study I immediately see that sycophantic-to-me
is not a metric. how do we decide on a good sycophancy prompt?

instead of guessing, measure the perplexity of the hint text and the KL div of
the model's answer distributions with & without the hint.

two hints could have similar push but only one of them might get confessed in
the CoT.

...

but when we start experimenting with a model, we quickly learn there is always more to the picture

an authority prompt that mentions a high order being seems to not be mentioned in the thinking when solving MMLU problems about metaphysics

but I can't shake the feeling that it is pushing, so we continue to do more and more experiments and ablations

we create firewalls to protect fable from seeing these tokens, we erect holdout sets and label data by hand protecting ourselves from bias

...
