# Ticket Body Format

This is the template for one task body.
Write all six sections in this order.
Do not skip sections. Do not merge sections.

---

## Overview

One paragraph. Not more than four sentences.

State what the task does and what problem it solves.
Be specific. An agent or developer who reads this overview
must understand the scope without reading other tickets.

---

## Context / Background

State why the task exists.
Link to the parent epic.
State which decisions caused this task.
Do not repeat the overview.

---

## Specification

State the exact behavior or contract.

Use a table for parameters.
Use a code block for request and response shapes.
State every error state and its status code.

Do not leave ambiguity about field names, types,
or response shapes.

---

## Technical Notes

State how to implement the task.

Which files to change.
Which patterns to use.
Which edge cases to handle.

The agent reads this section before it writes code.
Do not put instructions the agent can infer.

---

## Reference Material

List links to related documents.

- Parent epic ID
- Design documents
- API specifications
- Architecture decision records

Each link must have a label that describes what it is.

---

## Acceptance Criteria

List checkable conditions.

Use `- [ ]` for each condition.
Each condition must pass or fail with no judgment.

An agent checks the acceptance criteria before it
marks the task as complete.
A reviewer also checks them.

Good examples:

- `GET /products returns 200 with an empty data array`
- `POST /products with duplicate name returns 409`
- `The response shape matches the specification`

Bad examples:

- `The code looks clean` (not checkable)
- `Performance is good` (not specific)