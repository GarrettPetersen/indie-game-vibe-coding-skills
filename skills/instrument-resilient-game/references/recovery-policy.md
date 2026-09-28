# Recovery policy

## Classification matrix

| Failure | Development or automated diagnostic build | Production response |
|---|---|---|
| Text or widget does not fit | Fail the layout test or diagnostic capture | Report; clip, reflow, compress, or omit that widget |
| Optional visual asset fails after the game is active | Throw in the relevant test/build | Report; retain the last valid visual or omit the optional effect |
| Required content or initialization is missing | Fail immediately | Report clearly; do not fabricate content or save partial state |
| Stale UI action no longer satisfies its precondition | Fail action reachability tests | Report; reject/disable the action without mutating state |
| Purchase, sale, reward, or transfer capacity mismatch | Fail the transaction test | Recompute the bounded valid amount or reject atomically; retain remainder |
| Save schema or canonical reference is corrupt | Fail load and migration tests | Report; preserve the file, attempt only explicit safe recovery, never overwrite it |
| Unexpected frame/input/async exception | Throw and stop the diagnostic run | Report once per bounded signature; preserve last good save and recover locally if possible |

## Rules

- Catch only where the caller can add context, clean up, retry safely, or apply
  a defined local recovery.
- Do not swallow an error and continue as if the new system succeeded.
- Keep retry counts and backoff bounded. Retried work must be idempotent.
- Do not autosave a frame or transaction that just failed validation.
- Never use a release-only fallback to weaken tests. Tests should force the
  invalid state and prove both reporting and the chosen recovery.
- The developer crash renderer, if one exists, must be unreachable from normal
  production play. A public diagnostic toggle does not change that rule.

## Error context

Prefer stable context:

- build revision and edition;
- subsystem and operation ID;
- canonical entity IDs;
- schema version;
- bounded counts and units; and
- sanitized error type/signature.

Avoid translated text, player names, raw saves, full stack traces containing
paths or tokens, and unbounded object serialization.

