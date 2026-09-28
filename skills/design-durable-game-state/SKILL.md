---
name: design-durable-game-state
description: Design, implement, migrate, or audit durable game state with canonical entity IDs, deterministic transitions, versioned save schemas, safe autosaves, and rebuildable derived data. Use when adding quests, cities, characters, inventory, relationships, world history, save/load, migrations, replays, or any feature whose references must survive renames and content changes.
---

# Design Durable Game State

The cheapest time to establish identity and persistence invariants is before
content multiplies. A prototype may use names and array indexes for a week; a
game can spend months unwinding the resulting ambiguity.

## Canonical IDs everywhere

Every durable domain entity receives a stable canonical ID when created:

- locations, actors, vehicles, groups, quests, discoveries, items, contracts,
  incidents, world objects, and player-created entities;
- relationships, indexes, caches, selections, telemetry, and save references
  point to that ID; and
- duplicate IDs and unresolved references reject construction or load
  activation. Production preserves the original save and reports the invariant
  rather than crashing or guessing.

Never use display names, localized text, labels, coordinates, array positions,
sprite slots, or incidental object keys as identity. Coordinates may identify a
spatial observation only when location itself is the fact; document that
exception.

Keep identity separate from ownership, position, presentation, membership,
office, allegiance, and historical record. A rename, relocation, conquest,
translation, world-resolution change, or catalog reorder must not change who or
what the entity is.

Read [references/canonical-identity.md](references/canonical-identity.md) when
introducing or repairing an entity model.

## Make transitions explicit

Domain operations should validate preconditions, compute the complete result,
and commit atomically. Do not let UI code approximate eligibility while a
mutation function enforces a different rule. Expose one policy for both
presentation and mutation, with a mutation-side invariant as defense in depth.

Prefer pure functions for policy, scoring, migrations, and deterministic
selection. Control clocks, random seeds, and ordering. Define deterministic
tie-breaking whenever iteration order could affect visible or persisted state.

Derived indexes and caches must have one authoritative source and be
rebuildable. Do not serialize a cache merely because rebuilding it was not yet
implemented.

## Version saves from the first public build

Every persisted payload has an explicit schema version and build context.
Validate at the load boundary before activating runtime state. Migrate old
payloads through deterministic, idempotent steps, then validate the current
schema and rebuild derived state.

Read [references/save-and-migration-contract.md](references/save-and-migration-contract.md)
before changing persisted shape or meaning.

Keep frozen fixtures for every supported public schema. Test new-game startup,
each migration path, divergent player history, missing and duplicate IDs,
interrupted writes, and save restoration in the production build.

## Autosave without destroying evidence

Save at meaningful safe boundaries and periodically during active play. Use an
atomic write or platform-equivalent staging strategy. Never overwrite the last
good save with a frame that just failed validation, an incomplete transaction,
or a partially migrated payload.

Expose saving, success, and failure honestly. On load failure, keep the
original file and report the failed schema/invariant. Recover only facts that
can be reconstructed unambiguously; do not silently reset valid player choices
to current defaults.

## Treat deterministic scenarios as leverage

Provide compact scenario builders or replayable commands for high-risk state
machines. They support tests, performance benchmarks, debugging, and telemetry
reproduction without hand-editing arbitrary saves. Scenario
inputs must use the same canonical IDs and validation as production.

## Completion contract

For any durable-state change, verify:

1. canonical identity and every reference producer/consumer;
2. transition eligibility and atomic mutation;
3. schema version and migration necessity;
4. serializer, loader, caches, workers, UI selections, and telemetry;
5. frozen old-save and new-game fixtures;
6. interrupted or invalid boundary cases; and
7. restoration in the production artifact.
