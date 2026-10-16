# Flowcharts

A flowchart shows a process or a decision tree. Use it for workflows, algorithms, and user journeys.

## Direction

| Direction | Meaning |
|---|---|
| `TD` or `TB` | top to bottom |
| `BT` | bottom to top |
| `LR` | left to right |
| `RL` | right to left |

## Node Shapes

| Shape | Code | Use |
|---|---|---|
| rectangle | `A[text]` | default step |
| rounded | `B(text)` | soft step |
| stadium | `C([text])` | start, end |
| subroutine | `D[[text]]` | sub-process |
| database | `E[(text)]` | data store |
| circle | `F((text))` | connector |
| decision | `H{text}` | branch |
| hexagon | `I{{text}}` | preparation |
| parallelogram | `J[/text/]` | input, output |

## Edges

| Edge | Code |
|---|---|
| arrow | `-->` |
| line | `---` |
| dotted arrow | `-.->` |
| thick arrow | `==>` |
| labeled edge | `-->|text|` |
| circle end | `--o` |
| cross end | `--x` |

## Subgraph

```
flowchart TD
    subgraph checkout["Checkout"]
        A[Cart] --> B[Pay]
    end
    B --> C[Ship]
```

## Example: Login Flow

```
flowchart TD
    Start([User visits site]) --> Auth{Authenticated?}
    Auth -->|No| Login[Show login page]
    Auth -->|Yes| Dashboard[Show dashboard]
    Login --> Creds[Enter credentials]
    Creds --> Validate{Valid?}
    Validate -->|Yes| Dashboard
    Validate -->|No| Error[Show error]
    Error --> Login
```

## Rules

1. Max 15 nodes. Split larger flows into subgraphs or into several diagrams.
2. Name the edges that carry meaning.
3. Use `%%` for comments.