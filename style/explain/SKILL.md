---
name: explain
description: "House style for explaining anything technical to a human — verdict first, visuals over prose (box diagrams, call stacks, file trees, types, pseudocode, +/- rule diffs), real numbers, ruthless cuts. Use when writing an explanation, walking through how something works or why it broke, summarizing an investigation or a diff, discussing code design or a review, or when the user types /explain to get the previous answer re-explained more simply. For a one-line verdict only, use concise."
metadata:
  internal: true
---

# Explain

**Use as few sentences as possible.** Be EXTREMELY concise. The reader is ADHD — a wall of text is invisible to them, and every extra sentence buries the one that mattered.

Write the draft, then cut it to a third. If a sentence isn't carrying a fact, a number, or a decision, delete it. Length is not thoroughness — it hides the point.

## The shape — pick it before writing

"Be concise" is an adjective; models follow **shapes**, not adjectives. Before writing,
pick the shape for what you're sending, then fill it — never write first and trim after.

| What you're sending | Shape |
|---|---|
| Answer to a question | ≤ 6 sentences + 1 visual |
| How something works / why it broke | ≤ 10 sentences + 1 visual + 1 example |
| Progress / research update mid-task | bullets, one sentence each, ≤ 10, no prose around them |
| Code design / review / refactor discussion | visuals first (call stack, tree, types), ≤ 1 sentence of prose per visual |

The numbers are ceilings, not targets — most answers should land well under. The user
asking a follow-up is the mechanism for depth: undershooting and being asked to expand
is success; overshooting is failure. Never pre-answer questions that weren't asked.

A progress update, wrong then right:

```
✗ I've been digging into the license reconciliation flow and found some interesting
  things. It turns out that when a mutation happens, the gate check actually runs in
  several places. First, executeLicenseAssignmentLifecycle has a pre-gate that ...
  (12 more lines of narrative)

✓ - touchesLicenses runs 2–3× per license mutation — the lifecycle pre-gate,
    afterLicenseMutation, and resolveGatedFullCustomer each call it
  - the batch lane holds no customer locks; only the per-customer lane does
  - reconcile always drops the customer cache in a finally, even when it throws
```

Sub-bullets only when a point is genuinely composite, never deeper than one level.
Past ten bullets you're narrating, not reporting — cut findings, not words.

## Shape

1. **One sentence** naming the thing. No preamble, no restating the question.
2. **A visual** in a fenced block — pick its form from the table below.
3. **One example** with real values from the actual system (`652 ms`, `cus_abc`, `1001 rows`) — never invented ones.
4. **Then** only the detail that changes what the reader does next.

Steps 2 and 3 aren't garnish. If you can't draw it, you don't understand it yet.

## Show, don't narrate — the visual vocabulary

Analyzing prose is exhausting; the visual cortex processes structure for free. Match
the visual's form to the subject — a mismatched form reads as slop:

| Subject | Form |
|---|---|
| Runtime behavior, data moving between systems | boxes and arrows |
| Control flow, orchestration, "what calls what" | call stack |
| Where things live, scoping a refactor | file tree, one-line responsibility per entry |
| An API or design before the code exists | types + signatures |
| An algorithm | pseudocode |
| A **change** to any of the above | the same form, in diff syntax |
| A change in rules / behavior | `+/-` pseudocode — old rules out, new rules in |

### Boxes and arrows

```
┌────────────────┐        ┌────────────────────┐
│ track request  │  ──►   │ Redis (fast path)  │
│ value: 100     │        │ balance: 900       │
└────────────────┘        └─────────┬──────────┘
                                    │ lazy flush (SyncV4)
                                    ▼
                          ┌────────────────────┐
                          │ Postgres           │ ◄── cycle-end billing reads
                          │ balance: 1000      │     HERE, so 100 units of
                          └────────────────────┘     usage go unbilled
```

