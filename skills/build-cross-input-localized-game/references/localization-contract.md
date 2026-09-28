# Localization contract

## Source catalog

Maintain one extractable source catalog for player-facing text. Register full
dynamic templates with named or indexed placeholders. Keep developer
diagnostics separate from player prose.

The build should fail on:

- missing locale entries;
- placeholder mismatch;
- duplicate/conflicting keys;
- unsupported glyphs in required fonts;
- runtime-only English that bypasses extraction; and
- accidental regeneration churn outside the changed source text.

## Generation and review

Machine translation is a draft. Review every new or changed string in context,
with special attention to controls, legal or other domain-specific terms,
proper names, polysemy, grammatical gender, number,
capitalization, and placeholders.

Store reviewed terminology and corrections in an editable glossary or override
source so regeneration preserves them. Do not freeze generated output by hand
while leaving the generator wrong.

## Layout

Test high-expansion languages, dense scripts, long binding labels, and locales
with different line metrics. Verify wrap, truncation, scrolling, pagination,
tooltip placement, button sizing, and collision with persistent UI.

For CJK locales, use language-appropriate line breaking rather than splitting
arbitrarily between code points. Test prohibited line-start/line-end
punctuation, mixed Latin/CJK metrics, fallback fonts, and full-width
punctuation. Confirm platform conventions for confirm/cancel controls instead
of assuming one controller layout is universal.

Bitmap/pixel fonts must render at supported native sizes or integer logical
scales if the art direction depends on crisp pixels. Change the layout rather
than stretching or fractionally scaling text to fit. An explicitly separate
high-legibility text mode may use an accessible scalable renderer, but must
retain content, hierarchy, clipping, focus, and hit targets.

## Runtime

Exercise the actual localized flow when text depends on dynamic state or input
bindings. A catalog coverage test does not prove that the runtime branch uses
the translated template or that the result fits.
