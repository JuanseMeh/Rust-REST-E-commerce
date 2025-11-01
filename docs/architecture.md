# Rust E-Commerce — Modular Monolith Architecture Blueprint

## Overview

This project implements a **modular monolith** following **Clean Architecture** (a.k.a. Hexagonal Architecture), designed for a **RESTful E-commerce platform**.

The goal is to build a **scalable, maintainable, and evolvable** codebase that can later transition to **microservices** without re-architecture.

---

## Architecture Layers

| Layer | Responsibility | Depends On |
|-------|----------------|-------------|
| **domain** | Core business entities, value objects, and repository traits. | none |
| **application** | Use cases and business logic orchestration (services). | domain |
| **infrastructure** | Technical implementations (DB, APIs, adapters, repositories). | domain |
| **presentation** | Serialization, DTOs, input validation, API layer contracts. | domain, application |
| **api** | REST entry point, routes, dependency wiring, bootstrapping. | all |

---

## Modules

Each business area is isolated as its own crate inside `/modules/`.  
This keeps concerns modular and allows future extraction into microservices.

| Module | Responsibility |
|--------|----------------|
| `users` | Account management, profiles, user data. |
| `auth` | Authentication, authorization, JWT, sessions. |
| `products` | Product catalog, categories, pricing. |
| `orders` | Cart, checkout, and order lifecycle. |
| `payments` | Payment gateways, transactions, refunds. |
| `inventory` | Stock control and product availability. |
| `reviews` | Product feedback, ratings. |
| `notifications` | Email, SMS, or push notifications. |

---

## Workspace Setup

### 1. Initialize the Project

```bash
cargo new rust_ecommerce --vcs git
cd rust_ecommerce
```

### 2. Convert to a Workspace

Edit `Cargo.toml` to look like this:

```toml
[workspace]
members = [
    "api",
    "application",
    "domain",
    "infrastructure",
    "presentation",
    "modules/users",
    "modules/auth",
    "modules/products",
    "modules/orders",
    "modules/payments",
    "modules/inventory",
    "modules/reviews",
    "modules/notifications"
]
```

---

## Project Structure

```
rust_ecommerce/
├── Cargo.toml
├── api/
│   ├── Cargo.toml
│   └── src/main.rs
├── application/
│   └── src/lib.rs
├── domain/
│   └── src/lib.rs
├── infrastructure/
│   └── src/lib.rs
├── presentation/
│   └── src/lib.rs
└── modules/
    ├── users/
    │   └── src/lib.rs
    ├── auth/
    │   └── src/lib.rs
    ├── products/
    │   └── src/lib.rs
    ├── orders/
    │   └── src/lib.rs
    ├── payments/
    │   └── src/lib.rs
    ├── inventory/
    │   └── src/lib.rs
    ├── reviews/
    │   └── src/lib.rs
    └── notifications/
        └── src/lib.rs
```

---

## Module Architecture: Internal Design

### Overview

Each module under `/modules/` represents a **bounded context** of the e-commerce domain (e.g., `users`, `products`, `orders`).  
Even though this is a **modular monolith**, each module should be **independently understandable**, with its own mini-layered structure.

---

### Module Folder Layout

```
modules/
└── users/
    ├── src/
    │   ├── domain/
    │   ├── application/
    │   ├── infrastructure/
    │   ├── presentation/
    │   └── lib.rs
    ├── Cargo.toml
    └── README.md
```

---

### Layer Responsibilities (Inside Each Module)

#### 1. domain/
Defines the **core business logic**: entities, value objects, and repository traits.  
Has **no external dependencies**.

#### 2. application/
Coordinates business logic using the domain layer.  
Contains **commands**, **queries**, and **services**.  
Depends only on `domain/`.

#### 3. infrastructure/
Implements repositories, persistence, and integrations.  
Depends on `domain/` and external crates like SQLx.

#### 4. presentation/
Provides routes, controllers, DTOs, and serializers.  
Depends on `application/` for executing use cases.

---

## Common / Shared Layer Design

### Purpose
`common/` contains **shared cross-cutting utilities** like configuration, errors, and events.  
Every layer can depend on `common`, but `common` depends on no one.

### Structure

```
common/
├── config/
├── errors/
├── utils/
├── types/
└── events/
```

### Components Overview

| Crate | Purpose |
|-------|----------|
| `common/config` | App configuration and environment management |
| `common/errors` | Unified error system |
| `common/utils` | Pure helper utilities |
| `common/types` | Shared type-safe structures |
| `common/events` | Domain & integration event abstractions |

---

# Philosophy & Design Principles

## Core Concepts

### Clean Architecture
- **Dependency Inversion:** High-level policies (domain, application) are independent of low-level details (DB, web).  
- **Testability:** Each layer is isolated, enabling focused unit testing.  
- **Framework Independence:** The domain is not coupled to frameworks or libraries.

### Hexagonal Architecture (Ports & Adapters)
- **Ports:** Abstract boundaries defined in the domain (traits).  
- **Adapters:** Concrete implementations in infrastructure.  
- Promotes **replaceable technologies** — e.g., swap PostgreSQL for MongoDB without touching domain logic.

### Modular Monolith
- Encourages **module isolation** while remaining a single deployable unit.  
- Each module behaves like a **mini-service**, owning its data and logic.  
- Easy migration path: extract modules to independent microservices later.

### Domain-Driven Design Influence
- **Bounded Contexts:** Modules align with distinct business areas.  
- **Ubiquitous Language:** Each module reflects clear, business-oriented terminology.  
- **Entities & Value Objects:** Business data and rules are explicit and well-typed.

---

## Benefits of This Architecture

-  High **cohesion** within modules, low **coupling** between them.  
-  Clear boundaries and dependency flow.  
-  Scalable toward microservices with minimal rewrite.  
-  Maintainable, testable, and easy to onboard new developers.  
-  Supports event-driven expansion.  

---

## Final Structure Summary

```
rust_ecommerce/
├── api/
├── application/
├── domain/
├── infrastructure/
├── presentation/
├── common/
└── modules/
```

---

## Conclusion

This blueprint provides a **production-ready architectural foundation** for Rust-based enterprise systems.  
By following these guidelines, your project remains modular, clean, and adaptable — from startup monolith to scalable distributed architecture.