---
name: polish-game-feel
description: Design, implement, or review game feel through readable motion, impacts, camera feedback, sound and interaction responses. Use when asked to add juice, make actions feel weightier, tune screen shake or hit stop, or improve feedback on game actions and UI choices. Preserve gameplay rules and the chosen art direction; use a trailer review skill for marketing edits rather than this skill.
---

# Polish Game Feel

Make important actions satisfying and easy to understand. Amplify their meaning
without making every event equally loud or hiding weak animation under effects.

## Establish the feedback contract

Inspect the actual renderer, audio, action events and existing settings before
adding effects. Distinguish missing feedback from feedback that is implemented
but mistimed, obscured, too repetitive or inappropriate for the viewing scale.
Keep accepted assets and functioning cues; make targeted changes.

For the chosen action, identify:

- what the player did and what result they need to perceive;
- anticipation, commitment, contact, consequence and recovery, where relevant;
- which events are routine and which deserve exceptional emphasis;
- the camera distance, supported devices, art direction and sensory options.

A fired weapon, a connected hit and a body landing are different events. Route
their effects through confirmed gameplay events or explicit presentation events;
do not infer a hit merely because an attack started. Keep visual feedback,
sound and optional haptics aligned with the appropriate event.

## Choose emphasis that fits the game

Start with readable poses, physical follow-through and well-timed audio. Layer
local visual feedback and camera motion according to importance and proximity.
Use changes in density, quiet recovery and aftermath to give strong beats room
to register. Avoid constant shake, stacked flashes or effects that conceal the
next decision. Do not change damage, fire rate, enemy count or control rules
under the label of polish unless that design change is within the request.

Read [feedback-patterns.md](references/feedback-patterns.md) when selecting or
implementing camera impulses, hit stop, audio, UI responses or material effects.
Its patterns are choices, not a mandatory package for every action or genre.
Source-backed principles and project-specific tuning are different: do not
present an example timing, shake amplitude or colour as a universal rule.

## Preserve presentation and state

Keep base framing separate from additive camera effects. Bound overlapping
impulses and leave text/HUD readable. Register effects to their intended object,
world or screen space; verify them while the camera moves. Respect the game's
pixel grid, print aesthetic or other rendering contract.

Share event timing and effect definitions across live playback and captures.
For replayable, scrubbed or offline scenes, derive effect state from event IDs,
seeds and time rather than render-call count. A local animation hold or global
pause must not apply gameplay consequences twice, drop later events or
accidentally desynchronize sound. Specify how the affected clocks resume.

Use motion/sensory settings appropriate to the game, including a zero-shake
option when adding shake. Essential information must survive reduced motion,
muted sound and disabled decorative effects. Follow existing input/accessibility
contracts rather than creating a mouse-only settings path or resetting user
preferences. Avoid unsafe flashes in the default presentation.

## Judge the actual result

Compare the original and revised action under the same gameplay, event timing,
camera framing and audio levels. Check the complete life of the effect at the
intended display size, with sound, in the actual build. A static particle sheet
or passing numerical test does not establish game feel.

Exercise dense repeated actions as well as a single featured beat. Check pause,
seek/replay where supported, scene changes, camera bounds and reduced-effect
settings. Verify the benefit remains legible without sound or colour alone.
Test genuine invariants such as once-only gameplay consequences and bounded
overlapping camera displacement; avoid tests that merely copy tuning values.

Record the chosen parameters, observed result and remaining uncertainty in the
project's existing design or feedback record. Distinguish technical verification
from an actual listening/playtesting verdict. Credit acquired media and derived
assets through the project's provenance ledger. Choose production methods by
the required material behaviour, quality, cost and reproducibility; simple
procedural effects do not automatically need a generative-video workflow.
