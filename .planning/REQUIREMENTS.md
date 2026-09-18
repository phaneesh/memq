# Requirements: MemQ Java 21 Upgrade

## v1 Requirements

### Upgrade

- [ ] **UPG-01**: All modules compile with `--release 21`; root POM `java.version=21`; `memq-actor` module POM compiler properties updated
- [ ] **UPG-02**: Java 21 features applied where behavior-preserving (records for internal carriers, pattern-matching switch where applicable)
- [ ] **UPG-03**: Code review improvements implemented (dead code removal, redundant constructs, naming) with no functional change
- [ ] **UPG-04**: Performance optimizations implemented (allocation reduction, idiomatic replacements) with no functional change
- [ ] **UPG-05**: `mvn test` green on JDK 21 across all modules with no behavioral test changes

## Out of Scope

- Virtual threads — changes executor semantics; separate decision needed
- API surface changes — public signatures/serialization must remain identical
- Further dependency upgrades beyond staged DW5 migration

## Traceability

| REQ | Phase |
| --- | --- |
| UPG-01 | 1 |
| UPG-02 | 1 |
| UPG-03 | 1 |
| UPG-04 | 1 |
| UPG-05 | 1 |
