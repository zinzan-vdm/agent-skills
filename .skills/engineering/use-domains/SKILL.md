---
name: use-domains
description: Navigate and apply the project domain model during coding. Use before you write code that touches a domain concept or when you need to understand a project term.
metadata:
  version: "1.0"
  author: Zinzan
---

# Use Domains

Navigate and apply the domain model of the project. Read the glossary and decisions before you write code. Search for rules that apply to the topic.

## Files

These files contain the domain model:

| File | Content |
|---|---|
| `CONTEXT.md` | Glossary of project terms. What each term means. |
| `CONTEXT-MAP.md` | Index of multiple contexts. Only exists in multi-context repos. |
| `docs/adr/` | Decision records. Why each choice was made. |
| `docs/adr/TAGS` | Flat list of tags used in ADRs. |

## Search order

If the repo is a monorepo, load project-level docs before top-level docs. A project-level doc on the same topic wins over a top-level doc. Top-level fills gaps that the project level does not cover.

To understand a domain concept in this project:

1. Determine if this is a monorepo or a single-project repo.
2. If monorepo, search the project-level `adr/` first.
3. **Read CONTEXT.md** for the term definition.
4. **Read TAGS** in the relevant `adr/` directory for the available tags.
5. **Search ADRs by tag.** Search the relevant `adr/` for frontmatter that matches a tag. Use a pattern such as `^- tag-name$`.
6. **Search ADRs by content.** If tag search returns nothing, search the ADR bodies. Use the concept name or related terms.
7. **Read full content** of the most relevant ADRs.
8. If monorepo and nothing found at project level, repeat steps 3-7 at the top-level `docs/adr/`.

## Apply what you find

- Use the **terms** from `CONTEXT.md` in code and comments. Do not introduce synonyms.
- Follow the **decisions** in ADRs. They override default assumptions.
- If the code does not match an ADR, surface it. The code or the ADR may be wrong. Do not deviate without a check.

## Superseded decisions

An ADR with status `superseded by ADR-NNNN` is no longer current. Read the superseding ADR for the current rule. Keep the old ADR content. You may need it to understand old code that follows the superseded rule.

## When something is missing

If you search the glossary and ADRs and find nothing about a concept, tell the user. Ask if the concept needs a formal definition. If the user agrees, use `model-domains` to document it.