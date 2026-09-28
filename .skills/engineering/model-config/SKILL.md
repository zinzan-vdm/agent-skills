---
name: model-config
description: Model config fields in docs/conf/. Use for config design.
metadata:
  version: "1.0"
  author: Zinzan van der Merwe
---

# Model Config

Build and maintain the system variable definitions.
A system variable drives behavior from an external source.
You can change it without modifying the code.
It is a dial you can turn on the system.

This skill helps you document each variable, its type,
its source, and what it controls.

This skill does not cover config architecture decisions.
Put those in an ADR. Use model-domains for that.

## What is a system variable

A system variable is a value that comes from outside the code.
It affects behavior when you change it.

Examples of system variables:

- An environment variable that sets the log level.
- A database connection string from a secret store.
- A feature flag from a remote config service.
- A config file path that selects which plugin to load.
- A rate limit value from a startup config file.

These are NOT system variables:

- How config files are parsed. That is an ADR.
- Why you chose env vars over config files. That is an ADR.
- The internal data flow inside a service. That is code.

## File structure

Config docs live under `docs/conf/`.

```
docs/
└── conf/
    ├── TAGS
    └── 0001-billing-service-variables.md
```

Per-service or per-app config docs go under `<project>/docs/conf/`.

```
services/billing/docs/conf/
├── TAGS
├── 0001-billing-env-vars.md
└── 0002-billing-feature-flags.md
```

Create files lazily.
Create a file only when you have variables to document.
If `docs/conf/` does not exist, create it when you need the first file.
If `docs/conf/TAGS` does not exist,
create it when you write the first document with tags.

## Variable definition format

A config document defines a set of variables.

```
---
tags:
  - config
  - billing
---

# {Service} System Variables

The external variables that drive behavior for this service.

## `DATABASE_URL`

- Type: string
- Required: yes
- Source: env variable
- Description: Postgres connection string for the billing database.
- Constraints: Must be a valid postgres:// URL.

## `LOG_LEVEL`

- Type: enum
- Values: debug, info, warn, error
- Required: no
- Default: info
- Source: env variable
- Description: Controls the verbosity of the application log.
```

### Variable properties

| Property | Required | Content |
|---|---|---|
| Type | yes | string, number, boolean, enum, or a structured type |
| Required | yes | yes or no |
| Default | no | Value when unset. Omit if Required is yes. |
| Source | yes | Where the value comes from. See source categories below. |
| Description | yes | What this variable controls. One sentence. |
| Constraints | no | Format rules, value ranges, required patterns. |
| Values | no | List of allowed values for an enum type. |

### Free-form narrative

A variable can also have free-form narrative sections.
Add these when the loading, sourcing, management, or usage
needs more explanation than the properties can hold.

Common narrative topics:

- How the variable is loaded (boot time, lazy, on each request).
- Who or what manages the value (operator, CI pipeline, Terraform).
- When the value refreshes (once, every N seconds, on SIGHUP).
- Where the value resolves (Kubernetes Secret, Vault path, SSM parameter).
- Dependencies between this variable and other variables.

Keep the narrative short.
Remove it when the properties alone are enough.

Example with narrative:

```
## `DATABASE_URL`

- Type: string
- Required: yes
- Source: env variable
- Description: Postgres connection string for the billing database.
- Constraints: Must be a valid postgres:// URL.

This variable is loaded at boot time from the environment.
The value is managed by Terraform and injected by Kubernetes
from a Secret resource.
The connection string never changes while the process runs.
```

## Source categories

The Source field tells you where the variable comes from.

| Source | Meaning |
|---|---|
| env variable | Set as an environment variable before the process starts |
| config file | Read from a file on disk at startup |
| secret store | Read from a secret management system such as Vault or AWS SSM |
| remote service | Query an external service at startup or at runtime |
| startup arg | Passed as a command-line argument |

A variable can have more than one source.
List the first source that the system checks.
If the first source is empty, the system falls back to the next source.

Example:

```
- Source: env variable, falls back to config file
- Default: debug
```

## Structured variables

A variable can have a complex type with sub-variables.
Use a heading with the parent name.
Then list each sub-variable with its full path.

```
## `database`

Structured config block.
Sub-variables use the `DATABASE_` prefix for env vars.

### `database.host`
- Type: string
- Required: yes
- Source: env variable DATABASE_HOST or config file
- Description: The database server hostname.

### `database.port`
- Type: number
- Required: no
- Default: 5432
- Source: env variable DATABASE_PORT or config file
- Description: The database server port.
```

## Variables from remote services

When a variable comes from a remote service,
document the service endpoint and refresh behavior.

```
## `FEATURE_FLAGS`

- Type: object (JSON)
- Required: no
- Default: all features disabled
- Source: remote service (feature-flags.internal:8080)
- Description: Feature flag values for this service.
- Refresh interval: 60 seconds
```

## During the session

### Recognize when a variable needs a doc

Create or update a config document when one is true:

1. A new service has external variables.
   Document every one.
2. A variable is added to an existing service.
   Add it to the relevant document.
3. A variable changes its name, type, or source.
   Update the field in place.
4. A variable is deprecated.
   Mark it in the description.
   Remove it only after all consumers stop using it.

### Write a variable document

1. Is it a single-project repo or a monorepo?
2. If monorepo:
   - Cross-project variables go to root `docs/conf/`.
   - Per-service variables go to `<project>/docs/conf/`.
3. Write every variable the system reads from outside.
   Include optional variables with their defaults.
4. Use the format in this skill.
5. Add at least one tag. Use terms from CONTEXT.md or existing TAGS.
6. If the tag is new to `docs/conf/TAGS`, append it.

### Check variable docs against code

When you read code that reads external variables,
check that every one is documented.
If the code reads a variable not in the doc, surface the gap:

"The billing service reads a RATE_LIMIT_TOKENS variable from the environment.
This variable is not in docs/conf/.
Is this a new variable, or is the doc stale?"

If the code uses a variable differently from the doc,
the code or the doc is wrong. Surface the conflict.

### Mark superseded

When a variable changes, update it in place.
Do not create a new document. The number stays the same.

When a new document supersedes a whole document,
add a status section:

```
## Status

Superseded by services/new-billing/docs/conf/0001-billing-env-vars.md
```

Keep the old content. A reader may need it to understand old deployments.

### Config doc vs ADR

Config docs and ADRs have different jobs.

- ADR records why. Use an ADR for config architecture decisions.
  Example: "we load YAML with env var substitution at boot."
- Config doc records what. Use a config doc for the variables themselves.
  Example: "DATABASE_URL is a string from an env var."

Do not duplicate. If an ADR covers the approach,
the config doc can reference it with a See also line.