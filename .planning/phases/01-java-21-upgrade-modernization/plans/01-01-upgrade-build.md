# Plan 01-01: Upgrade build to Java 21

## Goal

All modules compile with Java 21 release semantics.

## Tasks

1. Root `pom.xml`: change `<java.version>17</java.version>` → `21`; keep `<release>${java.version}</release>` semantics — actually change compiler plugin config from source/target to `<release>` (more correct for cross-compilation).
2. `memq-actor/pom.xml`: remove stale `maven.compiler.source/target` properties (root manages compiler config).
3. Verify: `mvn clean test` green on JDK 21.

## Files

- pom.xml
- memq-actor/pom.xml

## Verification

- `JAVA_HOME=~/.sdkman/candidates/java/21.0.12-graal mvn clean test` → BUILD SUCCESS, 33 tests, 0 failures
