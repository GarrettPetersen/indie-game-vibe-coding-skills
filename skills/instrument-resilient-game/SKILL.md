---
name: instrument-resilient-game
description: Design, implement, or audit a game's consent-based telemetry infrastructure, production error recovery, developer fail-fast mode, diagnostic overlay, or optional Cloudflare ingestion adapter. Use when building one or more of those systems, deciding whether runtime code should assert or recover, or hardening incident boundaries for a public playtest; use a playtest-triage skill to manage a corpus of reports and priorities, and do not add covert analytics or broad player tracking.
---

# Instrument a Resilient Game

Thread the needle between two destructive extremes:

- silent fallback everywhere, where a new system never works and nobody knows;
- assertions everywhere, where first-wave players become involuntary crash
  testers and lose confidence in the game.

The default compromise is: **fail fast in development; report and recover
locally in production**.

## Separate three modes

Do not overload one “debug mode” flag with incompatible meanings.

1. **Development/automated diagnostic build:** assertions throw, unexpected
   rejections fail the run, missing assets and invalid layouts surface
   immediately, and production recovery fallbacks are disabled so defects fail
   hard. This is the developer debug mode.
2. **Production build:** player-reachable failures create a bounded local
   diagnostic record and use the narrowest safe local recovery. Transmit that
   record only when current consent permits it. Preserve the current session
   and last good save whenever possible.
3. **Player-visible diagnostics overlay:** shows FPS and useful state but keeps
   production recovery. A setting a player can enable must not expose a
   developer crash screen or turn ordinary play into a fail-fast build.

Read [references/recovery-policy.md](references/recovery-policy.md) when
classifying a failure or adding a runtime boundary.

## Make fallback observable and narrow

A fallback is acceptable only when it:

- reports the violated invariant with build, subsystem, stable entity IDs, and
  bounded context;
- preserves valid player state;
- affects the smallest possible surface;
- does not pretend the requested operation succeeded; and
- remains impossible to miss in development and tests.

Examples of narrow presentation recovery include keeping the last valid frame,
clipping one label, omitting one stale widget, or disabling one invalid action.
Corrupt identity, unknown persisted variants, missing required content, and
partially applied transactions are not presentation problems. Validate before
mutation, commit atomically, and report a rejected operation rather than
inventing plausible state.

## Route to the requested branch

Apply only the relevant branch or branches:

- For recovery/assertion policy, use the mode split and recovery reference.
- For telemetry collection or transport, use the consent/schema references and
  provider adapter selected by the project.
- For an FPS/debug overlay, implement the diagnostic-overlay guidance without
  inventing a telemetry backend or public setting.
- For report-corpus triage, prioritization, and ledger maintenance, use
  `triage-game-playtest-feedback` instead.

## Collect only useful telemetry

Obtain meaningful consent before transmission. The game must remain playable
when consent is denied, revoked, the endpoint is offline, or storage is full.
Do not collect names, email addresses, raw chat, arbitrary save contents,
precise location, secrets, or stable cross-game identity merely because they
are available.

Use a versioned, allowlisted event schema. Include only what can change a
debugging decision: build revision, edition, runtime/platform, locale, event
type, subsystem, stable in-game IDs where needed, bounded numeric state, and a
sanitized error signature. Bound payload size, event frequency, queue length,
cardinality, retries, and retention.

Telemetry must cover both bugs (including recovered failures) and sustained
below-threshold FPS. Set the FPS threshold and observation window for the
game's performance target; distinguish active gameplay from hidden tabs,
intentional pauses, and loading. Capture bounded frame-time summaries and
relevant scene context rather than sending every slow frame.

Require a cooldown per incident signature and per low-FPS episode, plus a
shared transport cooldown and session budget across all event kinds. Coalesce
repeats into bounded counts; prioritize bugs over routine performance reports.
Retries and reconnect flushes must obey the same limits. Client throttling and
server-side abuse protection are both necessary to avoid flooding the service.

Read [references/telemetry-contract.md](references/telemetry-contract.md) before
designing payloads or consent flow. If Cloudflare is the selected ingestion
provider, read [references/cloudflare-endpoint.md](references/cloudflare-endpoint.md).

## Build a useful diagnostic overlay

Show the statistics that answer “what is the game doing?” rather than a wall of
internal state. Start with:

- FPS, frame time, and worst recent frame;
- time by major update/render subsystem;
- visible and simulated entity counts;
- cache, queue, worker, and asset-loading pressure;
- memory when the platform exposes a meaningful measure;
- build revision, edition, renderer, locale, and active input device;
- telemetry consent/queue state; and
- recent recovered-incident count and stable signatures.

Keep it cheap, bounded, readable, and removable from public screenshots. Do not
run expensive whole-world scans every frame merely to populate diagnostics.

## Close the loop when implementing a tracked incident

For every public incident:

1. distinguish build/version and affected population;
2. reproduce from stable context, a sanitized save fixture, or deterministic
   scenario;
3. identify the violated invariant rather than patching the visible symptom;
4. add a regression test that fails for the original cause;
5. verify both fail-fast development behavior and production recovery;
6. if delivery is in scope and authorized, publish through the project's
   workflow and confirm the delivered revision; and
7. update the project's playtest/feedback ledger with the fix commit when the
   project maintains one.

A task based on tracked feedback is not complete until its ledger entry is
updated and committed under the project's workflow. Pushing or deploying still
requires the authority granted for that task.
