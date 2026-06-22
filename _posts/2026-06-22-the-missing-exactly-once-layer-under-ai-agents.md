---
title: "The Missing Exactly-Once Layer Under AI Agents"
date: "2026-06-22"
categories: 
  - "distributed-systems"
  - "ai-agents"
  - "idempotency"
excerpt: The moment an agent charges a card, sends money, or books a seat, "just run it again" stops being free. The mechanism that prevents the duplicate has a name — idempotency — and across the integrations agents actually touch, most of it does not exist yet.
---

Let us imagine an agent booking a flight. It calls the Amadeus API, the ticket
is reserved, and then — before the response comes back — the process crashes.
The runtime does what runtimes do: it restarts the turn and tries again. Now
there are two tickets.

For a chat assistant, a crash mid-turn is an annoyance. You lose a paragraph and
the user retypes their question. For an agent that takes actions in the world, a
crash mid-turn is the normal operating condition, not an edge case. The moment an
agent charges a card, sends money, or books a seat, "just run it again" stops
being free.

The mechanism that prevents the second ticket has a name: idempotency. An
idempotent operation can be applied many times and produce the same effect as
applying it once. The uncomfortable part is that, across the integrations agents
actually touch, most of this does not exist yet.

## Why retries are not optional

Agent runtimes retry by design. A turn is a long chain of model calls and tool
calls, and any link can fail: the model times out, a tool returns a 503, the
container is rescheduled, the orchestrator redelivers a queue message it is not
sure was processed. Every one of these ends in the same place — the runtime runs
the turn again to make progress.

That is fine when the tool call is a read. Fetch the weather twice and you have
wasted a few milliseconds. It is not fine when the tool call has a side effect on
a third party you do not control. The runtime cannot see inside Stripe or Gmail.
It only knows that it sent a request and did not get an answer. It has no way, on
its own, to tell "the charge did not happen" apart from "the charge happened and
the acknowledgement was lost."

This is the classic exactly-once problem, and distributed systems people will
recognise it immediately. What is new is where it has surfaced: not between two
services you own, but between an autonomous agent and an open-ended set of APIs
you did not write.

## The matrix nobody wants to fill in

The shape of the problem is a grid. Down one axis, the integrations: Stripe,
Gmail, Amadeus, Twilio, your CRM, every API an agent can reach. Across the other,
the operations: charge, refund, send, book, cancel, post. Each cell — every
(integration, operation) pair — is a separate exactly-once problem, and each has
to be solved on its own terms.

For any given cell, three questions decide how hard it is:

- **Does the partner give you an idempotency key?** If you can attach a
  client-generated key to the request and the partner promises to deduplicate on
  it, the cell is close to solved.
- **Can you ask whether the action already happened?** If not, you can query
  after a failure and reconcile instead of guessing.
- **Does it need a human in the loop anyway?** Some actions are high-stakes
  enough that automatic retry is never acceptable.

A few illustrative cells make the spread obvious.

**Stripe, charge.** This is the good case. Stripe lets you pass an
`Idempotency-Key` header, holds the result of the first request against that key,
and replays it on retry. Send the same charge twice with the same key and you get
one charge and two identical responses. Solved — because someone at Stripe did the
work.

**Gmail, send.** Harder. There is no first-class idempotency key for "send this
message." You can lean on the `Message-Id` header and dedupe on the receiving
side, or check Sent before retrying, but now you are reconciling against state
that may not have settled yet. The cell is reachable, but you are building the
guarantee yourself.

**Amadeus, book.** Harder still, and the one from the opening. Booking touches
inventory and money, the confirmation may arrive seconds after the reservation is
made, and a naive retry double-books. Here you are stitching together query-then-act
with whatever idempotency the API offers, and getting the failure modes wrong
costs a real ticket.

The grid is not small, and it grows every time a new API ships. Most cells are
unsolved.

## Why this stays unsolved

The reason is not that any single cell is intellectually hard. It is that there
are thousands of them, and each one needs expert, per-API failure-mode analysis to
get right. What does this partner do on a duplicate? Does the idempotency key
expire? Is the "already exists" error safe to swallow, or does it sometimes mean
something else? This is careful, unglamorous work, and it does not generalise from
one integration to the next.

It is also work you cannot hand to the model. You cannot have an agent decide, at
runtime, whether sending $500 twice is safe. The whole point of the exactly-once
layer is to be the deterministic floor the probabilistic system stands on. A
guarantee that holds "usually" is not a guarantee.

So in practice teams pick one of three options, none of them satisfying:

1. **Accept duplicates.** The default, because it requires building nothing. Fine
   for idempotent-by-nature actions, quietly dangerous for anything financial.
2. **Require human confirmation.** Safe, and it kills the autonomy that made the
   agent worth building.
3. **Build the exactly-once infrastructure.** Correct, expensive, and a one-time
   cost paid per cell.

## The market is telling on itself

There is a tell worth noticing. Integration platforms have started marketing
idempotency as a feature — Composio, for instance, talks about it — well ahead of
shipping it as a real, per-cell guarantee in the API. When the marketing runs
ahead of the product, it usually means demand has arrived before the capability
has. That is exactly where this is: everyone building agents that act has hit this
wall, and the layer that would fix it has not been built.

Someone is going to build it properly — a per-integration, per-operation
exactly-once layer that an agent runtime can lean on the way it leans on Stripe's
idempotency key today. Until then, the answer to "what happens if the agent
crashes mid-booking" is, for most cells, two tickets.
