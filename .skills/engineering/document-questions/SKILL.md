---
name: document-questions
description: >-
  Capture and track open questions about the project.
  Use when you need to record a knowledge gap or answer a recorded question.
metadata:
  version: "1.1"
  author: Zinzan van der Merwe
---

# Document Questions

Capture and track open questions about the project.
A question is a knowledge gap that blocks work.
It is not a ticket, a task, or a discussion.

Record the question when it comes up.
Write the answer when you find it.
Remove the file when the question is closed.

## File structure

Questions live in `docs/questions/`.
A single-project repo has one set of questions at the root.
A monorepo can have questions at two levels.

Single-project repo:

```
/
├── docs/
│   └── questions/
│       ├── TAGS
│       ├── 0001-order-status-format.md
│       └── 0002-backup-policy.md
└── src/
```

Monorepo with per-project questions:

```
/
├── docs/
│   └── questions/                  ← cross-project questions
├── services/
│   └── billing/
│       └── questions/              ← per-project questions
└── front/
    └── web/
        └── questions/
```

Create files lazily.
Create them only when you have a question to record.
If `docs/questions/` does not exist, create it when you need the first question.
If `docs/questions/TAGS` does not exist, create it when you tag the first question.
If a question file is deleted (purged), the count resets.
The next question starts at 0001.

## States

A question moves through three states:

| Status     | Meaning                                                    |
|------------|------------------------------------------------------------|
| `open`     | The question is recorded. It has no answer yet.             |
| `answered` | The question has an answer in the Answer section.           |
| `closed`   | Someone acted on the answer. Purge removes closed questions. |

### Open

The question is a known gap.
Anyone can add context to the Discussion section.
The question stays open until someone provides a factual answer.

### Answered

Someone wrote the answer in the Answer section.
The question is resolved in theory.
It stays in the file system until someone acts on the answer.

### Closed

Someone used the answer to write a ticket, change code, or update docs.
The question served its purpose.
A closed question is ready for purge.

### Purge

Remove old question files from the repo.
A future reader can find the answer in the git history.

Purge targets must be `closed`.
Do not purge `open` or `answered` questions.
After a purge batch, the next question starts at 0001.

## File format

Use [QUESTION-FORMAT.md](./QUESTION-FORMAT.md).

A question file has frontmatter, a Question section, and an Answer section.
The frontmatter is lean: title, status, created, tags.

The Question section is the request for information.
It is one or two sentences.

The Answer section is the response.
Write it when the status changes to `answered`.
Keep it short: one or two sentences for a simple answer, a paragraph for a complex one.

The Discussion section records progress.
Append entries as the conversation happens.
Each entry has a date, author, and note.

## Relates

A question can reference other items.
Add a relates field in the frontmatter when the question connects to a ticket, ADR, pattern, or other question.

```yaml
---
title: What SLA does the billing service need?
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

Use relates to connect information.
A reader can follow the link to see what depends on the answer.

## When to record a question

Record a question when any of these are true:

1. Someone asks for information that is not documented.
2. A design decision depends on a fact that is not known.
3. A conversation produces an answer that someone will ask again.
4. A contributor or agent needs information that is hard to find.

Do not record questions for:

- Bug reports. Use the issue tracker.
- Feature requests. Use the ticket system.
- Casual discussion. Use the chat platform.
- Decisions. Use an ADR.

A question is a request for information.
It is nothing else.

## During the session

### Recognize when a topic is a question

Three tests. At least one must be true:

1. **The answer is a fact.** There is a correct answer that someone knows.
2. **The question blocks work.** The team cannot proceed without an answer.
3. **The answer is stable.** It will not change every week.
   If the answer changes often, it is a config value or a metric, not a question.

If the topic passes none of these tests, it is not a question. Skip it.

### Write a question

1. Determine if this is a single-project repo or a monorepo.
2. If monorepo, determine the target:
   - Cross-project question goes to root `docs/questions/`.
   - Per-project question goes to `<project>/questions/`.
3. Scan the target directory for the highest number.
   If the directory is empty, start at 0001.
4. Write the slug. Use a short kebab-case name.
   For example: `0001-order-status-format.md`.
5. Follow the format in [QUESTION-FORMAT.md](./QUESTION-FORMAT.md).
6. Add at least one tag in the frontmatter.
   Use terms from the project domain.
7. If the tag is new to `docs/questions/TAGS`, append it.
   See [TAGS-FORMAT.md](./TAGS-FORMAT.md) for the format.

### Answer a question

1. Find the question file by number or by search.
2. Change the status from `open` to `answered`.
3. Write the answer in the Answer section.
4. Append an entry to the Discussion section.
5. Keep the original question intact.

### Close a question

1. Confirm the answer was used.
   The use can be a ticket, a code change, a document update, or a decision.
2. Change the status from `answered` to `closed`.
3. Append a close entry to the Discussion section.
   Include a reference to the action taken.

### Purge closed questions

Purge removes closed question files from the repo.
The answer stays in git history.
It is safe to remove.

Use one of these strategies based on the user request:

**Strategy 1: Purge all closed questions.**
Remove every question with status `closed`.
This is the default strategy.
Use it unless the user gives a more specific rule.

**Strategy 2: Purge questions before a date.**
Remove closed questions created before a given date.
The date is in the `created` frontmatter field.
Use this when the user says purge old questions.

**Strategy 3: Purge before a date that are closed.**
Remove questions that meet both conditions:
- status is `closed`
- `created` is before the given date

Do not purge `open` or `answered` questions for any strategy.
After a purge batch, verify the directory is empty or aligned.
The next new question starts at 0001.

### Search for open questions

When you start work on a project, check `docs/questions/`.
Read the TAGS file for available topics.
Search for files with status `open` or `answered`.
These are the questions that may affect your work.

### Superseded questions

A question is superseded when the answer becomes irrelevant.
For example, the team decided not to build the feature that needed the answer.

Add a note in the Discussion section:

```
2026-10-16 (user): Superseded. The team decided not to implement order
splitting. No answer needed.
```

Keep the file.
A reader may need it to understand why a question was asked.
Do not purge superseded questions unless the team confirms they are safe to remove.

### Offer questions sparingly

Only create a question file when one of these is true:

1. The user asks a question that is worth recording.
2. An answer from the user resolves an open question.
3. The conversation produces a fact that a future reader will need.

If the question is trivial or one-off, skip the file.
A question file is for information that is hard to find again.