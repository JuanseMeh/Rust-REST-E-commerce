# domain

The `domain` crate defines the **core business logic** of the e-commerce platform.  
It contains **entities**, **value objects**, and **traits** representing domain contracts.

## Responsibilities
- Define domain models (`User`, `Product`, `Order`, etc.)
- Specify repository traits and domain services
- Contain business invariants and validation rules

## Dependencies
- Depends on: `common`
- Must NOT depend on: `application`, `infrastructure`, `presentation`

## Structure
```
src/
├── entities/
├── value_objects/
├── traits/
└── lib.rs
```

## Design Rules
- No framework, database, or API code.
- No side effects or I/O.
- Pure business logic only.