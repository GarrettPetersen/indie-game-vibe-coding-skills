# Input contract

## Layers

Keep four layers distinct:

1. physical event: key, pointer, touch, gamepad axis/button;
2. binding: the player's mapping and thresholds;
3. semantic action: move, confirm, inspect, pause, and so on;
4. game operation: eligibility, transition, and result.

Only adapters know physical labels. Tutorials and UI ask the binding layer for
the current display label. Domain operations consume semantic actions or typed
commands, never browser key codes or controller button numbers.

## Interaction rules

- Make touch targets deliberate and large enough for imprecise input.
- Respect platform safe areas, browser chrome, orientation changes, and virtual
  keyboard resizing on touch devices.
- Do not require hover for essential information.
- Keep pointer movement from stealing controller focus until meaningful mouse
  movement occurs.
- Define what happens when a controller disconnects mid-action.
- Treat browser gamepads with `standard` mapping separately from unknown or
  nonstandard layouts; never guess button labels from an index alone.
- Define focus order, wrap, back/cancel, and modal focus restoration.
- Distinguish press, hold, repeat, analog magnitude, and chord behavior in the
  binding contract.
- Make rebinding conflicts visible and reversible.
- Keep gameplay from continuing or pausing on focus loss by accident; choose
  behavior appropriate to the game and platform.

## Device-neutral help

Help text should say “move,” “confirm,” or “open the map” and insert the active
binding when a concrete control is useful. If input can switch live, refresh
prompts without restarting the screen. Tutorial art and wording must represent
the capability being taught rather than implying it from an unrelated reused
sprite.

For DOM-based UI, preserve semantic elements, visible focus, labels, and
screen-reader relationships. Custom canvas UI needs an equivalent accessible
interaction strategy when accessibility support is in scope; visual focus
alone is not programmatic semantics.

## Test states

Exercise simultaneous inputs, held input across scene changes, pointer capture,
touch cancellation, analog dead zones, remapped bindings, device switching,
modal entry/exit, and a disconnected controller. Test operations through the
public semantic action boundary rather than duplicating game rules in adapter
tests.
