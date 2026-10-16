# Sequence Diagrams

A sequence diagram shows messages between participants over time. Use it for API flows, auth flows, and service calls.

## Participants

```
sequenceDiagram
    actor User
    participant API
    participant DB
```

`actor` is a person. `participant` is a system.

## Messages

| Arrow | Code | Meaning |
|---|---|---|
| solid | `->>` | request |
| dotted | `-->>` | response |
| open | `-)` | async message |
| cross | `-x` | failure, delete |

## Activation

Show processing periods with `+` and `-`:

```
sequenceDiagram
    Client->>+Server: Request
    Server->>+DB: Query
    DB-->>-Server: Data
    Server-->>-Client: Response
```

## Blocks

```
sequenceDiagram
    participant User
    participant API

    alt Valid credentials
        API-->>User: 200 OK
    else Invalid
        API-->>User: 401
    end

    loop Every 5 seconds
        API-->>User: Ping
    end

    opt Debug mode
        API-->>User: Trace
    end
```

## Notes

```
sequenceDiagram
    Note over User,API: Login attempt
```

## Example: Login

```
sequenceDiagram
    participant User
    participant API
    participant Database

    User->>API: POST /login
    API->>Database: Query credentials
    Database-->>API: Return user data

    alt Valid credentials
        API-->>User: 200 OK + JWT
    else Invalid credentials
        API-->>User: 401 Unauthorized
    end
```

## Rules

1. Declare every participant before the first message.
2. Name each message with a colon and a short verb. Example: `API-->>User: 200 OK`.
3. Use `-->>` for a response.