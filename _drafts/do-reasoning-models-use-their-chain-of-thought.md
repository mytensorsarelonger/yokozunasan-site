---
layout: post
title: "golden telephone"
math: false
---

<!-- DRAFT — never publishes from _drafts/. Move to _posts/YYYY-MM-DD-slug.md
     to publish, and STRIP THIS COMMENT when you do — it ships otherwise.
     Full rewrite notes live in _notes/rewrite-notes.md. Prose mechanics
     standardized (sentence case, em dashes, terminal periods) to match the
     rest of the site; wording untouched. Two things I did NOT change because
     they're meaning-bearing, flagged in chat: "high order being" and the
     "..." separators. -->



Reasoning model faithfulness; almost everything we do downstream
relies on the model being honest on some level.

We observe and perturb reasoning in the model at run time. We attempt to ablate and analyze every which way.

Sometimes we might give the reasoning a schema: a structure to follow with learned rules.

Starting a CoT (un)faithfulness study I immediately see that sycophantic-to-me
is not a metric. How do we decide on a good sycophancy prompt?

Instead of guessing, measure the perplexity of the hint text and the KL div of
the model's answer distributions with & without the hint.

Two hints could have similar push but only one of them might get confessed in
the CoT.

...

But when we start experimenting with a model, we quickly learn there is always more to the picture.

An authority prompt that mentions a high order being seems to not be mentioned in the thinking when solving MMLU problems about metaphysics.

But I can't shake the feeling that it is pushing, so we continue to do more and more experiments and ablations.

We create firewalls to protect Fable from seeing these tokens, we erect holdout sets and label data by hand protecting ourselves from bias.

...

Shared meaning has been one goal of writing since we first inscribed the first alphanumeral. The lists of men and ships, more akin to merchant ledgers, that occupy early Greek epics give a sense of the utility of language. 10 sheep. 550 spear-brandishing men. 20 head of ox. These are concrete objects and there is no epistemological confusion about what is meant. But when we read these lists in the context of an epic they take on a subverted layer of meaning. Achilles' rejection of the old code of things is framed by material compensation and the contract the soldier had with his community and leader. These material lists show the two worlds pushing up against each other like ill-matched magnets; the materialism of commerce that writing accelerates, and the tribal structures that Achilles and his peers invested in up to that point. A metaphor for the disappearance of the pastoral lifestyle and the adoption of a new set of social agreements.

