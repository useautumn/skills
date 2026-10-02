# Autumn skills

Agent skills for [Autumn](https://useautumn.com) — billing and entitlements on top of Stripe.

```bash
npx skills add useautumn/skills
```

That installs the public integration skills only.

## Public skills

| Skill | Use it for |
|---|---|
| `autumn-create-customer` | SDK setup and customer creation |
| `autumn-add-payments` | Checkout, attach, upgrades, cancel |
| `autumn-add-usage-tracking` | Check → work → track |
| `autumn-best-practices` | Integration reference |

## Internal skills

House-style writing skills from `useautumn/ai` live in `style/` and are marked `metadata.internal: true`. `npx skills add` does not list or install them unless you opt in:

```bash
INSTALL_INTERNAL_SKILLS=1 npx skills add useautumn/skills --list
INSTALL_INTERNAL_SKILLS=1 npx skills add useautumn/skills --skill explain
```

| Skill | Role |
|---|---|
| `explain` | House style for technical explanations |
| `concise` | Stricter one-verdict variant |
| `walk-me-through` | Multi-turn teaching that uses `explain` per turn |

Internal only hides them from the CLI. The files are still public on GitHub.
