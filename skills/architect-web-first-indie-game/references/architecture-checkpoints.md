# Architecture checkpoints

Use these checkpoints before content makes a foundational choice expensive.

## Renderer choice

Record the maximum representative scene, not an average screenshot:

- visible interactive/animated objects and frames per object;
- transparent, masked, lit, shadowed, or refracted layers;
- terrain/world extent and camera zoom range;
- text labels and UI compositing;
- render-target and post-processing needs; and
- lowest intended browser/device.

Build one stress scene containing the expected interaction of systems. A
benchmark of isolated sprites will not reveal overdraw, sorting, masks, text,
or upload stalls. Record CPU preparation, GPU/draw submission, asset decoding,
and worst-frame time separately.

Prefer DOM/CSS or SVG when accessibility, rich text, document flow, or a modest
board-like interface is central. Prefer Canvas for genuinely modest custom 2D
workloads and fast iteration. Prefer a GPU path when batching, masks, lighting,
huge layers, or high object counts are core rather than hypothetical. If using an engine, verify its web export,
loading, input, localization, save, and desktop-wrapper behavior with a real
build before committing.

## Domain boundary

The simulation should be runnable without a renderer. Favor pure operations
for eligibility, scoring, state transitions, migrations, and deterministic
selection. Rendering may consume snapshots and maintain rebuildable caches; it
must not become the authority for game facts.

Define canonical IDs for every durable entity at creation. Search every
producer and consumer when introducing an entity type: runtime state, saves,
quests, events, UI selection, telemetry, generated indexes, tests, and content
tools. Reject duplicate IDs and unresolved references at construction or load.

## Platform boundary

Define narrow services for storage, achievements, activity, window lifecycle,
store APIs, and file export. Provide browser and desktop implementations behind
the same contract. Keep unsupported features explicit rather than returning a
plausible fake success.

## Asset boundary

Keep authored sources separate from generated outputs. Give build tools a
deterministic input manifest and validate output dimensions, IDs, required
variants, and provenance. A generated file checked into source control should
still be reproducible.

## Review triggers

Revisit architecture when measured evidence crosses a recorded threshold:

- representative worst-frame time misses the device budget;
- loading or memory exceeds the deployment target;
- a platform adapter leaks into domain modules repeatedly;
- save migrations cannot express a content change cleanly; or
- localization/input requirements require duplicated presentation paths.

Do not schedule a rewrite merely because a different technology is fashionable.
