# Structural Architectural Patterns & Boundary Guidelines

Guidelines for establishing clean boundaries, directional dependencies, and module cohesion.

---

## 1. Boundary Design Principles

1. **Separation of Concerns**: Each module or package owns a distinct domain concern (e.g., domain entities vs. persistence adapters vs. transport handlers).
2. **Dependency Inversion Principle (DIP)**:
   - High-level policy modules must not depend on low-level detail modules. Both should depend on abstractions.
   - Core domain logic must never import database drivers, UI components, or network protocols directly.
3. **Single Direction of Dependencies (Acyclic Dependencies)**:
   - Component dependencies must form a Directed Acyclic Graph (DAG).
   - If Module A depends on Module B, Module B must never depend on Module A (directly or transitively). If a mutual dependency occurs, extract shared contracts into a common interface module.

---

## 2. Common Architectural Layering

```text
┌───────────────────────────────────────────────────────────┐
│ Presentation / Transport Layer                            │
│ (HTTP Handlers, CLI Commands, Event Listeners)            │
└─────────────────────────────┬─────────────────────────────┘
                              │ calls
                              ▼
┌───────────────────────────────────────────────────────────┐
│ Application / Use Case Layer                              │
│ (Workflow Orchestration, Service Coordinators)            │
└─────────────────────────────┬─────────────────────────────┘
                              │ executes
                              ▼
┌───────────────────────────────────────────────────────────┐
│ Domain / Entity Layer (Core Business Rules & Interfaces)  │
└─────────────────────────────▲─────────────────────────────┘
                              │ implements interfaces
┌─────────────────────────────┴─────────────────────────────┐
│ Infrastructure / Persistence Layer                        │
│ (SQL Repositories, Cloud Clients, File I/O)               │
└───────────────────────────────────────────────────────────┘
```

---

## 3. Boundary Trade-Off Evaluation

When deciding between monolithic vs. partitioned modules:
- **Too coarse-grained (Monolithic)**: Tight coupling, long build times, high risk of accidental side effects, unclear ownership.
- **Too fine-grained (Over-partitioned)**: High boilerplate, complex dependency trees, fragmented reasoning, difficult cross-module tracing.
- **Guideline**: Group by cohesion. Code that changes together for the same business reason belongs together in the same component.
