# Indie Game Vibe Coding Skills

Opinionated agent skills for developing a small indie game from the first
browser prototype through public playtests and desktop release. They are based
on lessons learned while building *Marque & Reprisal*: make the game easy to
play early, instrument the real build, recover gracefully for players, fail
loudly for developers, and establish durable architecture before content makes
change expensive.

This repository covers development practice. Marketing strategy, Steam page
audits, tags, trailers, and launch planning live in the separate
[`indie-game-marketing-skills`](https://github.com/GarrettPetersen/indie-game-marketing-skills)
repository.

## Skills

- `architect-web-first-indie-game`: choose the renderer and technical
  boundaries early, ship a browser prototype on Cloudflare Pages, and preserve
  a path to desktop storefronts.
- `instrument-resilient-game`: combine consent-based telemetry, narrow
  production recovery, fail-fast development builds, and useful diagnostics.
- `design-durable-game-state`: build canonical identities, deterministic state
  transitions, versioned saves, migrations, and safe autosaves.
- `build-cross-input-localized-game`: make every player-facing feature work
  across touch, mouse, keyboard, controller, languages, fonts, and layouts.
- `maintain-game-credits`: record every asset, license, contributor, and
  consenting playtester when they enter the project.
- `triage-game-playtest-feedback`: reconcile reports with the actual build and
  telemetry, prioritize fixes, and keep the action ledger synchronized.

The lessons that recur across the skills are deliberate: stable IDs before
content proliferates; generated artifacts that can be rebuilt; testable domain
logic outside rendering; real-browser and production-build checks; a visible
build revision; and a living feedback ledger that is updated with the code.

## Install

Clone this repository somewhere stable. Link individual skill directories into
one game's `.agents/skills` directory:

```sh
mkdir -p /absolute/path/to/game/.agents/skills
ln -s /absolute/path/to/indie-game-vibe-coding-skills/skills/architect-web-first-indie-game /absolute/path/to/game/.agents/skills/architect-web-first-indie-game
ln -s /absolute/path/to/indie-game-vibe-coding-skills/skills/instrument-resilient-game /absolute/path/to/game/.agents/skills/instrument-resilient-game
ln -s /absolute/path/to/indie-game-vibe-coding-skills/skills/design-durable-game-state /absolute/path/to/game/.agents/skills/design-durable-game-state
ln -s /absolute/path/to/indie-game-vibe-coding-skills/skills/build-cross-input-localized-game /absolute/path/to/game/.agents/skills/build-cross-input-localized-game
ln -s /absolute/path/to/indie-game-vibe-coding-skills/skills/maintain-game-credits /absolute/path/to/game/.agents/skills/maintain-game-credits
ln -s /absolute/path/to/indie-game-vibe-coding-skills/skills/triage-game-playtest-feedback /absolute/path/to/game/.agents/skills/triage-game-playtest-feedback
```

Linking them into `~/.codex/skills` instead makes them available to every
project. Do not install the same skill at both scopes.

## Scope and evidence

These skills encode a development philosophy, not a game engine or a promise
that one architecture fits every game. They should adapt to the game's actual
scale, genre, devices, team, and release plan. Cloud services, browser APIs,
storefront requirements, and pricing change; verify current first-party
documentation when implementing them. See [SOURCES.md](SOURCES.md).
