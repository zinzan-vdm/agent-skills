# Entity Relationship Diagrams (ERD)

An ERD models a database schema. Use it for tables, keys, and relations.

## Shape

```
erDiagram
    CUSTOMER ||--o{ ORDER : places
```

## Entities and Attributes

```
erDiagram
    CUSTOMER {
        int id PK
        string email UK
        string name
        datetime created_at
    }
```

Attribute format: `type name constraints`.

## Constraints

| Code | Meaning |
|---|---|
| `PK` | primary key |
| `FK` | foreign key |
| `UK` | unique key |
| `NN` | not null |

## Cardinality

Crow's foot tokens, left side:

```
||    exactly one
|o    zero or one
}o    zero or more
}|    one or more
```

Right side mirrors: `||`, `o|`, `o{`, `|{`.

## Example: Shop Schema

```
erDiagram
    CUSTOMER ||--o{ ORDER : places
    ORDER ||--|{ LINE_ITEM : contains
    PRODUCT ||--o{ LINE_ITEM : includes

    CUSTOMER {
        int id PK
        string email UK
        string name
    }

    ORDER {
        int id PK
        int customer_id FK
        decimal total
    }

    LINE_ITEM {
        int id PK
        int order_id FK
        int product_id FK
        int quantity
    }

    PRODUCT {
        int id PK
        string name
        decimal price
    }
```

## Rules

1. Name entities in uppercase. Use underscore between words.
2. Put a key on every entity.
3. Use `||` on the "one" side of a relation.
4. Use `}|` on the "many" side of a relation. The token `}{` is not in the Mermaid docs. It breaks the diagram.