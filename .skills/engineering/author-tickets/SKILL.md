---
name: author-tickets
description: >-
  Plan tickets from a brief. Decompose into epics and tasks.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Author Tickets

Plan work and write tickets from a brief.
This skill produces the plan and the ticket bodies.
It does not describe ticket storage or implementation workflow.
Those are separate concerns.

When you get a brief, do two things:

1. Plan the work. Decompose the brief into an epic and its tasks.
2. Write a ticket body for each task using the six-section format.

A human reviews the plan and the ticket bodies before anyone writes code.

## When to use this skill

Use this skill when you must turn a brief into trackable tickets.
The brief can come from a user request, a product requirement,
a bug report, or a feature specification.

Do not use this skill when you write code.
This skill is for planning only.

## Process

### Step 1: Understand the brief

Read the brief. Do you understand the outcome?
If the brief is vague, ask for clarification before you plan.

Do not plan against an unclear brief.
A plan built on an unclear brief produces bad tickets.

### Step 2: Decide the level

Apply the litmus test:

| Question | Level |
|----------|-------|
| Does this brief need a plan before anyone writes code? | epic |
| Does this brief fit into one independently reviewable deliverable? | task |
| Does the user already have a clearly defined task? | task |

A brief that needs multiple deliverables is an epic.
A brief that fits into one deliverable is a task.

### Step 3: Decompose into tasks

If the brief is an epic, break it into tasks.

Each task must pass two checks:

1. It delivers one independently reviewable result.
2. It delivers value by itself.

A task that fails check 1 is too big. Make it an epic instead.
A task that fails check 2 is too small. Combine it with the
tasks it depends on.

### Step 4: Apply vertical slicing

Each task cuts through all layers of the stack.
Each task delivers user value you can test or see.

Good example:
"POST /products (create)" needs a route handler,
validation, data storage, and a response. A user can test it.

Bad example:
"Create the database table for products" needs other tasks
before it delivers value. Combine it with the endpoint task.

A task is a behavior change a user can see.
It is not a list of files to change.

### Step 5: Right-size tasks

Use this checklist to validate each task:

| Condition | Action |
|-----------|--------|
| Task delivers more than one independent result | Make it an epic. |
| Task needs another task to deliver value | Combine them. |
| Task has more than 10 acceptance criteria | Split the task. |
| Task has only one acceptance criterion | Combine with next task. |
| Task name contains "and" | Split the task. |
| Task name is a technical layer | Re-slice vertically. |

### Step 6: Write ticket bodies

Write one ticket body per task.
Use ticket-format.md as the template.

Write all six sections in the order the template defines.
Do not skip sections. Do not merge sections.

## Hierarchy reference

| Level | Duration | Plan needed? | Example |
|-------|----------|--------------|---------|
| Epic | Multiple pull requests | Yes | "User registration with email" |
| Task | One pull request | No | "POST /auth/register" |
| Subtask | One step inside a task | No (goes in body) | "Add password validation" |

A subtask goes in the task body as a markdown checklist item.
Do not create a separate ticket file for a subtask.

## What a good epic looks like

A good epic has one outcome.
You can describe the outcome in one sentence.

Examples of good epic outcomes:

- A user can register with email and password.
- A user can search products by name and category.
- An admin can generate a sales report for a date range.

If you cannot describe the outcome in one sentence,
the epic is too broad. Split it into multiple epics.

An epic is not a category. Do not create epics named
"frontend work" or "backend tasks" or "bug fixes".

## What a good task looks like

A good task has all of these:

- It delivers one independently reviewable result.
- A user can see or test the result.
- It has 3 to 10 acceptance criteria.
- The acceptance criteria are checkable.
- The task name describes what the user gets, not what
  the code does.

Good task name: "User can register with email and password"
Bad task name: "Add bcrypt hashing to user model"

## Reference material

- ticket-format.md: template for one task body
- The six sections: Overview, Context, Specification,
  Technical Notes, Reference Material, Acceptance Criteria