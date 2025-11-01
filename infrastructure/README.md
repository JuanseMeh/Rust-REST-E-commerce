# infrastructure

The `infrastructure` crate provides **technical implementations** and **adapters** for external systems such as databases, APIs, and external services.

## Responsibilities
- Implement repository traits using SQLx, etc.
- Manage data persistence, caching, messaging, and external APIs.
- Provide adapters for domain/infrastructure boundaries.

## Dependencies
- Depends on: `domain`, `application`, `common`
- Must NOT depend on: `presentation`, `api`

## Structure
```
src/
├── db/
├── repositories/
├── external/
└── lib.rs
```

## Design Rules
- Should implement interfaces defined in `domain`.
- Should not contain business logic.
