---
name: build-cross-input-localized-game
description: Design, implement, or review player-facing game features so they work across touch, mouse, keyboard, remappable controllers, languages, fonts, and screen shapes. Use when adding controls, tutorials, menus, HUDs, dialogue, accessibility, text, focus behavior, or any interaction that must not assume one device or English layout.
---

# Build a Cross-Input, Localized Game

Input and localization are architecture, not polish. Adding them after dozens
of features exist creates duplicated event paths, hard-coded key names,
unextractable prose, broken layouts, and inconsistent game rules.

## Model actions, not devices

Define semantic actions such as move, confirm, cancel, inspect, open map, and
fire. Keyboard keys, mouse buttons, touch regions, gamepad controls, storefront
input APIs, and accessibility alternatives map into those actions.

One action dispatcher should enforce the same eligibility and transition for
every device. Do not implement a click path and later approximate it in a
controller handler. UI availability and mutation preconditions must share one
policy.

Read [references/input-contract.md](references/input-contract.md) when adding or
changing controls, focus, tutorials, or navigation.

Never write instructions that assume WASD, QWERTY, Xbox labels, a particular
controller, hover, right-click, or a touchscreen. Render the player's current
binding or describe the semantic action. Any information revealed on hover
needs a focus, tap, selection, or inspect path where relevant.

## Treat text as data at the feature boundary

Every player-facing label, notice, option, tooltip, tutorial, dynamic sentence,
error recovery message, and accessibility description enters the localization
catalog when the feature is built. Do not hide prose in concatenation, runtime
branches, image pixels, or unrecognized data fields.

Read [references/localization-contract.md](references/localization-contract.md)
before creating text or changing the catalog/generation workflow.

Build dynamic sentences from registered templates with validated placeholders.
Keep canonical IDs separate from translated names. Preserve case intentionally;
do not uppercase whole translated sentences or player names as a layout trick.

## Design layouts for variation

Assume text can expand, line metrics can change, scripts can require different
fonts, and controls can display longer binding names. Define wrapping,
scrolling, pagination, minimum hit target, clipping, and compact-layout policy
explicitly. Do not fix overflow by fractionally distorting a bitmap font or by
silently dropping meaning.

Separate default pixel-art text constraints from an optional high-legibility
renderer if the game supports one. Accessibility mode may need different fonts
and metrics while retaining the same painter order, clipping, hit targets,
content, and full-window behavior.

## Test the matrix without multiplying logic

For each player-facing state machine, test semantic actions first, then a small
adapter matrix:

- keyboard default and at least one remapped/non-QWERTY binding;
- mouse without relying on hover alone;
- touch without accidental steering or hidden precision targets;
- a generic controller with focus recovery and device changes;
- supported locales with placeholder and font coverage;
- short, wide, tall, and resized viewports; and
- focus loss, hidden tab, disconnect, and input-device switching.

The same enabled action should succeed through every adapter. A disabled action
should remain disabled with the same reason.

## Completion contract

A player-facing feature is incomplete until:

1. it uses semantic actions and current bindings;
2. keyboard, mouse, touch, and controller implications are resolved, even when
   a platform intentionally does not support one;
3. all new text is catalog-visible and generated for every supported locale;
4. translations are manually reviewed for ambiguous game terminology, proper
   names, controls, and placeholders;
5. layout and font behavior is exercised in representative locales/screens;
6. runtime input switching and focus behavior are tested; and
7. credits are updated for any new fonts, icons, audio, art, translators, or
   playtesters.

Use `maintain-game-credits` for the final provenance step.
