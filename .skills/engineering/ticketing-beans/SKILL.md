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

## Troubleshooting

| Problem | Action |
|---------|--------|
| `beans: not found` | Run `brew install hmans/beans/beans` |
| `no .beans directory` | Run `beans init` in the project root |
| `beans prime` shows nothing | Run from the directory with `.beans.yml` |