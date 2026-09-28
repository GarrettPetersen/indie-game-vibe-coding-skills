# Canonical identity

## ID contract

A canonical ID is assigned once, unique in its entity namespace, stable across
presentation and state changes, serializable, and safe to compare without
localization or normalization. Prefer explicit typed wrappers or clearly named
fields such as `locationId`, `actorId`, and `groupId`; do not pass them as
interchangeable strings.

Human-readable slugs can be canonical IDs if they are treated as immutable
identifiers rather than regenerated from display names. If a slug might need to
change for editorial reasons, use a separate opaque ID and retain the slug as
presentation metadata.

Choose and test an ID-generation policy for runtime/player-created entities:
random UUID/ULID-style IDs, a persisted monotonic allocator, or another scheme
with an explicit collision rule. Never derive the ID from the player's chosen
name. Permit duplicate display names when the game design allows them; search
and UI disambiguation are presentation concerns. Normalize text for search
without rewriting the stored player-authored display value.

## Introduction checklist

When adding an entity type, identify:

- creation and ID assignment;
- duplicate detection;
- authoritative registry;
- save representation and migration;
- references from quests, events, ownership, relationships, and history;
- UI selection and navigation;
- worker or network messages;
- telemetry context;
- generated indexes and content tools; and
- deletion, retirement, or tombstone behavior.

Resolve references at a boundary and fail with the referencing and missing
canonical IDs. Do not search by display name as a quiet fallback.

## Common fiascos to prevent

- A translated or renamed location breaks quests keyed by its English name.
- Array insertion points every saved index at the wrong entity.
- An ownership change accidentally changes the owned object's identity.
- A map-resolution change invalidates saves keyed to tile coordinates.
- Two characters with the same name collapse into one record.
- A UI row persists its visual index and selects a different object after sort.
- A cache becomes a second authority and disagrees with runtime state.

Write invariant tests for the class of mistake: rename, reorder, relocate,
translate, change owner, rebuild indexes, and round-trip the save while
references continue to resolve to the same canonical entity.

When deletion is legal, define whether references prevent deletion, cascade to
explicit dependent records, or resolve through a tombstone/history record.
Never leave silent dangling references.
