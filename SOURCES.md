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

## Game feel and impact feedback

Checked 12 October 2026. These references inform `polish-game-feel`; they do not
establish universal effect strengths, durations or art styles.

- [Martin Pichlmair and Mads Johansen, Designing Game Feel: A Survey](https://arxiv.org/abs/2011.09201)
  (2020 preprint; subsequently IEEE Transactions on Games): distinguishes
  physicality, amplification and support. The inspected impact/time sections
  distinguish local hit stop from whole-world frame freezes and discuss layered
  audiovisual feedback.
- [Squirrel Eiserloh, Juicing Your Cameras With Math, GDC 2016](https://www.gdcvault.com/play/1023557/Math-for-Game-Programmers-Juicing),
  with [official slides](https://media.gdcvault.com/gdc2016/Presentations/Eiserloh_Squirrel_JuicingYourCameras.pdf):
  bounded decaying stress, nonlinear shake amplitude, smooth seeded noise,
  preserved base framing and restraint. Session listing and camera slides were
  inspected; no fresh complete viewing of the video is claimed.
- [Martin Jonasson and Petri Purho, Juice It or Lose It, GDC Europe 2012](https://www.gdcvault.com/play/1016487/juice-it-or-lose)
  and [Jan Willem Nijman's author listing for The Art of Screenshake, INDIGO 2013](https://www.jwaaaap.com/):
  practitioner demonstrations of game-feel polish. Official listings inspected;
  useful viewing references, without claiming a fresh complete viewing or
  inferring game-specific tuning from their titles.
- [Xbox Accessibility Guideline 117](https://learn.microsoft.com/en-us/xbox/accessibility/xbox-accessibility-guidelines/117):
  additive camera movement, screen shake and motion settings. Verify current
  guidance when implementing accessibility features.
- [Curtis, Anderson, Seims, Fleischer and Salesin, Computer-Generated Watercolor, SIGGRAPH 1997](https://grail.cs.washington.edu/projects/watercolor/paper_small.pdf),
  also described on [the author's research page](https://otherthings.com/uw/watercolor/):
  surface-fluid, pigment-deposition and capillary layers; paper capacity and
  medium-dependent edge accumulation. Relevant paper sections were inspected. This is a
  reference for material-specific effects, not a validated blood/dye-ink solver.

All skill instructions are original synthesis. No paper figures, stock media,
third-party solver implementation or game-specific artwork is bundled. The
effect choices require evaluation in the actual game; examples from one game
must not become mandatory aesthetics or gameplay changes for another.
