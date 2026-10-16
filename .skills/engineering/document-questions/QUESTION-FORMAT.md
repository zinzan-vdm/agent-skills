# Question Format

Questions live in `docs/questions/`.
Use sequential numbering: `0001-slug.md`, `0002-slug.md`.
If the directory is empty, start at 0001.

Create the `docs/questions/` directory lazily.
Create it only when you need the first question.

## Frontmatter

```yaml
---
title: What is the expected SLA for payment processing?
status: open
created: 2026-10-16
tags:
  - billing
  - operations
relates:
  - tickets: BILL-42
  - docs/adr/0003-payment-retries.md
---
```

Every question must have a title and a status.
The `created` field is the date the question was first recorded.
Add relates when the question connects to a ticket, ADR, or other document.
Use tags from the project domain.

Valid status values:
- `open`
- `answered`
- `closed`

## Template

```markdown
# {Question title}

## Question

{The question. One or two sentences.}

## Answer

{The answer. One or two sentences for a simple answer.
A paragraph for a complex one. Leave empty when status is open.}

## Discussion

{YYYY-MM-DD (author): Description of the update.}

{YYYY-MM-DD (author): Another update appended in order.}
```

The Question section is the request for information.
The Answer section is the response.
The Discussion section is the log of updates.

All sections are short and factual.

## States

| Status     | Meaning                                               |
|------------|-------------------------------------------------------|
| `open`     | The question is recorded. It has no answer yet.        |
| `answered` | The answer is written. The question is resolved.       |
| `closed`   | Someone acted on the answer. Ready for purge.          |

A question starts as `open`.
When someone provides a factual answer, change the status to `answered`.
When the answer is used, change the status to `closed`.
Closed questions are candidates for purge.

## Tags

Use the TAGS system from other doc skills.
Every question must have at least one tag.
See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the format.