# MemQ — Java 21 Upgrade & Modernization

## What This Is

MemQ is an in-memory partitioned actor system library (`memq-actor`) with a Dropwizard integration bundle (`memq-dw-bundle`). This effort upgrades the build to Java 21 and modernizes the code with Java 21 language features, code-review improvements, and performance optimizations — with zero functional changes, verified by the existing test suite.

## Core Value

All existing tests (33) pass unchanged on JDK 21 after modernization.

## Requirements

### Validated

- ✓ Partitioned in-memory actor system with sync/async dispatch — existing
- ✓ Retry strategies (failsafe-backed) with configurable limits — existing
- ✓ Exception handling (drop/sideline) — existing
- ✓ Metrics observer (Dropwizard Metrics) — existing
- ✓ Dropwizard bundle integration — existing

### Active

- [ ] UPG-01: Build compiles with Java 21 (`--release 21`) in all modules
- [ ] UPG-02: Java 21 language features applied (records, pattern matching for switch) where behavior-preserving
- [ ] UPG-03: Code review improvements (dead code, naming, redundant constructs) with no functional change
- [ ] UPG-04: Performance optimizations with no functional change
- [ ] UPG-05: Full test suite green on JDK 21 with no test modifications to accommodate refactors

### Out of Scope

- Virtual threads migration — behavior change to executor semantics, needs its own decision
- New features / API additions — this is a no-functional-change effort
- Further dependency upgrades beyond the already-staged Dropwizard 5 migration in the working tree

## Context

- Working tree already contains an uncommitted Dropwizard 2.1.10 → 5.0.2 + javax→jakarta migration (POMs, imports, tests). It is the baseline; tests pass green on JDK 21 with it.
- JDK 21 available at `~/.sdkman/candidates/java/21.0.12-graal`.
- Pre-existing local artifacts (`.project`, `.settings/`, `.classpath` etc.) are IDE noise; left alone.

## Key Decisions

| Decision | Rationale | Outcome |
| --- | --- | --- |
| Treat uncommitted DW5 migration as baseline | Tests pass green; it is the intended new state | Pending |
| Skip project research phase | Brownfield mechanical upgrade; code fully surveyed inline | Pending |
| Records only for internal immutable carriers | Public API uses Lombok @Value + @Jacksonized + validation annotations; converting risks serialization behavior changes | Pending |

---
*Last updated: 2026-09-18 after initialization*
