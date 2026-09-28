---
name: architect-web-first-indie-game
description: Plan or review a small indie game's browser-first technical architecture, rendering strategy, Cloudflare Pages prototype path, platform boundaries, and performance envelope. Use when starting a browser-first game, choosing DOM/Canvas/SVG/WebGL/WebGPU or an engine, planning a playable web build, diagnosing architectural scaling risk, or preparing a browser prototype to become a desktop release; do not use for marketing strategy or native-only architecture.
---

# Architect a Web-First Indie Game

Make the real game playable in a browser early. Treat the browser build as a
production surface that reveals input, performance, persistence, loading, and
deployment problems—not as a disposable mockup that will be rewritten later.

## Walk beginners through the human setup

Before infrastructure work, check whether the game already has version control
and a hosting account. The default human checklist is:

1. Create a GitHub account and a repository for the game, then connect the local
   checkout and authenticate Git access.
2. Create a free Cloudflare account, obtain the scoped credentials needed for
   the planned services, and paste them privately into the project's local
   ignored `.env` so the agent can configure hosting and web services.

If either step is incomplete, guide the user through it in small, concrete
steps assuming no coding experience. Use existing setup when present and honor
an explicit choice of another version-control or hosting provider. Do the
technical setup the agent can perform; reserve account signup, authentication,
and private credential entry for the human.

Read [references/beginner-setup.md](references/beginner-setup.md) when either
prerequisite is missing. Never ask the user to paste credentials into chat or
display their values while verifying setup.

## Start with the envelope

Before recommending a renderer, framework, or engine, establish:

- game dimension and camera model;
- maximum visible interactive elements, sprites, particles, lights, world cells,
  labels, and UI;
- target desktop and mobile hardware;
- expected asset volume and largest loading burst;
- simulation frequency, state-history/undo needs, determinism, and offscreen
  simulation scale;
- accessibility, localization, and input requirements; and
- intended web, desktop, and storefront packaging.

Choose for the expected production scene, not the first empty room. DOM/CSS or
SVG may be the best fit for text-heavy, accessible, or modest board/puzzle
interfaces. Canvas is appropriate when measured draw count, effects,
compositing, and scale fit it. If the concept plainly requires large layered worlds,
thousands of sprites, masks, lighting, or heavy compositing, establish a GPU
renderer before content depends on Canvas-specific behavior. Do not prescribe
GPU complexity when the game does not need it.

Read [references/architecture-checkpoints.md](references/architecture-checkpoints.md)
when selecting or reviewing the renderer and module boundaries.

## Preserve replaceable boundaries

Keep these concepts separate from the first working version:

- deterministic domain state and transitions;
- rendering and presentation caches;
- semantic input actions;
- persistence and migrations;
- localization and text layout;
- asset loading and generated manifests;
- telemetry and production recovery; and
- platform services such as storefront APIs, filesystem, achievements, and quit.

Browser and desktop builds should call the same game operations through small
platform adapters. Do not scatter environment checks throughout domain logic.

Canonical IDs are an architectural requirement from the first prototype. Every
durable entity receives one at creation. Names, translated labels, coordinates,
array positions, and storage keys are presentation or location facts, never
identity. Renames, relocation, conquest, localization, world-resolution
changes, and catalog reordering must not break references.

Use `design-durable-game-state` when specifying identities, save schemas, or
migrations in detail.

## Ship the web build continuously

Deploy a static production build to a free or low-cost private preview or public
host as soon as the core loop is playable. When the current Free-plan limits
fit the build, Cloudflare Pages is a good default for a static browser game:
Git deployments make every pushed revision testable, and preview deployments
support review before production. Keep hosting replaceable; do not couple the
game simulation to Pages.

Read [references/web-prototype.md](references/web-prototype.md) before creating
or changing the deployment workflow. Retrieve current Cloudflare documentation
before relying on plan limits or configuration details.

Every deployed build should expose a revision identifier that can be matched to
telemetry and source control. After deployment, verify the live revision rather
than treating a successful local build or Git push as proof that production is
current.

## Scale by evidence

Establish representative stress scenes before the world is full of content.
Measure frame time by subsystem, not only average FPS. Keep hot-path work
bounded: index spatial queries, cache stable layers, move expensive preparation
off the frame, and match update frequency to gameplay need.

Keep generated artifacts reproducible. Source data and generation code are the
authority; atlases, navigation tables, manifests, and catalogs must be
rebuildable and validated against checked-in fingerprints or contracts.

## Deliver an architectural decision

When planning or auditing, produce:

1. the production envelope and unverified assumptions;
2. the renderer and platform architecture with reasons;
3. stable boundaries and canonical entity identities;
4. the smallest shareable browser milestone;
5. representative performance and loading tests;
6. the route from browser to intended desktop/storefront builds; and
7. explicit triggers for revisiting a decision.

Prefer one coherent architecture with measured escape hatches over parallel
implementations kept “just in case.”
