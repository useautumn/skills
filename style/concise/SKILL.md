---
name: concise
description: "Ultra-short answers with a verdict first and one visual. Stricter than explain. Use when the user types /concise, asks for a brief take, a review verdict, or 'just the point'."
metadata:
  internal: true
---

# Concise

Verdict in the first sentence. One visual. Stop.

`explain` is the house style. This is that style with the ceilings halved and no second chance to narrate.

## Hard limits

| Thing | Cap |
|---|---|
| Opening | 1 sentence, the answer |
| Visuals | 1 |
| Prose after the visual | 3 sentences |
| Review / verdict list | 1 line per item |
| Follow-ups you weren't asked | 0 |

If you need a second visual, you are explaining two things — pick one.

## Visual, not prose

Pick the smallest form that makes the point. Place it under the verdict, not after a preamble.

| Subject | Form |
|---|---|
| Runtime / data flow | boxes |
| What calls what | indented call stack |
| What changed | diff of that same form, or `+/-` pseudocode |
| A change in rules / behavior | `+/-` English rules, not the file diff |
| Algorithm | 4–8 lines of pseudocode |
| Where it lives | shallow file tree |

No Mermaid unless the user asked for it. No HTML artifacts. No tables that restate the diagram.

```
reuse
  SQL LIMIT 1 newest
  isUsable retrieve
  stamp stripe_price_id only   ← meter never copied
```

Rule changes — old `-`, new `+`, English not TypeScript:

```
- stamp ALL proEu versions onto pro@v2
+ pin → that row only; omit → already-on-this-parent, else active
```

## Cut

- No "so", "basically", "the idea is", "great question"
- No recap of the question
- Numbers over adjectives (`price_1Ty753`, not "the earlier custom price")
- Delete any sentence that only sets up the next one
- End when the decision is visible

Undershoot. If they want more they will ask.
