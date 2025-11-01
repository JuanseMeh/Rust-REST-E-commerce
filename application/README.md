# application

The `application` crate coordinates **use cases and business operations**, implementing logic that orchestrates the domain layer.

## Responsibilities
- Implements **commands**, **queries**, and **services**
- Coordinates multiple domain entities
- Enforces business workflows and transactions

## Dependencies
- Depends on: `domain`, `common`
- Must NOT depend on: `infrastructure`, `presentation`

## Structure
```
src/
├── services/
├── commands/
├── queries/
└── lib.rs
```

## Design Rules
- No database or HTTP code.
- Works with abstract repository traits from `domain`.