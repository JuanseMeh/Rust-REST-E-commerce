# api

The `api` crate serves as the **entry point** for the e-commerce system, exposing a **REST API**.

## Responsibilities
- Define and register all HTTP routes
- Initialize app state, dependency injection, and middleware
- Integrate modules and bootstrap the server

## Dependencies
- Depends on: `presentation`, `application`, `infrastructure`, `common`, and any modules.
- Should not contain business logic — only wiring.

## Structure
```
src/
├── main.rs
├── routes/
├── middlewares/
└── startup.rs
```

## Design Rules
- No direct domain logic.
- Keep routes thin; delegate to `presentation` or `application`.