---
name: use-tickets
description: >-
  Read and understand tickets from the project structure.
  Get context from existing tickets when you start work.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Use Tickets

Read and understand tickets in the project.
Use existing tickets to get context before you write code.

This skill does not describe how to plan or author tickets.
That is the author-tickets skill.

---

## How tickets are structured

A ticket has two parts:

- **Metadata.** Fields that classify the ticket.
- **Body.** Sections that specify the work.

### Metadata fields

| Field | What it tells you |
|-------|-------------------|
| `id` | Unique identifier for the ticket |
| `title` | Short name for the ticket |
| `type` | Type of ticket: epic, task, bug, feature |
| `status` | Current state: draft, todo, in-progress, completed |
| `priority` | Importance: critical, high, normal, low, deferred |
| `parent` | ID of the parent epic. Omitted for epics with no parent. |
| `tags` | Labels for search and filtering. |

### Body sections in order

| Section | What it tells you |
|---------|-------------------|
| Overview | What the task does. Read this first. |
| Context / Background | Why the task exists. The decisions that caused it. |
| Specification | The exact contract. Parameters, response shapes, error states. |
| Technical Notes | How to implement it. Which files to change, which patterns to use. |
| Reference Material | Links to related tickets and documents. |
| Acceptance Criteria | Conditions that must pass for the task to be complete. |

### Summary of Changes

When a task is complete, the body has a Summary of Changes section.
It lists the files created or changed.
Read this section if you pick up work after another agent.

---

## How to read a ticket

### Step 1: Read the metadata

Check the type and status first.

- If status is `completed`, the work is done. Do not start implementation.
- If status is `draft` or `todo`, the work has not started.
- If status is `in-progress`, another agent is working on it.
  Read the Summary of Changes to see what is done.
- If type is `epic`, the ticket is a container.
  Find its child tasks through the `parent` field.

### Step 2: Read the Overview

The overview tells you what the task delivers.
Read this before you read the specification.
If the overview is unclear, read the parent epic for more context.

### Step 3: Read the Specification

This is the contract you must implement.
Read every parameter, response shape, and error state.
Do not deviate from the specification.

### Step 4: Read the Technical Notes

These tell you how to implement the specification.
Which files to change. Which patterns to use.
The Technical Notes are guidance, not a substitute for the specification.

### Step 5: Read the Acceptance Criteria

These are the pass/fail conditions.
Check each one before you mark the task as complete.
The Acceptance Criteria are the definition of done.

### Step 6: Check the parent epic

If the ticket has a parent, read the parent epic.
The epic gives you the broader context.
Tasks from the same epic share a common goal.

---

## How to navigate tickets

### Find the parent epic

Look at the `parent` field in the ticket metadata.
The parent is an epic ID.

### Find sibling tasks

Sibling tasks share the same parent epic.
Read them to understand the scope of the full epic.
Your task may depend on work from a sibling task.

### Find blocked or blocking tickets

Check the `depends_on` field in the ticket metadata.
If a task your ticket depends on is not complete,
your ticket cannot be completed yet.

---

## How to update a ticket

### When you start work

Set the status to `in-progress`.

### When you complete work

1. Check every acceptance criterion.
2. If all pass, set the status to `completed`.
3. Append a Summary of Changes section to the body.
   List the files created or changed.

### When the task depends on other work

Do not mark the task as complete until the
dependency is complete and your acceptance
criteria pass.

---

## How to link tickets

Include the ticket ID in your commit message.
Use the format `Ticket: {id}` in the commit body.
This creates a permanent link between the
implementation and the ticket.