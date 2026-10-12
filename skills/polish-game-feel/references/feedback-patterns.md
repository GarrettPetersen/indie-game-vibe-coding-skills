# Feedback patterns

Choose the patterns that communicate the action and fit the game. These are
implementation decision criteria, not an instruction to apply every technique.
Primary references are maintained in [SOURCES.md](../../../SOURCES.md#game-feel-and-impact-feedback).

## Camera impulses and shake

Keep authored framing/following separate from a temporary additive impulse.
Start with a small directional kick for a nearby discharge or heavy contact,
then a short settling movement. Distance, event importance and current action
density should influence strength. A missed melee swing normally needs actor
motion and sound, not the same camera emphasis as a decisive hit.

For overlapping events, use an intensity accumulator in a bounded range:

1. Add a weighted contribution at the event time and cap the accumulated value.
2. Decay it with elapsed time, independently of frame rate.
3. Derive shake amplitude from its square or cube so small routine events stay
   small while a deliberate cluster has a perceptibly stronger crest.
4. Sample smooth seeded noise for offsets instead of unrelated random values
   every rendered frame. Apply offsets to the preserved base camera.

This is Eiserloh's stress/"trauma" pattern. A short deterministic impulse curve
may be simpler for sparse events; the accumulator is not a required architecture.
For seeking/replay, reconstruct intensity from retained events or checkpoints.
Repeatedly adding an event on every render is a bug, not extra impact.

Do not sum unrestricted shot amplitudes into an entire volley. Check maximum
displacement under simultaneous events, exposed scene borders and crop margins.
Camera motion should move the intended scene coherently; accidentally shaking
actors without their contact ground can make feet look as though they slide.
Depth-layer offsets still follow the rendering model. Keep text outside shake.
Respect pixel snapping where required. Rotation may suit some 2D games but can
be inappropriate for a printed page, readable map or motion-sensitive audience.

## Hit stop, recoil and physical weight

First sell commitment, impact and follow-through in the actor and object motion.
Scale recoil to the weapon and stance; do not replace a believable reaction with
a whole-screen flash. Physics exaggeration must fit the game's tone rather than
launching every struck enemy across the scene.

Use a very short emphasis hold when it clarifies an important contact. Distinguish
local hit stop, which holds participating animations, from a global freeze,
which stops the whole world. Both can work; globally freezing every ordinary hit
in a busy multi-unit battle can interrupt unrelated actions and become tiring.
Prefer local feedback for routine contacts and stronger emphasis for rare beats.

Specify which clocks stop: world state, actor/root motion, effects, camera and
audio are not automatically the same clock. Resume without teleporting an actor,
reapplying damage or losing queued sounds. In networked or externally timed
games, keep presentation holds separate from authoritative simulation unless
the game explicitly supports global time manipulation.

There is no universal number of hold frames. Express durations in time, judge
them at the target frame rate and tempo, and inspect the complete recovery.
Recorded/captured sound must follow intentional retiming rather than the old
timestamps by accident.

## Audio and contrast

Align the audible transient with ignition/contact, then give it an appropriate
body and environmental tail. Weapon report, struck material, exertion and ground
contact convey different information; layer only what the event warrants.
Maintain headroom under clustered cues and judge the mix with its ambience and
music, not only isolated source files.

Use bounded variation from appropriate source performances when it prevents
obvious repetition. Do not pitch-shift indiscriminately, randomize synced gait
contacts or play a continuous generic motion loop over a stationary actor.
Growth in proximity, receding sound and quieter aftermath can supply weight
without constant camera motion. Preserve essential event information visually
when audio is muted. Acquire and record licences for game and edited-video use.

## UI and lower-intensity actions

Differentiate focus, press, acceptance and the resulting state change. Immediate
local feedback can be a crisp outline, ink stroke, restrained displacement or
short sound appropriate to the interface's materials. Confirmation deserves
more emphasis than hover. Keep words and focus targets stable and readable.

Let the response begin promptly. Avoid mandatory ornamental delays, repeating
sounds whenever a menu rerenders, or every control bouncing continuously. Do not
animate a portrait or generate new facial poses when a panel/selection response
meets the interaction need. Apply the same feedback contract across keyboard,
controller and touch, which may have no hover state.

## Material-specific effects

Choose effects from the art direction and the physical/material event. A muzzle
flash, smoke, dust, liquid spatter and absorbed ink have different motion and
settled states. One expanding particle cloud should not stand in for all of them.
Colour exceptions, gore and print-specific treatments remain project decisions.

For an ink-on-paper effect, separate a brief directional deposition from slow
absorption and the persistent stain. A simplified deterministic model can move
moisture and mobile pigment across a fixed fibre/capacity field, deposit colour,
and dry. Irregular connected lobes and a thin feathered margin can suggest
seepage; uniform radial scaling and frame-randomized noise usually cannot.

Curtis et al.'s watercolor work is a useful reference for fluid, capillary and
deposition layers, not proof that every dye ink or blood behaves identically.
Paper/medium determine edge pooling and spread; do not prescribe a dark ring or
wet-on-wet bloom universally. Keep hidden flow structure distinct from visible
whole-page grain, especially when the style requires clean whitespace.

Simple deterministic build-time baking may be enough: a small atlas with a
stable final frame, or arrival-time/concentration textures. Compare with real
material references and inspect the actual assembled result before adopting a
more expensive live solver or generative-video workflow. Preserve linework and
correct world registration where appropriate. These technical recipes do not
approve the artwork or grant rights to copied code/reference footage.
