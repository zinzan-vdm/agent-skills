# CONTEXT.md Format

## Structure

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Language

**Order**:
{A one or two sentence description of the term}
_Avoid_: Purchase, transaction

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account
```

## Rules

- **Be opinionated.** When multiple words exist for the same concept, pick the best one. List the others under `_Avoid_`.
- **Keep definitions tight.** Use one or two sentences maximum. Define what it IS. Do not define what it does.
- **Only include project-specific terms.** General programming concepts do not belong. Timeouts, error types, and utility patterns are not project-specific. Do not add them. Before you add a term, ask: is this a concept unique to this context? If it is a general concept, skip it.
- **Group terms under subheadings** when natural clusters emerge. If all terms form one group, a flat list is fine.

## Single vs multi-context repos

**Single context (most repos).** One `CONTEXT.md` at the repo root.

**Multiple contexts.** A `CONTEXT-MAP.md` at the repo root lists the contexts. It shows where they live and how they relate to each other:

```md
# Context Map

## Contexts

- [Ordering](./src/ordering/CONTEXT.md): receives and tracks customer orders
- [Billing](./src/billing/CONTEXT.md): generates invoices and processes payments
- [Fulfillment](./src/fulfillment/CONTEXT.md): manages warehouse picking and shipping

## Relationships

- **Ordering to Fulfillment**: Ordering emits `OrderPlaced` events. Fulfillment consumes them to start picking.
- **Fulfillment to Billing**: Fulfillment emits `ShipmentDispatched` events. Billing consumes them to generate invoices.
- **Ordering and Billing**: Shared types for `CustomerId` and `Money`.
```

The skill infers which structure applies:

- If `CONTEXT-MAP.md` exists, read it to find the contexts.
- If only a root `CONTEXT.md` exists, use single context.
- If neither exists, create a root `CONTEXT.md` lazily when you resolve the first term.

When multiple contexts exist, infer which one the current topic relates to. If it is not clear, ask.