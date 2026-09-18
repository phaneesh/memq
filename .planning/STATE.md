# Project State

## Project Reference

See: .planning/PROJECT.md (updated 2026-09-18)

**Core value:** All existing tests pass unchanged on JDK 21 after modernization
**Current focus:** Phase 1 - Java 21 Upgrade & Modernization

## Current Position

Phase: 1 of 1 (Java 21 Upgrade & Modernization)
Plan: 0 of 3 in current phase
Status: Ready to execute
Last activity: 2026-09-18 - Project initialized (auto mode, brownfield, inline survey)

Progress: [░░░░░░░░░░] 0%

## Accumulated Context

### Decisions

- Working tree contains uncommitted Dropwizard 5.0.2 + jakarta migration; treated as baseline (tests green on JDK 21)
- Records only for internal immutable carriers; public @Value/@Jacksonized types left as-is (serialization safety)
- Virtual threads out of scope

### Session Context

- Baseline test run: 33 tests (31 memq-actor + 2 memq-dw-bundle), 0 failures, JDK 21.0.12
- JDK 21 at ~/.sdkman/candidates/java/21.0.12-graal; default shell java is 25 — build must export JAVA_HOME
