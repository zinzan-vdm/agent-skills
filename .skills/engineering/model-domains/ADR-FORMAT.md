# ADR Format

ADRs live in `docs/adr/`. Use sequential numbering: `0001-slug.md`, `0002-slug.md`.

Create the `docs/adr/` directory lazily. Create it only when you need the first ADR.

## Frontmatter

```yaml
---
tags:
  - auth
  - pagination
---
```

Every ADR must have at least one tag. Use terms from the `CONTEXT.md` glossary. See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the TAGS file rules.

## Template

```md
# {Short title of the decision}

{1-3 sentences: what is the context, what did we decide, and why.}
```

That is the template. An ADR can be a single paragraph. The value is the record of the decision and its reason. It is not in the sections.

## Optional sections

Add these only when they add genuine value. Most ADRs will not need them.

- **Status** frontmatter (`proposed | accepted | deprecated | superseded by ADR-NNNN`). Add it when you revisit decisions later.
- **Considered Options**. Add it only when the rejected alternatives are worth remembering.
- **Consequences**. Add it only when non-obvious downstream effects matter.

## Numbering

Scan `docs/adr/` for the highest existing number. Increment it by one.

## When to offer an ADR

All three statements must be true:

1. **Hard to reverse.** The cost of changing your mind later is meaningful.
2. **Surprising without context.** A future reader will look at the code and wonder "why did they do it this way?"
3. **The result of a real trade-off.** There were genuine alternatives. You picked one for specific reasons.

If a decision is easy to reverse, skip it. You can reverse it later. If it is not surprising, no one will wonder why. If there was no real alternative, there is nothing to record beyond "we did the obvious thing."

## What qualifies

- **Architectural shape.** Examples: "We use a monorepo." "The write model is event-sourced. The read model is projected into Postgres."
- **Integration patterns between contexts.** Example: "Ordering and Billing communicate through domain events, not synchronous HTTP."
- **Technology choices that carry lock-in.** Database, message bus, auth provider, deployment target. Not every library qualifies. Only the ones that take a quarter to swap out.
- **Boundary and scope decisions.** Example: "Customer data belongs to the Customer context. Other contexts reference it by ID only." The explicit rejections are as valuable as the approvals.
- **Deliberate deviations from the obvious path.** Example: "We use manual SQL instead of an ORM because X." Anything where a reasonable reader would assume the opposite. These records stop the next engineer from fixing something that was deliberate.
- **Constraints not visible in the code.** Examples: "We cannot use AWS because of compliance requirements." "Response times must be under 200ms because of the partner API contract."
- **Rejected alternatives when the rejection is non-obvious.** If you considered GraphQL and picked REST for subtle reasons, record it. Otherwise someone will suggest GraphQL again in six months.