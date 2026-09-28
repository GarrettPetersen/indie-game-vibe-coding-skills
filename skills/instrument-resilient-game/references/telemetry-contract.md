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

## Bugs and below-threshold FPS

Report unexpected bugs whether they crash a subsystem, reject an operation, or
recover locally. Use a stable signature based on the failed invariant and
subsystem, not changing timestamps or individual entity IDs.

Also report sustained FPS below a configurable performance threshold during
active gameplay. Measure over a bounded rolling window using a monotonic clock;
a single slow frame is not a sustained low-FPS episode. Exclude hidden-tab
throttling, intentional pauses, and loading from gameplay FPS alerts; measure
loading separately if useful. Include the threshold, window duration, measured
FPS, frame-time summary, and bounded scene/render workload context. Define
recovery with hysteresis so measurements near the threshold do not repeatedly
start new episodes. A continuing episode may send a cooldown-spaced summary,
never a report per frame. Keep consent requirements identical to bug reports.

## Cooldowns and flood prevention

Enforce limits at collection and delivery, shared across runtime boundaries,
workers, and both bug and performance events:

- A per-signature cooldown suppresses repeated bugs; a per-episode cooldown
  suppresses repeated low-FPS reports. Preserve bounded occurrence counts and
  first/last occurrence times rather than enqueueing every repeat.
- A global minimum interval between outbound requests and a session-wide event
  budget protect against many distinct signatures bypassing deduplication.
  Bound the signature table, event queue, batch size, and aggregation counters.
- Prioritize actionable bug reports when the budget is tight. Track suppressed
  and dropped counts locally in diagnostics; reporting those counts must not
  bypass the limiter or recursively generate incidents.
- Use bounded exponential backoff with jitter for retries and respect server
  retry guidance. Offline recovery, page-exit delivery, and reconnect flushes
  must use the same limiter; never drain a backlog in a burst.
- Apply server-side request, body-size, and storage/write budgets too. Browser
  cooldowns can be bypassed, and origin restrictions are not authentication.

Document tunable defaults against the expected playtest population and hosting
budget. As a starting example, allow one request per 10 seconds, repeat a bug
signature or ongoing low-FPS episode no more than once per minute, and cap
events per session; tune these rather than treating them as universal values.
Do not let telemetry measurement, aggregation, or transport create frame stalls.

Verify with controlled clocks and deterministic transport fakes: repeated bugs
every frame, many unique signatures, sustained low FPS, recovery/re-entry,
hidden tabs, concurrent producers, revoked consent, endpoint failures, retry
responses, and reconnects. Assert bounded request/write counts and memory, no
catch-up burst, and retained diagnostic context for the first eligible report.

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
