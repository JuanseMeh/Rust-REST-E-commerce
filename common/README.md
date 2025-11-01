# common

The `common` crate provides **cross-cutting utilities and foundational abstractions** shared across all layers and modules of the Rust E-commerce system.

It serves as the **shared kernel** of the architecture — containing **non-domain-specific components** such as configuration, error handling, and general-purpose utilities.

---

## Purpose

This crate acts as the **lowest-level dependency** in the workspace graph.  
All other crates (domain, application, infrastructure, modules, etc.) may depend on `common`, but `common` itself must **not** depend on any internal crates.

This design ensures:
- No circular dependencies.
- A consistent, centralized foundation for shared types and helpers.
- A clean separation between cross-cutting concerns and domain logic.

---

## Directory Structure

```
common/
├── Cargo.toml
└── src/
    ├── config/
    │   ├── mod.rs
    │   └── settings.rs
    ├── errors/
    │   ├── mod.rs
    │   └── app_error.rs
    ├── events/
    │   ├── mod.rs
    │   └── domain_event.rs
    ├── types/
    │   ├── mod.rs
    │   └── id.rs
    ├── utils/
    │   ├── mod.rs
    │   └── time.rs
    └── lib.rs
```

---

## Dependencies

Declared in `Cargo.toml`:

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
uuid = "1"
chrono = { version = "0.4", features = ["serde"] }
dotenv = "0.15"
```

These dependencies support configuration management, serialization, unique identifiers, and time utilities.

---

## Module Responsibilities

| Module | Purpose |
|---------|----------|
| `config` | Manages global application configuration and environment variables. |
| `errors` | Defines unified application errors and result types used across layers. |
| `events` | Provides base abstractions for domain and integration events. |
| `types` | Contains common primitive wrappers (e.g. `UserId`, `OrderId`) to ensure type safety. |
| `utils` | Shared helper functions for time, formatting, conversions, and validation. |

---

## Design Rules

- `common` **must not** import any internal workspace crates.  
- Should depend only on **stable, general-purpose external crates**.  
- Avoid adding business logic — this is purely a **technical foundation**.  
- Public exports should remain minimal and consistent (`pub use` only what’s necessary).

---

## Example Usage (Conceptual)

```rust
use common::config::AppConfig;
use common::errors::AppError;
use common::types::UserId;
use common::utils::now_utc;

fn example() -> Result<(), AppError> {
    let config = AppConfig::load()?;
    println!("Running in env: {}", config.environment);
    
    let user_id = UserId::new_v4();
    println!("New user: {}", user_id);
    
    println!("Current UTC time: {}", now_utc());
    Ok(())
}
```

> This is a conceptual example — implementation details may vary as modules evolve.

---

## Role in the Overall Architecture

```
domain <── application <── infrastructure <── presentation <── api
        ↑
        │
      common (shared foundation)
```

The `common` crate underpins all higher layers, ensuring consistency across configuration, error handling, and shared types, without introducing coupling between business modules.

---

## Summary

- Provides **cross-cutting, non-domain logic**.  
- Acts as the **foundation crate** for all others.  
- Keeps the architecture **clean, consistent, and decoupled**.  
- Designed for **reuse and minimal dependency footprint**.  

---

© 2025 Rust E-commerce Modular Monolith — Common Crate Documentation