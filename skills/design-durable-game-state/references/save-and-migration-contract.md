# Save and migration contract

## Current payload

A current save should contain:

- schema version;
- game/build edition needed for compatibility decisions;
- authoritative player and world facts;
- canonical IDs for all relationships; and
- enough deterministic state to resume without guessing.

Do not continue writing deprecated fields after migration. Avoid serializing
rebuildable render state, caches, DOM/canvas objects, asset handles, or indexes.

## Load pipeline

1. Parse without activating the payload.
2. Validate the outer envelope and supported schema range.
3. Apply ordered deterministic migrations to an isolated value.
4. Validate the complete current schema, uniqueness, references, units, and
   closed-set variants.
5. Construct runtime state and rebuild derived indexes/caches.
6. Activate only after the complete state passes.

Do not mutate the on-disk original during a failed load. Write the migrated
current format only through the normal atomic save path after successful
activation.

## Migration rules

- Make every step idempotent or explicitly guard its source version.
- Preserve divergent player-created history.
- Fill missing metadata only when the answer is unique and documented.
- Never guess between ambiguous legacy references; add an explicit mapping.
- Record unit and semantic changes, not only renamed fields.
- Remove temporary compatibility code after its supported migration window;
  version control retains old implementations.

## Save scheduling

Use safe points such as completed transactions, scene or hub entry, menu opening,
and bounded periodic intervals. Coalesce concurrent requests and prevent stale
async completion from overwriting newer state. Surface write failures without
blocking play when the current in-memory state remains valid.

For browser storage, define behavior for unavailable APIs, private browsing,
quota exhaustion, and eviction. For player-valued saves, provide an export or
backup path where practical. If supporting multiple local slots or cloud saves,
give each save lineage a stable ID and define conflict resolution explicitly;
timestamps alone are not a safe merge policy.

## Tests

For each supported schema, keep a frozen representative fixture and verify:

- migration is deterministic;
- running it twice does not corrupt state;
- canonical references resolve;
- valid historical choices survive;
- current serialization contains no deprecated fields;
- interrupted writes leave the previous save usable; and
- the production build can start and resume the result.
