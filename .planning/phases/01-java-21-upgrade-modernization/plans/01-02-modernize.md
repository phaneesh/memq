# Plan 01-02: Java 21 language features + code review improvements

## Goal

Behavior-preserving modernization. Public API shapes (Lombok @Value/@Jacksonized with bean validation) untouched; only internal classes modernized.

## Tasks

1. **Records for internal immutable carriers** (package-private, not serialized):
   - `InternalMessage<M>` → record
   - `ObserverMessageMeta` → record (check usages: constructed in Actor, read via getters — record accessors used in Actor only; safe if callers updated)
   - `MetricKeyData`, `MetricData` → records (internal stats cache keys; `@Value`→record fine — check Map keys use equals/hashCode: Lombok @Value provides them; records do natively)
2. **RetryStrategyFactory**: `RetryConfig` already exposes type — replace unchecked casts with pattern-matching switch:
   `case CountLimitedExponentialWaitRetryConfig config -> ...` (Java 21 pattern matching for switch)
3. **Code review fixes**:
   - `RetryStrategy.policy` field not final → make final
   - `MessageMeta.deliveryAttempt` non-final → final
   - `Mailbox` constructor: missing `this.partition` param doc/unused warnings — leave; drop unused import `java.util.HashSet`? (check usage — used)
   - `Actor.process`: `else` after `return` — remove
   - `ActorMetricObserver.getMetricPrefix(String...)` varargs used with single arg — simplify (behavior identical)
   - `CommonUtils.isEmpty(Collection)` null-hostile + Guava `Preconditions` mixed — leave (public util)
   - `AsyncIsolatedThreadpoolDispatcher.deRegister`: `containsKey` + `get` + `remove` → single `computeIfPresent`/get-then-remove
   - `SyncDispatcher.registeredMailbox = new HashMap<>(partition)` — capacity arg misused as expected size; fine, leave or use LinkedHashMap — leave (perf-neutral)
   - `MemqActorBundle`: `private final List<ActorObserver> observers = new ArrayList<>()` fine
   - `ActorSystem.observers()` — minor; leave public interface untouched
4. **No test changes** to accommodate refactors (UPG-05). Tests referencing internals updated only if compile requires (e.g., record accessor names identical — check test usage of InternalMessage/ObserverMessageMeta accessors).

## Files

- memq-actor/src/main/java/io/appform/memq/actor/InternalMessage.java
- memq-actor/src/main/java/io/appform/memq/observer/ObserverMessageMeta.java
- memq-actor/src/main/java/io/appform/memq/stats/MetricKeyData.java
- memq-actor/src/main/java/io/appform/memq/stats/MetricData.java
- memq-actor/src/main/java/io/appform/memq/retry/RetryStrategyFactory.java
- memq-actor/src/main/java/io/appform/memq/retry/RetryStrategy.java
- memq-actor/src/main/java/io/appform/memq/actor/MessageMeta.java
- memq-actor/src/main/java/io/appform/memq/actor/Actor.java
- memq-actor/src/main/java/io/appform/memq/actor/AsyncIsolatedThreadpoolDispatcher.java
- memq-actor/src/main/java/io/appform/memq/stats/ActorMetricObserver.java
- callers of changed accessors (Actor.java, Mailbox.java, Dispatcher.java, tests if needed)

## Verification

- `mvn test` green, 33 tests, no behavioral test edits
