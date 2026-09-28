# Public web prototype

## Purpose

The browser build shortens the feedback loop: a tester opens a URL, plays the
actual build, and can report the exact revision. The preview may be private or
public according to the project's needs. Keep onboarding short enough
to reach the mechanic under test. Do not use the public prototype as an excuse
to expose unfinished telemetry, secrets, debug controls, or private content.

## Cloudflare Pages default

Cloudflare Pages currently supports GitHub and GitLab integration, automatic
production deployment, branch previews, and framework-free static projects.
Use the current official setup guide:

- <https://developers.cloudflare.com/pages/get-started/git-integration/>
- <https://developers.cloudflare.com/pages/configuration/git-integration/>

Pages has a Free plan with limits that change over time. Check the current
limits before promising that a build will remain free:

- <https://developers.cloudflare.com/pages/platform/limits/>

Decide deliberately between Git integration and Direct Upload; current Pages
documentation warns that a Git-integrated project cannot later be converted to
a Direct Upload project.

## Build contract

The deployment must:

- produce the same optimized artifact tested locally;
- fail on missing assets, broken imports, invalid generated manifests, or an
  unknown build edition;
- stamp the source revision and build edition into a small live-readable file;
- avoid bundling source maps, secrets, dev menus, or private test fixtures
  unintentionally; and
- preserve deterministic URLs or manifests for cache-sensitive assets.

After each production push, inspect the provider run and fetch the live
revision file. A green local build and successful Git push are prerequisites,
not deployment verification.

## Prototype exit criteria

Before treating the browser prototype as evidence, verify:

- first load and reload on a clean browser profile;
- touch, mouse, keyboard, and controller paths intended for the milestone;
- save/load and version mismatch behavior;
- browser storage availability, quota/eviction, backup/export, and private-mode
  behavior appropriate to the chosen persistence layer;
- consent granted, denied, revoked, and endpoint unavailable;
- representative low-end performance and memory;
- hidden-tab, focus-loss, resize, audio-autoplay, and offline/interrupted
  loading or cache-update behavior; and
- the exact production URL and revision seen by the tester.
