# Pattern Format

Pattern documents live in `docs/patterns/`.
Use sequential numbering: `0001-slug.md`, `0002-slug.md`.

Create the `docs/patterns/` directory lazily.
Create it only when you need the first pattern.

## One rule per document

A pattern document describes ONE rule for ONE part of the codebase.
If you have multiple rules, write one document per rule.

The document must show its scope in the first sentence.
The scope is the part of the codebase where the rule applies.

Examples of scope:

- **Single service**: "In the billing microservice, cursor pagination is the only pagination method."
- **Single module**: "In the invoicing module, all prices use a Money value object."
- **Cross-cutting**: "Every service must log request IDs on every response."
- **Layer**: "API layer handlers must not call repositories directly."

A reader must know from the first sentence whether the rule applies to them.

## Frontmatter

```yaml
---
tags:
  - pagination
  - cursor
---
```

Every pattern must have at least one tag.
Use terms from the project domain.
See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the TAGS file rules.

## Template

```markdown
# {Name of the pattern}

{1-2 sentences: the standard. State the scope, then the rule.
Write in imperative voice.}
```

That is the template.
A pattern document can be one paragraph.
The value is in the rule itself, not in the sections.

## Optional sections

Add these only when they add genuine value.
Most patterns do not need them.

- **Rationale.** One sentence that explains why the rule exists.
  Omit when the reason is clear from the name.
- **Examples.** One good example and one bad example.
  Use only when the rule is subtle.
- **Status.** `active` or `superseded by NNNN-slug.md`.
  Use when a new approach replaces the pattern.
  Keep the old content.
- **Enforcement.** How to check for compliance.
  Describe lint rules, review items, or automated checks.
- **See also.** Links to related patterns in this directory.