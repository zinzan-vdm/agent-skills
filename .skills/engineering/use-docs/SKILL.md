---
name: use-docs
description: >-
  Load the right doc skills for the work at hand.
  Reads project docs, clarifies understanding, and selects domain/config/pattern skills
  to load.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Use Docs

Load the correct documentation skills for the work you do.
This skill is a router. It tells you which other skills to load based on the task.

Do not load every skill at once. Load only the skills you need.
This keeps the context small and the agent focused.

## Skills this skill manages

| Skill | Load when |
|---|---|
| `use-domains` | You need to understand the domain model and architecture decisions. |
| `document-domains` | You learn something about the application domain and need to document it. |
| `use-patterns` | You need to understand codebase patterns and engineering standards. |
| `document-patterns` | You learn a new pattern or standard and need to document it. |
| `use-config` | You need to understand how configuration works for a service or app. |
| `document-config` | You learn about configuration variables and need to document them. |
| `write-tism` | You need to update or write documentation. Use this as the writing system for all technical authoring. |
| `grill` | The agent needs to discuss and understand the subject more deeply. |

## How to select skills

### Step 1: Understand the task

Read the task or request from the user.
Identify what the agent needs to do.

### Step 2: Match task to skills

The table lists each skill and the condition to load it.
When the condition is true, load the skill.

A task can need more than one skill.
Load all the skills that match.

### Step 3: Decide read or write

A `use-*` skill means read and understand the docs.
Do not write to the project docs when you load a `use-*` skill.

A `document-*` skill means create or update the docs.
The `document-*` skills include enough reading logic to write correctly.
They scan for existing file numbers, check existing tags, and read what they need.
Do not load a `use-*` skill for the same area when you load a `document-*` skill.
The `document-*` skill is the single skill for write tasks.

A task that reads and writes the same area loads only the `document-*` skill.

### Step 4: Use `write-tism` for all documentation text

Every document you create or update must use the `write-tism` skill.
This includes ADRs, CONTEXT.md entries, config docs, and pattern documents.
Technical authoring uses the ASD-STE100 flavored mode from `write-tism`.

### Step 5: Use `grill` for open questions

Load `grill` when any of these are true:

- The task has open questions the agent cannot answer alone.
- The user suggests an approach and the agent needs to understand their intent.
- The agent needs clarification about the domain before writing docs.
- The user asks the agent to discuss or stress-test a decision.

`grill` is not for each small question. It is for a structured conversation
about the subject. Use it when the discussion needs depth.

## Example scenarios

### Scenario A: Document a new domain concept

The user says: "We have a new entity called Invoice. It tracks payment status."

Load these skills:
1. `grill`: to discuss the concept with the user.
   Clarify boundaries, relationships, and edge cases.
2. `document-domains`: to write the CONTEXT.md entry and an ADR.
3. `write-tism`: to format the documentation.

### Scenario B: Understand config before deployment

The user says: "Deploy the billing service to staging."

Load these skills:
1. `use-config`: to read the config docs for the billing service.
2. `use-domains`: to understand the billing domain if config references domain terms.
3. `grill`: if any config values are unclear.

### Scenario C: Add a coding pattern

The user says: "All our API routes should use cursor pagination."

Load these skills:
1. `grill`: to discuss the rule and make sure the boundaries are clear.
2. `document-patterns`: to write the pattern document.
3. `write-tism`: to format the pattern document.

### Scenario D: Read docs to implement a feature

The user says: "Add a new endpoint that lists user invoices."

Load these skills:
1. `use-domains`: to understand the domain terms and decisions.
2. `use-patterns`: to find the code patterns you must follow.
3. `use-config`: if the new endpoint needs config.
4. `grill`: if the requirements have open questions.

### Scenario E: Document config for a new service

The user says: "Add config docs for the notification service."

Load these skills:
1. `document-config`: to write the config document.
2. `write-tism`: to format the document.
3. `grill`: if the variable sources or defaults are unclear.

## What to do when no skill matches

If the task does not match any condition in the table,
do not load any documentation skills.
Only load skills you need. Extra skills add tokens without value.

## Skill load order

When you load multiple skills, load them in this order:

1. `grill`: first, to clarify the subject.
2. `document-*`: second, to write new or updated docs.
   The `document-*` skill reads what it needs as part of writing.
3. `use-*`: last, for read-only tasks that do not write.
   Load these only when the task does not need a `document-*` skill.
4. `write-tism`: always last, to apply the writing system to the model output.

Read-only task: `use-*` skill (no `document-*` needed).
Write or mixed task: `document-*` skill (no `use-*` for the same area needed).
This keeps each load set minimal and focused.