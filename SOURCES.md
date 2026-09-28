# Sources

## Project evidence

The opinions in these skills come primarily from direct experience building,
testing, deploying, and revising *Marque & Reprisal*. In particular:

- silent fallbacks concealed broken new systems;
- player-reachable assertions made first-wave playtests unnecessarily brittle;
- a production-recovery/development-fail-fast split made faults observable
  without sacrificing the player's session;
- a late Canvas-to-GPU transition was much more expensive than choosing the
  rendering envelope early;
- canonical IDs, versioned saves, generated-asset validation, multi-input
  actions, early localization, and an asset credit ledger became more valuable
  as content multiplied; and
- telemetry plus a maintained feedback ledger turned vague playtest reports
  into reproducible fixes.

These are case-study conclusions, not universal platform rules. A skill should
explain when a recommendation does not fit the game in front of it.

## Current first-party platform references

Checked 28 September 2026:

- [GitHub account setup](https://docs.github.com/en/get-started/onboarding/getting-started-with-your-github-account)
  and [repository creation](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
  for beginner onboarding.
- [Cloudflare API token creation](https://developers.cloudflare.com/fundamentals/api/get-started/create-token/)
  for scoped credentials supplied through local configuration.

- [Cloudflare Pages Git integration](https://developers.cloudflare.com/pages/get-started/git-integration/)
  for repository deployments and preview builds.
- [Cloudflare Pages limits](https://developers.cloudflare.com/pages/platform/limits/)
  for current Free-plan constraints. Recheck rather than copying limits into a
  long-lived plan.
- [Cloudflare D1 overview](https://developers.cloudflare.com/d1/) and
  [D1 getting started](https://developers.cloudflare.com/d1/get-started/) for a
  small Worker- or Pages Function-backed incident store.
- [Workers rate limiting](https://developers.cloudflare.com/workers/runtime-apis/bindings/rate-limit/)
  for abuse controls.
- [Workers context](https://developers.cloudflare.com/workers/runtime-apis/context/)
  for the rule that background work must be awaited or registered with
  `waitUntil()`.
- [Workers CORS example](https://developers.cloudflare.com/workers/examples/cors-header-proxy/)
  for browser endpoint response and preflight handling.

Product availability, limits, API shapes, pricing, and deployment behavior can
change. Retrieve the current official page before implementing platform code.
