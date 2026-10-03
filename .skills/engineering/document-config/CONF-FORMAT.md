# Config Document Format

Config documents live in `docs/conf/`.
Use sequential numbering: `0001-slug.md`, `0002-slug.md`.

Create the `docs/conf/` directory lazily.
Create it only when you need the first config document.

## Frontmatter

```yaml
---
tags:
  - config
  - runtime
---
```

Every config document must have at least one tag.
See TAGS-FORMAT.md for the TAGS file rules.

## Template

```markdown
# {Service} System Variables

The external variables that drive behavior for this service.

## `VARIABLE_NAME`

- Type: string | number | boolean | enum
- Required: yes | no
- Default: {value} (omit if required)
- Source: env | file | arg | vault | http | secret-backend
- Description: What this variable controls.
- Constraints: Format rules, value ranges, required patterns.

Free-form narrative goes here.
Describe how the variable loads, who manages it,
when it refreshes, or any dependencies.
Keep it short. Omit it when the properties alone are enough.

## `ANOTHER_VARIABLE`
...
```

That is the template.
A config document defines system variables.
Each variable has structured properties.
It can also have free-form narrative.

## Optional sections

Add these only when they add genuine value.

- **Status.** `active` or `superseded by NNNN-slug.md`.
  Use when a doc is fully replaced.
- **See also.** Links to ADRs, related config docs, or patterns.

## Variable properties

| Property | Required | Content |
|---|---|---|
| Type | yes | string, number, boolean, enum, or structured type |
| Required | yes | yes or no |
| Default | no | Value when unset. Omit if Required is yes. |
| Source | yes | Where the value comes from |
| Description | yes | What this variable controls. One sentence. |
| Constraints | no | Format rules, value ranges, required patterns. |
| Values | no | Allowed values for an enum type. |

## Source categories

| Source | Meaning |
|---|---|
| env variable | Set as an environment variable before the process starts |
| config file | Read from a file on disk at startup |
| secret store | Read from a secret management system such as Vault or AWS SSM |
| remote service | Query an external service at startup or at runtime |
| startup arg | Passed as a command-line argument |