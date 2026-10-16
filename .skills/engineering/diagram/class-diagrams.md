# Class Diagrams

A class diagram models classes, their members, and their relationships. Use it for domain models and object design.

## Classes and Members

```
classDiagram
    class BankAccount {
        +String owner
        +Decimal balance
        -String accountNumber
        +deposit(amount)
        +withdraw(amount)
    }
```

## Visibility

| Code | Meaning |
|---|---|
| `+` | public |
| `-` | private |
| `#` | protected |
| `~` | package |

## Relationships

| Code | Meaning | Example |
|---|---|---|
| `--` | association | `Customer -- Order` |
| `o--` | aggregation | `Playlist o-- Song` |
| `*--` | composition | `Order *-- LineItem` |
| `<|--` | inheritance | `Animal <|-- Dog` |
| `<..` | dependency | `Service <.. Config` |
| `<|..` | realization | `Drawable <|.. Circle` |

## Labels and Multiplicity

```
classDiagram
    Customer "1" --> "0..*" Order : places
    Order "1" *-- "1..*" LineItem : contains
```

`*` means many. `0..*` means zero or many. `1..*` means one or many.

## Stereotypes and Abstract Members

```
classDiagram
    class IRepository {
        <<interface>>
        +save(entity)
    }
    class Shape {
        <<abstract>>
        +draw()*
    }
    Shape <|-- Circle
```

The `*` after `draw()` marks an abstract method.

## Generics

```
classDiagram
    class List~T~ {
        +add(item: T)
        +get(index: int) T
    }
    List~String~ <-- StringProcessor
```

## Example: E-Commerce Domain

```
classDiagram
    Customer "1" --> "0..*" Order : places
    Order "1" *-- "1..*" LineItem : contains
    LineItem "1" --> "1" Product : references

    class Customer {
        +String email
        +placeOrder(cart) Order
    }

    class Order {
        +Decimal total
        +ship()
        +cancel()
    }

    class LineItem {
        +int quantity
        +Decimal pricePerUnit
    }

    class Product {
        +String name
        +Decimal price
        +isAvailable() bool
    }

    class OrderStatus {
        <<enumeration>>
        PENDING
        PAID
        SHIPPED
        CANCELLED
    }

    Order --> OrderStatus
```

## Rules

1. Declare a class before you use it in a relationship.
2. Show only the members that matter.
3. `*--` means the child cannot exist without the parent.