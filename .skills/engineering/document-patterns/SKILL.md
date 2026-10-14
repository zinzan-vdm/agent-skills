---
name: document-patterns
description: Document code patterns and engineering standards. Use when you set a convention or write a rules document for how to write code.
metadata:
  version: "1.1"
  author: Zinzan van der Merwe
---

# Document Patterns

Build and sharpen the code patterns and engineering standards of the project.
This is the active discipline.
Recognize when a convention needs a formal rule.
Write the rule.
Keep it current as the codebase changes.

Reading `docs/patterns/` is not this skill.
That is a one-line habit you do on your own.
This skill is for changes to the model.

## File structure

Determine the repo type before you write.
A single-project repo has one set of patterns at the root.
A monorepo can have patterns at two levels.

Single-project repo:

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

Monorepo with project-level patterns:

```
/
├── docs/
│   └── patterns/                    ← cross-project conventions
├── services/
│   └── billing/
│       └── patterns/                ← per-project conventions
└── front/
    └── web/
        └── patterns/
```

Create files lazily.
Create them only when you have a pattern to write.
If `docs/patterns/` does not exist, create it when you need the first pattern.
If `docs/patterns/TAGS` does not exist, create it when you write the first pattern with tags.

## During the session

### Recognize when a topic is a pattern

Three tests. At least one must be true:

1. **It dictates code shape.** There is a right way and a wrong way to write the code.
2. **It is cross-cutting.** The rule applies to multiple modules or services.
3. **A developer or agent must know it to write correct code.**
   The rule is not obvious from the architecture.

If the topic passes none of these tests, it is not a pattern.
Skip it.

### Keep patterns targeted

A pattern must describe ONE rule.
It must target ONE part of the codebase.

One rule. One scope. One file.

Do not put multiple rules in one document.

If a topic covers multiple rules, split it.
Write one file per rule.

Example of a correctly targeted pattern:

- `0001-cursor-pagination.md` has one scope and one rule:
  "In the API layer, all list endpoints use cursor pagination."
  That is one rule for one scope.

Example of an incorrect pattern:

- A single file that says "use cursor pagination,
  return 404 for not found, format errors as RFC 7807, use 200 for bulk endpoints,
  and validate with zod."
  That is five rules. Write five files.

The first sentence of a pattern document must name the scope and the rule.
The reader must know from the first sentence whether the rule applies to them.

When you list a pattern for the user, use the correct scope.

"Use cursor pagination for list endpoints in all API layer routes."
That sentence has one scope (API layer routes) and one rule (cursor pagination).
It is correctly targeted.

"Use cursor pagination, error format, and load balancer settings."
That is three rules.
List them separately.

### Write a pattern document

Determine where the pattern belongs:

1. Is the repo a monorepo or a single-project repo?
2. If single-project repo, write all patterns to `docs/patterns/`.
3. If monorepo, determine the target:
   - Cross-project pattern applies to all projects. Write to `docs/patterns/`.
   - Per-project pattern applies to one project. Write to `<project>/patterns/`.
4. If a per-project pattern overrides a top-level pattern,
   add the override in its frontmatter.
   For example: `overrides: 0001-name`.

To write a pattern document:

1. Scan the target directory for the highest number.
   Increment by one for the new file.
2. Write the slug. Use a short kebab-case name.
   For example: `0001-cursor-pagination.md`.
3. Follow the format in [PATTERN-FORMAT.md](./PATTERN-FORMAT.md).
4. Write the rule in imperative voice.
   Use short sentences.
   Use one name for one thing.
5. Add at least one tag in the frontmatter.
   Use tags from the project domain or from existing `TAGS` files.
6. If the tag is new, append it to `docs/patterns/TAGS`.
   See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the format.
7. Include examples only when the rule is subtle enough to get wrong.

### Check patterns against code

When you read code, check that it follows the documented patterns.
If you find a violation, surface it:

"The code in services/products/src/routes.ts returns raw entities.
 docs/patterns/0003-service-layering.md says routes must return models.
 Which is right?"

If the pattern is wrong (the code shows a better approach),
update the pattern document.
The code is the source of truth for what works.
The pattern is the record of what must be standard.

### Mark superseded

When a rule changes, update the pattern in place.
Do not create a new file for the change.
The number is stable.

When a new approach replaces a pattern, add a superseded-by header.
Preserve the old content.
A future reader may need it to understand old code that followed the superseded rule.

```
## Status

Superseded by docs/patterns/0005-webhook-format.md
```