---
name: model-patterns
description: Document code patterns and engineering standards. Use when you set a convention or write a rules document for how to write code.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Model Patterns

Build and sharpen the code patterns and engineering standards of the project. This is the active discipline. Recognize when a convention needs a formal rule. Write the rule. Keep it current as the codebase changes.

Reading `docs/patterns/` for guidance is not this skill. That is a one-line habit. This skill is for changes to the model, not for reading it.

## File structure

Pattern documents live in one `docs/patterns/` directory at the project root:

```
/
├── docs/
│   └── patterns/
│       ├── TAGS
│       ├── 0001-cursor-pagination.md
│       ├── 0002-error-code-format.md
│       └── 0003-service-layering.md
└── src/
```

Create files lazily. Create them only when you have a pattern to write. If no `docs/patterns/` directory exists, create it when you need the first pattern document. If no `docs/patterns/TAGS` file exists, create it when you write the first pattern with tags.

## During the session

### Recognize when a topic is a pattern

Three tests. At least one must be true:

1. **It dictates code shape.** There is a right way and a wrong way to write the code.
2. **It is cross-cutting.** The rule applies to multiple modules or services.
3. **A developer or agent must know it to write correct code.** The rule is not obvious from the architecture alone.

If the topic passes none of these tests, it is not a pattern. Skip it.

### Write a pattern document

To write a pattern document:

1. Scan `docs/patterns/` for the highest existing number. Increment by one for the new file.
2. Write the slug. Use a short kebab-case name. For example: `0001-cursor-pagination.md`.
3. Follow the format in [PATTERN-FORMAT.md](./PATTERN-FORMAT.md).
4. Write the rule in imperative voice. Use short sentences. Use one name for one thing.
5. Add at least one tag in the frontmatter. Use terms from the project domain or from existing `TAGS` files.
6. If a tag in the pattern frontmatter is not yet in `docs/patterns/TAGS`, append it. See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the format.
7. Include examples only when the rule is subtle enough to get wrong.

### Check patterns against code

When you read or write code, check that it follows the documented patterns. If you find a violation, surface it:

"The code in services/products/src/routes.ts returns raw entities. docs/patterns/0003-service-layering.md says routes must return models. Which is right?"

If the pattern is wrong (the code shows a better approach), update the pattern document. The code is the source of truth for what works. The pattern is the record of what must be standard.

### Mark superseded

When a rule changes, update the pattern document. Do not create a new file for the change. The number is stable.

When a new approach replaces a pattern, add a superseded-by header. Preserve the old content. A future reader may need to understand old code that followed the superseded rule.

```
## Status

Superseded by docs/patterns/0005-webhook-format.md
```