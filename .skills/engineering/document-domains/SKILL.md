---
name: document-domains
description: Build and maintain the project domain model. Use when you discuss codebase terms, write a CONTEXT.md, or record an ADR.
metadata:
  version: "1.1"
  author: Zinzan
---

# Document Domains

Build and sharpen the domain model of the project as you design.
This is the active discipline.
Challenge terms.
Invent edge-case scenarios.
Write the glossary and decisions when they crystallize.

Reading `CONTEXT.md` or `docs/adr/` is not this skill.
That is a one-line habit you do on your own.
This skill is for changes to the model.

## File structure

Determine the repo type before you write.
A single-project repo has one project at the root.
A monorepo has multiple projects.
The same doc structure applies at both levels.

Single-project repo:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/
│       ├── TAGS
│       ├── 0001-event-sourced-orders.md
│       └── 0002-postgres-for-write-model.md
└── src/
```

Monorepo with project-level documents:

```
/
├── CONTEXT.md
├── docs/
│   └── adr/                          ← cross-project decisions
├── services/
│   └── ordering/
│       ├── CONTEXT.md
│       └── adr/                      ← per-project decisions
└── front/
    └── web/
        └── adr/
```

Create files lazily.
Create them only when you have something to write.
If no `CONTEXT.md` exists, create one when you resolve the first term.
If no `docs/adr/` directory exists, create it when you need the first ADR.
If no `docs/adr/TAGS` file exists, create it when you write the first ADR with tags.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the language in `CONTEXT.md`,
surface it.

"Your glossary defines 'cancellation' as X, but you seem to mean Y.
Which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise term.

"You say 'account'. Do you mean the Customer or the User?
Those are different things."

### Discuss concrete scenarios

When you discuss domain relationships, stress-test them with specific scenarios.
Invent scenarios that test edge cases.
Force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees.
If you find a contradiction, surface it.

"Your code cancels entire Orders, but you just said partial cancellation is possible.
Which is right?"

### Update CONTEXT.md inline

When you resolve a term, update `CONTEXT.md` right there.
Do not batch the updates.
Capture them when they happen.
Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

`CONTEXT.md` must stay clear of implementation details.
It is a glossary.
It is nothing else.

### Write an ADR

An ADR records ONE decision.
If you need to record two independent decisions, write two ADRs.

Before you write, ask: is this one decision, or several?
If the decision has multiple independent parts, split them.
Each ADR must be understandable without reading the other ADRs.

Use the title to identify the single decision.
Use the body to give the context, the decision, and the reason.
Do not add extra decisions as justification.

When the three conditions are true, create an ADR.

Determine where the ADR belongs:

1. Is the repo a monorepo or a single-project repo?
2. If single-project repo, write all ADRs to `docs/adr/`.
3. If monorepo, determine the target:
   - Cross-project ADR affects all or most projects.
     Write to `docs/adr/`.
   - Per-project ADR affects one project only.
     Write to `<project>/adr/`.
4. If a per-project ADR overrides a top-level ADR,
   add the override in its frontmatter.
   For example: `overrides: 0001-name`.

To write an ADR:

1. Scan the target directory for the highest number.
   Increment it by one.
2. Write the slug. Use a short kebab-case name.
   For example: `0001-event-sourced-orders.md`.
3. Write the decision. Use [ADR-FORMAT.md](./ADR-FORMAT.md).
4. Add at least one tag in the frontmatter.
   Use terms from the `CONTEXT.md` glossary.
5. If a tag in the ADR frontmatter is not yet in `docs/adr/TAGS`,
   append it. See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the format.

### Offer ADRs sparingly

Only offer to create an ADR when all three statements are true:

1. **Hard to reverse.** The cost of changing your mind later is meaningful.
2. **Surprising without context.** A future reader will look at the code
   and wonder "why did they do it this way?"
3. **The result of a real trade-off.**
   There were genuine alternatives.
   You picked one for specific reasons.

If any of the three is missing, skip the ADR.