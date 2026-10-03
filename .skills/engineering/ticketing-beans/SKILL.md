---
name: ticketing-beans
description: Prime the agent to use Beans as the project ticketing tool.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Ticketing Beans

A thin loader that primes the agent with Beans' canonical instructions.
The agent runs `beans prime` at session start to ingest the project
types, statuses, commands, and workflows.

## What is Beans

Beans is a CLI issue tracker for humans and robots.
Issues (beans) are Markdown files with YAML frontmatter.
They live in a `.beans` directory in the project.

Project page: https://github.com/hmans/beans

## Session start

When this skill loads:

1. Check for Beans. Run `which beans`.
   If not found, run `brew install hmans/beans/beans`
   or `go install github.com/hmans/beans@latest`.

2. Prime. Go to the project root. Run `beans prime`.
   Read the full output into your context.
   This is the canonical agent guide for this project.
   It contains the types, commands, workflows, and
   conventions that apply here.

3. No file is written to disk.
   Priming happens each session.
   The output always matches the current config.

The user prompt decides what to do with Beans.
The primed instructions tell you how.

## Configure the project after init

Run `beans init` in the project root.
This creates `.beans.yml` and the `.beans` directory.

Set `project.name` from the project directory name.
Set `beans.prefix` from a short form of the project name.

For a project directory called `cli-roundtable`:
- `project.name` is `cli-roundtable`
- `beans.prefix` is `cli-`

For a project directory called `meety`:
- `project.name` is `meety`
- `beans.prefix` is `mee-`

All other fields have defaults that work without changes:

- `beans.path` defaults to `.beans`
- `beans.id_length` defaults to 4
- `beans.default_status` defaults to `todo`
- `beans.default_type` defaults to `task`

Edit `.beans.yml` after `beans init`.
The file is YAML. Each field has a comment that explains it.

## Troubleshooting

| Problem | Action |
|---------|--------|
| `beans: not found` | Run `brew install hmans/beans/beans` |
| `no .beans directory` | Run `beans init` in the project root |
| `beans prime` shows nothing | Run from the directory with `.beans.yml` |