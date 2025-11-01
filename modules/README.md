# Modules Overview

## 1. Concept of a Module

In this architecture, a **module** represents a **business domain feature** — a vertical slice of functionality that encapsulates all the layers required to serve a specific purpose.

For example:
- The **User** module owns all logic related to user registration, profile management, and roles.
- The **Product** module owns catalog management, pricing, and inventory.
- The **Order** module coordinates checkout, payments, and shipping.

Each module is designed to be:
- **Self-contained** — owns its entities, logic, and data access patterns.
- **Replaceable** — can evolve independently, or even be extracted as a microservice later.
- **Isolated** — communicates with other modules through defined interfaces (application layer or APIs).

---

## 2. Purpose in the Modular Monolith

Modules give **boundaries** to your codebase inside a monolith, allowing separation by **business domain** rather than by technical layer only.

Instead of one large set of domain models or services shared by all features, modules let you design **independent subdomains**, e.g.:

```
modules/
├── user/
├── auth/
├── product/
└── order/
```
Each can internally follow the same layered architecture:
```
domain → application → infrastructure → presentation
```

---

## 3. Benefits of the Modular Design

| Benefit | Description |
|----------|--------------|
| **High cohesion** | Related logic stays together; a single module owns a single responsibility. |
| **Low coupling** | Modules depend on shared abstractions instead of each other directly. |
| **Scalable architecture** | Each module can be extracted into a microservice with minimal friction. |
| **Parallel development** | Teams can work independently on modules without merge conflicts. |
| **Clear ownership** | Each module defines clear APIs and boundaries, preventing accidental dependencies. |

---

## 4. Inter-Module Communication Rules

Modules should **not directly import code** from each other.  
Instead, they should communicate through **interfaces** or **the application layer**.

### Allowed
- Application layer uses repository or service traits exposed by another module’s **public API**.
- Events or messages (e.g., domain events, command handlers).

### Forbidden
- Direct access to another module’s entities or infrastructure implementations.
- Cross-importing private domain logic.

---

## 5. Directory Layout

### Dedicated `/modules/` directory
Ideal for modular monoliths aiming to migrate to microservices.
```
/modules/
├── user/
│ ├── domain/
│ ├── application/
│ ├── infrastructure/
│ ├── presentation/
│ └── README.md
├── auth/
│ ├── ...
```

Each module can be treated as its own crate (with a `Cargo.toml`), depending on the global workspace crates (`common`, `domain`, `application`, etc.).

---

## 6. Dependency Rules

Each module adheres to the same dependency principles as global layers:

| Layer | Depends On | Example in Module |
|--------|-------------|-------------------|
| **Domain** | common | Entities, value objects |
| **Application** | domain, common | Use cases, orchestrators |
| **Infrastructure** | domain, application, common | Database, adapters |
| **Presentation** | application, common | DTOs, mapping, handlers |

Modules **never** depend directly on each other’s internals — only on stable abstractions or APIs.

---

## 7. Naming Conventions

| Component | Convention | Example |
|------------|-------------|----------|
| Module name | lowercase, singular | `user`, `product`, `order` |
| Crate name | `modulename-layer` (if separate crates) | `user-domain`, `product-application` |
| Entities | PascalCase | `User`, `Product`, `Order` |
| Traits | PascalCase + `Repository` or `Service` suffix | `UserRepository`, `AuthService` |
| Folder names | plural, lowercase | `entities/`, `repositories/`, `handlers/` |

---

## 8. Migration Path to Microservices

Because modules are isolated and layered, it's possible to extract any module into its own service by:

1. Moving the module folder into a new repository.
2. Keeping its internal layers intact.
3. Replacing in-process communication with HTTP or messaging.

This makes your modular monolith a **microservice-ready architecture** without early complexity.

---

## 9. Suggested Core Modules for an E-commerce

| Module | Description | Dependencies |
|--------|--------------|---------------|
| **user** | Registration, profile, roles | common |
| **auth** | Authentication, JWT, sessions | user, common |
| **product** | Catalog, categories, inventory | common |
| **order** | Cart, checkout, order state | user, product, payment |
| **payment** | Payments and transactions | order |
| **shipping** | Shipping, delivery, tracking | order |
| **notification** | Email, SMS, push | user, order |
| **analytics** | Reports, stats, dashboards | order, product |

---

## 10. Summary

**Modules** are the backbone of business separation in the Rust e-commerce architecture.  
They make the codebase scalable, maintainable, and microservice-ready — while still reaping the benefits of a single deployable binary during early development.

> Treat each module as an autonomous domain — not just a folder.  
> The goal: isolate business responsibilities, not just code.





