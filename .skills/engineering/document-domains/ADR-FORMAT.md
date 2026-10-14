# ADR Format

ADRs live in `docs/adr/`.
Use sequential numbering: `0001-slug.md`, `0002-slug.md`.

Create the `docs/adr/` directory lazily.
Create it only when you need the first ADR.

## One decision per ADR

An ADR records ONE decision.
If you need to record two independent decisions, write two ADRs.

Before you write, ask: is this one decision, or several?
If the decision has multiple independent parts, split them.
Each ADR must be understandable on its own.

The title must identify the single decision.
The body gives the context, the decision, and the reason.
Do not add extra decisions as justification.

## Frontmatter

```yaml
---
tags:
  - auth
  - pagination
---
```

Every ADR must have at least one tag.
Use terms from the `CONTEXT.md` glossary.
See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the rules.

## Template

```md
# {Short title of the single decision}

{1-3 sentences: what is the context, what did we decide, and why.}
```

That is the template.
An ADR can be a single paragraph.
The value is the record of the decision and its reason.
It is not in the sections.

## Optional sections

Add these only when they add genuine value.
Most ADRs will not need them.

- **Status** frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`).
  Add it when you revisit decisions later.
- **Considered Options**. Add it only when the rejected alternatives are worth remembering.
- **Consequences**. Add it only when non-obvious downstream effects matter.

## Scope in ADRs

An ADR does not need an explicit scope field.
The directory structure shows the scope:
`docs/adr/` for cross-project decisions.
`<project>/adr/` for project-level decisions.
Use the tags to connect the ADR to the relevant domain terms.

## When to offer an ADR

All three statements must be true:

1. **Hard to reverse.** The cost of changing your mind is meaningful.
2. **Surprising without context.** A future reader will look at the code and wonder "why did they do it this way?"
3. **The result of a real trade-off.** There were genuine alternatives.

If the decision is easy to reverse, skip the ADR.
You can reverse it later.
If it is not surprising, no one will wonder why.
If there was no real alternative, there is nothing to record.