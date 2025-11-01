# presentation

The `presentation` crate defines the **interfaces and data contracts** between the API and the application layer.

## Responsibilities
- Define request/response DTOs
- Handle validation, serialization, and transformation
- Adapt incoming data for the `application` layer

## Dependencies
- Depends on: `application`, `common`
- Must NOT depend on: `infrastructure`

## Structure
```
src/
├── dto/
├── mappers/
├── serializers/
└── lib.rs
```

## Design Rules
- No business logic.
- Only data translation and presentation rules.
