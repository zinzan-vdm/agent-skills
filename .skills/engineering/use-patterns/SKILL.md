---
name: use-patterns
description: Navigate and apply code patterns and engineering standards during coding. Use before you write code that must follow project conventions.
metadata:
  version: "1.0"
  author: Zinzan
---

# Use Patterns

Navigate and apply the code patterns and engineering standards of the project. Read the rules before you write code. Make sure the code follows the patterns.

## Files

These files contain the code patterns:

| File | Content |
|---|---|
| `docs/patterns/` | Pattern documents. How to write code correctly. |
| `docs/patterns/TAGS` | Flat list of tags used in patterns. |

## Search order

To find the right way to write code for a topic:

1. **Read TAGS** in `docs/patterns/TAGS` for the available tags.
2. **Search patterns by tag.** Search `docs/patterns/` for frontmatter that matches a tag. Use a pattern such as `^- tag-name$`.
3. **Search patterns by content.** If tag search returns nothing, search the pattern bodies. Use the topic name or related terms.
4. **Read full content** of the most relevant patterns.

## Apply what you find

- Follow the **rule** in the pattern document. It tells you what code must do and what it must not do.
- If the code does not match a pattern, change the code or flag it. Do not leave a violation without a check.
- Use the **examples** as a reference for correct implementation.

## Superseded patterns

A pattern with `## Status: Superseded by NNNN-slug.md` is no longer current. Do not follow it for new code. Read the superseding pattern for the current rule. Keep the old pattern in mind only if you change old code that used the superseded rule.

## Conflicts between patterns

If two patterns give conflicting rules, surface both to the user. Let the user resolve the conflict. Do not pick one without a check.

## When a pattern is wrong

If the code shows a better approach than the pattern, the pattern may be stale. Surface it to the user. If the user agrees, use `model-patterns` to update the pattern document.