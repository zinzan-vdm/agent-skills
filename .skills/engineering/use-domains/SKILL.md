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

To understand a domain concept in this project:

1. **Read CONTEXT.md** for the term definition.
2. **Read TAGS** in `docs/adr/TAGS` for the available tags.
3. **Search ADRs by tag.** Search `docs/adr/` for frontmatter that matches a tag. Use a pattern such as `^- tag-name$`.
4. **Search ADRs by content.** If tag search returns nothing, search the ADR bodies. Use the concept name or related terms.
5. **Read full content** of the most relevant ADRs.

## Apply what you find

- Use the **terms** from `CONTEXT.md` in code and comments. Do not introduce synonyms.
- Follow the **decisions** in ADRs. They override default assumptions.
- If the code does not match an ADR, surface it. The code or the ADR may be wrong. Do not deviate without a check.

## Superseded decisions

An ADR with status `superseded by ADR-NNNN` is no longer current. Read the superseding ADR for the current rule. Keep the old ADR content. You may need it to understand old code that follows the superseded rule.

## When something is missing

If you search the glossary and ADRs and find nothing about a concept, tell the user. Ask if the concept needs a formal definition. If the user agrees, use `model-domains` to document it.