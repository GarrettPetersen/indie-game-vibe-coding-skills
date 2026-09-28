# Pixel-art resolution and no-mixels contract

Choose the logical render width and height before producing substantial art or
UI. Test a representative scene, readable text, and touch controls at that size
before committing. Record the base pixel density, sprite/tile conventions,
camera zoom steps, safe areas, and supported aspect-ratio behavior in one
authoritative rendering contract. Do not prescribe one resolution for every
genre or target device.

## Work within the logical grid

- Compose artwork, UI, effects, and bitmap fonts in logical game pixels. Prefer
  one base pixel density; any intentional coarser style layer must be documented
  and consistently applied rather than produced by arbitrary asset scaling.
- Import assets at the intended density. Re-author or deliberately resample
  mismatched assets in the asset pipeline; do not silently stretch them at
  runtime until they fit. Retain originals and reproducible conversion settings.
- Use exact positive integer enlargement and grid-snapped origins for bitmap
  artwork and fonts. Disable smoothing in every intermediate texture, canvas,
  and composition pass. Avoid fractional scale, shear, and arbitrary rotation
  that break the pixel grid; use authored frames or grid-aware effects instead.
- Keep simulation and camera state precise, then snap at the rendering boundary.
  Do not quantize physics or change gameplay to make a sprite look aligned.
- Reflow, wrap, paginate, or change logical layout when text does not fit. Never
  shrink one bitmap label fractionally as a layout repair.

## Separate internal rendering from display scaling

Logical screen dimensions are not physical monitor dimensions. Choose either a
fixed logical viewport or a documented responsive logical-grid policy. Resize
through that single policy; sprites and text still use the same pixel units.
Map pointer/touch coordinates back through the viewport transform and offsets.

Integer nearest-neighbor final enlargement gives uniform physical pixel blocks,
but can require letterboxing. Continuous full-window scaling trades that physical
uniformity for window coverage. Make this an explicit product choice and preserve
an existing choice; neither permits inconsistent internal asset scaling. Account
for browser zoom and device pixel ratio without changing the logical art density.

A separate high-legibility text mode may use smoothly rendered scalable fonts
without rescaling pixel artwork or weakening the default bitmap-text contract.
Its layout and hit targets must remain correct.

Verify representative scenes at native logical size and supported display sizes,
camera positions/zooms, intermediate render passes, and aspect ratios. Include
automated transform/font contracts and screenshot inspection for mixed density,
blur, seams, and misaligned hit targets. Treat resolution changes as coordinated
layout/asset-pipeline changes, not scattered replacement of magic numbers.
