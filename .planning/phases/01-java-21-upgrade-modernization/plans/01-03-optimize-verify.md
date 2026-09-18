# Plan 01-03: Performance optimizations + final verification

## Goal

Behavior-preserving perf improvements, then full verification on JDK 21.

## Tasks

1. `Dispatcher.dispatch()` default method — hot path, runs per dispatch tick per partition:
   - Current: stream keySet limit → collect toSet; Sets.difference → Set.copyOf; then stream map get → toList. Allocates several intermediate collections.
   - Optimize: iterate `mailbox.getMessages()` keySet with a bounded loop, skip ids already in inFlight, add to inFlight directly, collect messages in one pass with ArrayList sized to needed. Avoid Guava `Sets.difference` (builds view + copy).
   - Keep semantics: ordered selection (LinkedHashMap order), max concurrency cap, no dispatch when nothing new.
2. `Actor.publish()` — `mailboxes.get(partitioner.applyAsInt(message))` fine. Leave.
3. `ActorMetricObserver.getMetricPrefix(String...)` — avoid varargs array alloc on hot path if trivially inlinable; measure by simplicity: replace with fixed-arg private method (behavior identical).
4. `Mailbox` — nothing hot beyond lock; leave.
5. Final: `mvn clean verify` (or test) on JDK 21; confirm 33 tests green; update ROADMAP/STATE progress; commit.

## Files

- memq-actor/src/main/java/io/appform/memq/actor/Dispatcher.java
- memq-actor/src/main/java/io/appform/memq/stats/ActorMetricObserver.java (if touched in 01-02, may already be done)

## Verification

- `JAVA_HOME=~/.sdkman/candidates/java/21.0.12-graal mvn clean test` → BUILD SUCCESS, 33 tests, 0 failures
