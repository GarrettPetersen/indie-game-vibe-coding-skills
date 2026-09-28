# Accessibility options as feature contracts

Choose options according to the game's barriers and supported platforms. Build
the low-cost foundations early, and agree on scope for larger features such as
screen-reader gameplay or alternative difficulty modes. Do not silently promise
support that the renderer or interaction model cannot deliver.

## Baseline to assess

- Vision: readable text, text/UI sizing or a high-legibility mode, adequate
  contrast, visible focus, and symbols/labels alongside meaningful colors.
  Color filters alone do not replace redundant information.
- Hearing: captions for meaningful speech and visual equivalents for essential
  audio cues. Separate music, effects, and speech volumes where those channels
  exist; preserve gameplay information when any channel is muted.
- Motor: remappable semantic actions, keyboard/controller menu navigation,
  configurable hold/toggle behavior, alternatives to repeated presses or precise
  dragging, and suitable sensitivity/dead-zone controls. Avoid essential chords
  or rapid sequences without an alternative.
- Motion and sensory load: reduced camera shake, flashing, motion effects, and
  optional vibration. Avoid unsafe flashes in the default presentation; a
  warning or opt-out is not a substitute for safer effect design.
- Cognitive load and timing: clear objectives, replayable tutorials, legible
  notices, and adjustable text advance. Consider pause, speed, timing assists,
  or difficulty options when compatible with the game; make any multiplayer or
  scoring consequences explicit rather than silently changing rules.

## Integration and verification

Make settings available from startup and in-game through every supported input
adapter. Show the result immediately when safe, with a reversible preview for
changes that could make the UI unusable. Persist preferences independently from
a particular playthrough; avoid resetting them on new game or save loading.

Localize labels, explanations, captions, and accessible names. Test large text
and high-expansion locales together with touch and controller focus. A scalable
high-legibility renderer can be separate from pixel-art text; preserve the
default art-grid contract and never shrink accessibility text to resolve overflow.

Use semantic DOM controls where appropriate. Canvas-drawn controls need real
programmatic semantics and focus integration for claimed assistive-technology
support; a drawn focus ring is insufficient. Test with the actual assistive
technology before claiming support.

Exercise muted audio, reduced motion, remapped controls, hold/toggle changes,
largest text, restart/save-load persistence, and menu entry before gameplay.
Verify information remains available and the same enabled semantic actions
still work. Seek playtester feedback on usability, not just technical toggling.
