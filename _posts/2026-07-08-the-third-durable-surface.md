---
title: "The Third Durable Surface"
date: "2026-07-08"
categories: 
  - "distributed-systems"
  - "ai-agents"
  - "durable-execution"
excerpt: A crash mid-turn brings the agent back with no memory that it ever spoke. That amnesia is not a bug. It is the symptom that tells retryable runtimes apart from continuable ones, and the difference is where you paid for durability.
---

"Find me the best waffle maker under $100." The agent starts working. It surfaces
a $75 model, weighs it against a $90 one with a better plate, and is composing its
recommendation when a deploy reschedules the container out from under it. The
runtime does what runtimes do: it redelivers the input and runs the turn again.
The agent comes back with no memory that it ever spoke. It either re-sends
everything it already said, or it reasons afresh and lands on a different answer.

The crash did not lose a paragraph. It gave the agent a personality disorder.

As I argued in [a previous post](https://www.martinkysel.com/the-missing-exactly-once-layer-under-ai-agents/),
a crash mid-turn is the normal operating condition for an agent that acts, not an
edge case. Vendors have answered by selling "durable" and "stateful" agent
runtimes. Most of them made the agent retryable. Almost none made it continuable.
The waffle-maker amnesia is the symptom that tells the two apart.

- **Retryable**: you can re-run the whole turn from the beginning and get a
  correct result.
- **Continuable**: you can resume the turn from where it died.

The relationship is exact: continuable = retryable + durable intermediate state.
The only added ingredient is durability of the in-flight computation. Everything
below is about the price of that one ingredient.

## Where you paid for durability

The usual way to frame this trade is latency versus correctness. That is the
wrong axis. The right one is *where in the computation you paid for durability*:
at the ends, or in the middle.

**Retryable needs durability only at the ends.** Two things must survive a crash:
the input, and the effect. A durable input and an idempotent sink. Everything
between them can be as unreliable as you like. The container can lose all its RAM
and vanish, because a retryable turn is all-or-nothing: nothing in the middle was
ever externalised, so throwing the half-finished run away and restarting from the
durable input is not a compromise, it is correct. This is why you can bolt
retryability onto any disposable executor: put a queue in front, make the
consumer idempotent, treat workers as cattle. The cost is paid in latency and
pacing. You cannot stream a result you might have to retract, so you buffer
everything until the turn commits and release it at once.

**Continuable needs durability in the middle**, a journal of in-flight progress
written before the crash, not after. You cannot throw the run away, because you
have already externalised part of it: a message the user has seen, the first half
of a recommendation. To resume instead of restart, you must have written down
what you did as you did it. There is no queue you can put in front to get this for
free. You change what the executor records while it runs.

## Why the cheap trick does not work on an LLM

Durable execution is not new. Temporal, Restate, DBOS, and Cloudflare Workflows
all make the middle of a computation durable, and they do it with the same trick:
journaled replay. The workflow records the result of each step as it completes.
After a crash it re-runs the workflow code from the top, but every step that
already has a journal entry returns its recorded result instead of executing
again. Replay re-derives the program's state cheaply, because the code between
steps is deterministic: given the same journal, it walks the same path.

An LLM is not deterministic. You cannot replay the prompt and trust it to
re-derive the same generation, and that non-determinism is the whole reason you
reached for a model in the first place. So the cheap half of the trick, replaying
the recorded steps and re-executing the deterministic glue between them, breaks the
moment the glue is a model call.

The fix is one the durable-execution world already knows: model each LLM call as
an **activity**. An activity is journaled once when it completes and never
re-executed on replay; its recorded output is authoritative. For a deterministic
workflow the journal is cheap, because most of the state can be re-derived and
only a handful of external results need storing. For an LLM you have no choice but
to store the actual generations, every token the model produced, because there
is no code path that re-derives them. The black-box, non-replayable executor is
precisely what makes continuability cost real durability instead of cheap replay.

And that journal, the record of every decision the model committed to during the
turn, is exactly the memory the amnesiac agent was missing. The waffle-maker bug
is not a separate failure. It is what ends-only durability looks like from the
user's chair: the model's accumulated decisions lived in container RAM, the
container went away, and there was nothing on disk to continue from, only the
input to redo.

## It is not the delivery guarantee

One objection worth heading off. Both models deliver effects the same way:
at-least-once delivery into an idempotent sink, which nets out to
effectively-once. The delivery guarantee is not what separates them.

The difference is *when* you are allowed to externalise an effect. Retryable
buffers every effect to the end of the turn and releases them together, so a crash
before the commit point erases a run that never touched the outside world.
Continuable externalises incrementally as the turn proceeds, which is what makes
streaming and mid-turn actions possible, and pays for that by journaling each
decision before it acts, so a crash leaves a record to resume from instead of a
void to re-guess.

## The question to ask any substrate

You do not shop for continuability as a checkbox on a framework's feature page. It
is a property of the storage sitting under the loop, so the only useful thing to
do when you evaluate an agentic LLM substrate is ask one question: when the model
commits a decision in the middle of a turn, does that decision land on storage
that outlives the container before the next crash? Put it more bluntly. Is my disk
continuable?

That question is unkind, because a continuable disk is one of the least pleasant
things an ops team has to operate. The journal sits on the hot path of every turn:
each decision is written before it acts, so the write latency is added to every
step the agent takes. To be worth trusting it has to be genuinely durable, with
POSIX fsync semantics, where a write that reports success is actually on the
medium and not in a buffer the crash eats. You want it colocated with the compute,
ideally a local mount, so a journal write is not a network round-trip bolted onto
every decision. And it still has to survive the node it is colocated with going
away.

Fast, POSIX-durable, colocated, and crash-surviving, all at once: that is
nightmare territory. Network-attached block storage gives you durability and a
detach latency you feel on every write. Local NVMe gives you speed and dies with
the instance. A distributed POSIX filesystem gives you all of it and a pager that
never sleeps. Persistent disks are the bane of every ops team for exactly this
reason, and continuability puts one on the critical path of your agent loop.

So the real recommendation is not "make your agents continuable." It is: decide
whether you are willing to run that storage. If you are not, you are choosing
retryable, and that is a legitimate choice with its own bill: buffer every effect
to the end of the turn, give up streaming and pacing, re-run from the input on
every crash. Neither option is free. Retryable pays in latency and pacing;
continuable pays in storage operations. You can have retryability on unreliable
execution. You cannot have continuability without durable execution.

## Two durable surfaces instead of three

When I first started building agents, I used a substrate that runs on unreliable disks. 
It throws RAM away on a whim and promises me nothing like a fast, colocated, crash-durable journal, which
is the one thing continuability cannot do without. So I did not get to choose
continuability. I built the retryable system on purpose, and spent the design
effort on making it need as few durable surfaces as possible: two, not three.

Here is the shape. During a turn the agent is not allowed to send anything. It can
only *propose* messages: accumulate the set of outbound messages it intends to
send. When the turn completes, that whole set is sealed into a Durable Sink in a
single transaction, and the same transaction advances the inbound message's
lifecycle out of `processing`. From then on the Durable Sink owns delivery: it
sends each message, and it marks the inbound message `replied` only once every
outbound message is confirmed out.

Count the durable surfaces. The inbound message that arrived is one, the durable
input. The Durable Sink that holds the sealed set and drives delivery is the
other. The middle, every decision the model made during the
turn, is never written down. It lives on local disk and dies with the container, and that
is fine: a crash before the seal strands the inbound message in `processing`,
where a sweep picks it up and re-runs the turn from the input. Nothing was
externalised, so re-running is correct. That is the retryable bargain paid in
full: two durable surfaces, an unreliable middle, and a re-run whenever the middle
is lost.

A continuable version would add the third durable surface, the journal in the
middle, and with it the fast, colocated, crash-durable disk I do not have. I
traded that away for the retryable tradeoffs: no streaming, buffer to the seal,
re-run on crash. Let us see how it does in the real world.