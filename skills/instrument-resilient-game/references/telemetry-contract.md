# Telemetry contract

## Consent

Explain in plain language what is collected, why, where it is sent, how long it
is retained, and how to change the choice. Consent must be an affirmative
choice, not inferred from starting the game. Store the choice separately from
event data. Exercise grant, denial, revocation, and unavailable-storage paths.

Do not block core play on consent or endpoint health. On revocation, purge
queued events that no longer have a lawful/consented delivery basis. Version
the consent disclosure so a materially changed collection policy can request a
new decision rather than silently extending old consent. A local bounded queue
may hold consented incidents during transient failure; it must expire or
discard oldest entries rather than grow forever.

## Event envelope

Use a versioned closed schema. A practical incident envelope includes:

- schema version;
- event ID unique only enough for idempotency;
- coarse timestamp;
- build revision and edition;
- game/runtime/platform versions;
- locale and renderer;
- event kind and subsystem;
- sanitized signature;
- canonical in-game IDs needed to reproduce;
- bounded numeric/enum context; and
- recovery action and outcome.

Reject unknown keys, wrong types, oversized arrays, non-finite numbers, unknown
enum values, and payloads above the endpoint limit. Never accept arbitrary
client-provided SQL columns or treat display text as identity.

## Cardinality and privacy

Prefer enums, canonical content IDs, ranges, and sampled measurements over raw
strings. Hashing personal data does not make collecting it necessary. Do not
record IP addresses in application storage, exact real-world location, email,
account tokens, filesystem paths, or free-form user text.

Separate high-volume performance samples from rare incident reports. Sample or
aggregate normal performance; preserve enough stable context for failures.

## Delivery

- batch small events when practical;
- use idempotency keys where a retry could duplicate a write;
- bound retries and queue size;
- treat non-success HTTP status as failed delivery;
- never let telemetry exceptions escape into gameplay; and
- expose queue/drop state in developer diagnostics.

Telemetry delivery is a side effect, not authority for game state.

## Previous-session and startup failures

Keep a minimal local startup/crash marker that can distinguish a clean exit from
an interrupted prior session without containing personal or save content. On a
repeated startup crash, preserve unsent incident evidence and offer a narrow
safe-mode or recovery path appropriate to the game; do not loop indefinitely
through the same failing initialization or overwrite the last good save.

Instrument main-thread, worker, promise-rejection, input, save/load, and startup
boundaries deliberately. A global boundary is the last net, not a substitute
for feature-specific recovery.
