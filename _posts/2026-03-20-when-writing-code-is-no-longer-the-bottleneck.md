---
layout: post
title: "When Writing Code Is No Longer the Bottleneck"
date: 2026-03-20
---

AI coding tools have fundamentally changed how fast we ship. A feature that used to take three days can now produce a ready-to-review PR in the time it takes to grab a coffee.

But once the speed problem is solved, a new one surfaces — the bottleneck has shifted.

## The risk density hasn't dropped, but the time window has compressed

The probability of introducing bugs doesn't decrease just because AI is writing the code. If anything, the risks are more subtle — AI output often looks correct, but lacks genuine understanding of the broader system.

Before, a three-day feature gave you three days to catch problems. Now, with a PR landing in under an hour, all that risk is compressed into a much shorter window. The importance of code review is going up, not down.

## It's not easy to have the full context for AI

This is the core issue. No matter how capable the model is, it can't automatically know everything your team knows.

Different people use different models. Some use Claude, some GPT, some a locally hosted model. Each one reasons differently and produces different outputs for the same task.

Even with the same model, prompts differ. What you tell the AI depends on how you frame the question, the history of your conversation, and your own judgment. Two engineers giving the same model the same task will likely get two meaningfully different implementations.

Even with the same prompt, LLM output is inherently non-deterministic. That's a fundamental property of these models, not a bug. Ask the same question today and tomorrow, and you may get subtly different code.

Most importantly: unless you explicitly tell it, the AI has no idea what was discussed in your team chat. It doesn't know you decided last Friday to avoid a certain point. It doesn't know the field naming convention was agreed on in Slack. It doesn't know about the legacy issue that makes a whole class of solutions unworkable. That implicit context is invisible to AI by default.

## Human laziness is its own risk

AI output carries a natural sense of authority — clean formatting, clear comments, logic that seems airtight. That fluency makes it easy for reviewers to unconsciously lower their guard, shifting from genuine scrutiny to a quick skim.

This is a real risk. The more confidently AI writes, the easier it is for humans to stop paying attention. And the cost of that inattention usually doesn't show up until something breaks in production.

## Review itself needs to evolve

The old review checklist: is the logic correct? Does it follow our conventions? Any obvious bugs?

Now there's an additional question worth asking: "Did the author — human or AI — actually understand the full context here?"

That means reviewers need to actively supply the unwritten knowledge: does this implementation reflect last week's architecture decision? Is this approach consistent with how other parts of the codebase work? Is there a reasonable-looking assumption buried in this AI-generated code that doesn't actually hold for our system?

## The bottleneck shifted. It didn't disappear.

AI has made writing code easier. But it's also made getting the code right harder and more important. When everyone can produce more code every day, ensuring that code is consistent, correct, and grounded in shared understanding is where the real engineering challenge now lives.

Writing code is no longer the bottleneck. Understanding it, aligning on context, and making the right judgment calls — that's where the work is now.
