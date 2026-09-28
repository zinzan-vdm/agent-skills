---
name: use-config
description: Navigate config docs in docs/conf/. Use before deploy.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Use Config

Navigate and apply the system variable definitions.
Read the config docs before you deploy a service,
set up an environment, or configure a dependency.

System variables drive behavior from outside the code.
The config docs tell you which variables exist,
where they come from, and what they control.

## Files

These files contain the system variable definitions.

| File | Content |
|---|---|
| `docs/conf/` | Variable definitions. Types, sources, defaults, constraints. |
| `docs/conf/TAGS` | Flat list of tags used in config docs. |

## Search order

If the repo is a monorepo,
load project-level config docs before root-level config docs.
A project-level doc on the same topic wins over a root-level doc.
Root fills gaps that the project level does not cover.

To find the variables for a topic:

1. Determine if this is a monorepo or a single-project repo.
2. If monorepo, search the project-level `docs/conf/` first.
3. Read TAGS in the relevant `docs/conf/` directory.
4. Search config docs by tag.
   Search the frontmatter for a matching tag.
5. Search config docs by content.
   If tag search finds nothing, search the body text.
   Use the variable name or related terms.
6. Read the full content of the most relevant docs.
7. If monorepo and nothing found at project level,
   repeat steps 3 to 6 at root-level `docs/conf/`.
8. If still nothing found, search ADRs for config decisions.

## Apply what you find

- Every required variable must have a value before the service starts.
  Optional variables with defaults can stay unset.
- Use the Source field to find how to provide the value:

  - env variable: Set an environment variable.
  - config file: Write a config file at the expected path.
  - secret store: Configure access to the secret management system.
  - remote service: Make sure the service endpoint is reachable.
  - startup arg: Pass the value as a command-line argument.

- If a variable has a fallback source,
  the first source is primary.
  The fallback applies when the primary is empty.
- If a variable is marked as Sensitive,
  make sure it never appears in logs, errors, or debug output.
- For variables from remote services,
  check the refresh interval to know when values update.

## What to do when a variable is missing

If you search config docs and find nothing about a variable:

1. Check the service source code.
   The config loader in the service is the ground truth
   for what variables it reads.
2. Surface the gap.
   Show the user the variable name and the file location.
3. If the user agrees, use model-config to create or update the doc.

## Conflicts

If two config docs give conflicting definitions for the same variable,
show both to the user.
The more specific scope wins (project-level over root-level).
If both are at the same scope, the newer doc wins.
Let the user resolve the conflict.

## Superseded config docs

A config doc with this status is no longer current:

```
## Status

Superseded by NNNN-slug.md
```

Do not follow it for new deployments.
Read the superseding doc for the current variable definitions.