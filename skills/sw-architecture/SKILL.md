---
name: sw-architecture
description: Apply personal software architecture preferences to a project. Use when starting a new project, auditing existing structure, or generating a project CLAUDE.md. Enforces DDD feature-based modules, TypeScript strict conventions, and minimal complexity.
---
# SW Architecture

Apply the following architectural conventions to this project.

## Module Structure

Every feature follows this layout:

```
src/
  <feature>/
    domain/         — entities, value objects, invariants (zero external deps)
    application/    — use cases, commands, queries
    infrastructure/ — DB adapters, API clients, external integrations
  shared/           — Result type, base errors, primitives
  common/           — auth guards, middleware, cross-cutting concerns
```

## TypeScript Rules

- `"strict": true` always on in tsconfig.
- No `any`. No non-null assertions (`!`) except where unavoidable with third-party types.
- Prefer discriminated unions over optional fields when modeling domain states.
- Explicit return types on all public functions and domain methods.
- Relax type strictness only in test and config files.

## Domain Layer

- Entities encapsulate invariants. Validation lives in the constructor or a static factory.
- Value objects are immutable, compared by value not reference.
- No framework imports. Zero external dependencies.
- Model failure states as discriminated unions — not nullable or optional fields.

## Application Layer

- One use case per file. Logic is orchestration only.
- Commands mutate state. Queries read state. Never mix.
- Never throw for domain errors — always return a typed result.

## Infrastructure Layer

- Wraps all external I/O (DB, APIs, filesystem).
- Maps external errors to typed domain errors at the boundary.
- No domain logic here.

## Testing

- Always unit test domain logic: entities, value objects, pure functions.
- Integration/E2E tests for critical flows and system boundaries — only when asked.
- Do not unit test infrastructure adapters; use integration tests.

## General Guardrails

- Minimum complexity for the current task. No speculative abstractions.
- No docstrings, comments, or type annotations on code you didn't touch.
- No backwards-compatibility shims for removed code.
- Prefer editing existing files over creating new ones.
- Read existing code before proposing changes. Confirm conventions from files, not assumptions.

---

## Task

Based on context, do one of the following:

1. **New project** — Scaffold the folder structure and generate a `CLAUDE.md` capturing these conventions for the specific stack in use.
2. **Existing project** — Audit the current structure against these conventions. Report deviations with concrete fix suggestions. Do not make changes until approved.
3. **Code review** — Evaluate a module or diff against these rules and flag violations.

---

## Follow-up

After completing the above, ask the user:

> This project uses typed error handling. Would you like me to apply the **sw-effect** skill to enforce Effect.ts conventions for error modeling and composition across domain and application layers?
