---
name: walk-me-through
description: "Teach a concept incrementally across multiple turns, one idea per message, stopping after each for a comprehension check before continuing. Use when asked to walk through, explain step by step, teach, or build up understanding of a system, codebase, or concept — especially when the user says they don't fully grasp something."
metadata:
  internal: true
---

# Walk Me Through

Teach one concept per message, then stop and wait. The user sets the pace, not you.

Within a single turn, follow the `explain` skill's house style (verdict first, real
numbers, ruthless cuts). This skill governs the pacing *across* turns.

## The rule

Never deliver the whole explanation in one message, however well-organised. A complete
answer split into headed sections is still one message — that is the thing this skill
exists to prevent. One concept, then stop.

## Before you start

Establish the baseline in one question, not a quiz: what's adjacent knowledge they
already have, and what's the specific thing that isn't landing. "I know Autumn but not
usage windows" tells you to skip the fundamentals and not explain balances from scratch.

Plan the chain privately — usually four to eight concepts, each one depending on the
one before. Don't show the outline. A roadmap invites "just give me all of it" and
spoils the payoff of each step.

## Each turn

1. **One concept, named.** Open with what this turn is about: "Concept 3: nothing ever
   resets the counter."
2. **Make it concrete.** Real numbers, a real timeline, the actual shape of the data.
   Abstractions don't land; `usage: 140, window_start_at: Apr 20 00:00` does.
3. **Explain why it's built that way.** The mechanism is the content. "There's no
   midnight cron because a job that runs late would wrongly block customers."
4. **End with a comprehension check** on the load-bearing part.
5. **Stop.** Do not start the next concept, do not preview it.

Keep each turn short — a few paragraphs. If a turn needs headings and subsections,
it's more than one concept. Split it.

## The comprehension check

Target the one thing that must be true for the next concept to work. Not "does that
make sense?" — that gets a reflexive yes.

- Weak: "Make sense so far?"
- Strong: "Are you clear that the 200/day is not 200 emails being handed to you, just
  a tally being watched?"

When a concept has a common misconception, name it: "that's the piece people get
backwards." It gives them permission to say no.

## Responding to their answer

- **Confirmed** → next concept.
- **Hesitation, a partial answer, or a follow-up question** → re-teach the *same*
  concept from a different angle. Don't advance, and don't repeat the same words —
  reach for a different example or a concrete counter-case.
- **They jump ahead** → answer the question briefly, then return to where you were.

## Finishing

On the last turn, recap the chain in one or two sentences, each link in order — "a
window is a tally plus the time it belongs to → it never resets, it just stops
matching → what 'today' means comes from an anchor". Then offer the next step: go
deeper on one link, or apply it to their actual problem.
