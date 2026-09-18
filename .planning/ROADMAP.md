# Roadmap: MemQ Java 21 Upgrade & Modernization

## Overview

Single-phase mechanical upgrade: bump build to Java 21, apply behavior-preserving Java 21 language features and code-review improvements, add performance optimizations, verify with the existing test suite.

## Phases

- [ ] **Phase 1: Java 21 Upgrade & Modernization** - Upgrade build to 21, modernize code, optimize, verify no functional change

## Phase Details

### Phase 1: Java 21 Upgrade & Modernization
**Goal**: Build targets Java 21; code uses Java 21 features where safe; review/perf improvements applied; tests green with no functional changes
**Depends on**: Nothing (first phase)
**Requirements**: UPG-01, UPG-02, UPG-03, UPG-04, UPG-05
**Success Criteria** (what must be TRUE):
  1. All modules compile with `--release 21` (root POM `java.version=21`, memq-actor module POM updated)
  2. Public API and serialization behavior unchanged (public types keep Lombok @Value/@Jacksonized shapes; only internal classes become records)
  3. Existing test suite (33 tests) passes on JDK 21 with no test modifications to accommodate refactors
  4. Code review improvements applied with no functional change (dead code, redundant constructs)
  5. Performance improvements applied with no functional change
**Plans**: 3 plans

Plans:
- [ ] 01-01: Upgrade build to Java 21 and switch compiler config to --release 21
- [ ] 01-02: Apply Java 21 language features and code-review improvements (behavior-preserving)
- [ ] 01-03: Performance optimizations and final verification on JDK 21

## Progress

**Execution Order:**
Phases execute in numeric order: 1

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Java 21 Upgrade & Modernization | 0/3 | Not started | - |
