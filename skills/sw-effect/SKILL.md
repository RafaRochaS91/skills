---
name: sw-effect
description: Enforce Effect.ts conventions for error modeling and composition, aligned with DDD feature-based architecture. Use after sw-architecture to layer functional error handling onto domain and application layers.
---
# SW Effect

Apply Effect.ts conventions across domain and application layers. This skill assumes the project already follows the `sw-architecture` structure.

## The Effect Type

```ts
Effect<Success, Error, Requirements>
```

- `Success` — value produced on completion
- `Error` — expected, recoverable domain failures (use `never` for infallible effects)
- `Requirements` — contextual service dependencies (use `never` in domain layer)

## Layer Rules

### Domain Layer — `Effect<A, DomainError, never>`

- No Requirements. Domain effects are self-contained.
- All failure modes are tagged discriminated unions — one type per failure mode.
- Never `throw`. All errors flow through the `E` channel.

```ts
type UserError =
  | { _tag: "UserNotFound"; id: string }
  | { _tag: "EmailAlreadyTaken"; email: string }
```

- Use `Effect.fail({ _tag: "UserNotFound", id })` — never `new Error(...)`.
- Factory methods and invariant checks return `Effect<Entity, DomainError, never>`.

### Application Layer — `Effect<A, AppError, ServiceDeps>`

- Use cases are written with `Effect.gen` for readable sequential composition.
- Dependencies (repositories, services) are declared as Requirements and resolved at the entry point.
- Never resolve Requirements inside domain layer.

```ts
const createUser = (input: CreateUserInput) =>
  Effect.gen(function* () {
    const repo = yield* UserRepository
    const user = yield* User.create(input)
    yield* repo.save(user)
    return user
  })
```

- Return `Effect<T, DomainError, never>` from use cases after dependency resolution.

### Infrastructure Layer — I/O Boundaries

- Wrap all external I/O with `Effect.tryPromise` or `Effect.try`.
- Map thrown exceptions to typed domain errors at the boundary — never let raw errors escape.

```ts
Effect.tryPromise({
  try: () => db.findById(id),
  catch: () => ({ _tag: "UserNotFound" as const, id }),
})
```

- Service implementations satisfy the Requirements interface declared in application layer.

## Composition Patterns

| Pattern | When to use |
|---|---|
| `Effect.gen` | Sequential composition with multiple steps |
| `Effect.map` | Transform success value, no new effects |
| `Effect.flatMap` | Chain dependent effects |
| `Effect.tap` | Side effects (logging) without changing value |
| `Effect.all` | Parallel independent effects |
| `Effect.orElse` | Fallback on failure |

## Error Handling Operators

| Operator | When to use |
|---|---|
| `catchTag` | Handle a specific tagged error — prefer this |
| `catchTags` | Handle multiple tagged errors at once |
| `catchAll` | Last resort; avoid — widens error type |
| `mapError` | Transform error type, enrich with context |
| `tapError` | Log or observe error without handling it |

- Prefer `catchTag` over `catchAll`. Blanket catches hide type information.
- Use `tapError` for logging before propagating.
- Never swallow errors silently.

## Execution (Entry Points Only)

- `Effect.runPromise` — async entry points (API handlers, CLI)
- `Effect.runSync` — synchronous entry points only; fails on async effects
- `Effect.runPromiseExit` — when you need detailed Exit information

Only execute at the outermost boundary. Never call `run*` inside domain or application layers.

## Testing

- Test effects by injecting test service implementations as Requirements.
- Assert on `Exit` values using `Effect.runPromiseExit` in tests.
- Do not mock at the module level — use the Effect Layer/Requirements system.

## Doc Verification (optional)

If invoked with `--verify-docs`, fetch the following pages and flag any patterns
in this skill that have drifted from the current Effect.ts API before proceeding:

- https://effect.website/docs/getting-started/the-effect-type
- https://effect.website/docs/error-management/expected-errors
- https://effect.website/docs/requirements-management/services

Run this periodically or after a major Effect.ts version bump — not on every invocation.

## Guardrails

- Never `throw` in domain or application layers.
- Never use generic `Error` as a domain error type — always tag.
- Never call `runSync` on effects that may be async.
- Never mix raw Promises and Effects — wrap at the infrastructure boundary.
- Sequential composition is not always correct — consider `Effect.all` for independent operations.