- Entities get boxes with their key fields inside. Arrows connect boxes.
- Label the edge that matters. Never put an if/else on an arrow — enumerate the combinations in a 3-row table instead.
- Show `before → after` when the action changes state.
- Whitespace is part of the diagram. Cramped boxes read as noise.

### Call stacks

Indentation is the call graph. Only load-bearing frames — every frame you keep is a
claim that it matters.

```
attach
  executeBillingPlan
  publishBillingTransition
    shouldPublishBillingTransition
    publishCachedFullSubject          ← Lua handoff happens here
    persistOrQueuePublishedBalanceTransitions
```

### File trees

Shallow, one line of responsibility per entry. Good for "where does this live" and
for scoping a refactor.

```
publish/
├── publishBillingTransition.ts       # post-execute orchestrator
├── shouldPublishBillingTransition.ts # liveness gate, named predicates
└── publishCachedFullSubject/         # cache action, billing-scoped
```

### Types and signatures

The shape of code before it exists — too internal for an architecture doc, exactly
what design discussion needs. Elide fields that don't matter with `// ...`.

```ts
type BalanceTransitionPlan = {
  transitions: BalanceTransition[];
  unsupportedReason?: string;   // set ⇒ publish skipped, logged
};
publishBillingTransition({ ctx, billingPlan, billingResult, policy }): Promise<void>
```

### Pseudocode

For algorithms, tighter than real code. Real branch conditions, invented syntax.

```
publish(subject, transitions)
  live A     = HGET balance_key, source
  extra      = (snapshot.balance - live.balance) + (live.adj - snapshot.adj)
  target.bal = draft.bal - extra
  HDEL A; HSET B; INCR epoch          // only after every check passed
```

### Diff syntax

When most of the shape is unchanged, reuse the same tree/stack/types form with
`+`/`-` lines instead of describing the change in prose:

```
 attach()
   executeBillingPlan
-  60-line inline publish block
+  publishBillingTransition({ ... })
```

### Pseudocode diffs

When explaining what a change *does*, don't paste the file diff. Write the old
rules as `-` lines and the new rules as `+` lines — English, not TypeScript.

```
- skip if this plan is already a variant of pro (any version)
- else stamp ALL proEu versions onto pro@v2

+ skip customize (edits own that)
+ pin version/slug → that row only
+ no pin → already-on-this-parent, else active
+ already pointing at pro@v2 → no-op
```

Load-bearing rules only. A 20-line pseudocode diff is a wall of text again.

A visual that just redraws the sentence above it is noise — delete one of them.
One visual per point; two visuals of the same thing means you didn't pick.

## Cut

- No "Great question", no "Let me explain", no "So essentially".
- No recap of what you already said earlier in the conversation.
- Numbers beat adjectives: "652 ms, 12 MB", not "quite slow and fairly large".
- Quote the real artifact — the log line, the plan node, the error — don't paraphrase it.
- Cut every sentence that only sets up the next sentence.
- Cut hedges ("essentially", "basically", "it seems") unless the doubt is real and load-bearing.
- One metaphor maximum, then drop it.
- End when the explanation ends. No summary of the summary, no "hope that helps", no unrequested next steps.

## When invoked on a previous answer

Your last message didn't land — too dense, too jargon-heavy. Re-explain **that message**, like you're explaining it to a smart friend over a beer.

- Re-explain, don't re-answer. No new questions, no new information, no tools.
- Simpler, not padded. Take the space clarity genuinely needs and not a sentence more.
- Every path, command, filename, number, URL, name and decision survives **verbatim**. Simplify the explanation around the facts, never the facts themselves.
- Flatten structure. Drop headers and ceremony, turn tables into plain sentences, keep a list only if the original genuinely had parts.
- Casual and direct ("ok so...", "the point is..."). Same language as the original.
- Nothing to simplify yet? Say so in one line and stop.

## Check before sending

Two gates, in order:

1. Could someone who knows the language but not this codebase point at the exact thing that is wrong? If no, add the missing detail. If yes, stop adding.
2. Does the message still match the shape you picked? Over the ceiling means cut whole sentences, not send.
