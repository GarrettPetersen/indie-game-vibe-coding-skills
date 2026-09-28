# Credit record contract

## Stable fields

Each asset record should be able to answer:

| Field | Purpose |
|---|---|
| `id` | Stable internal identity; never derived from display credit |
| `category` | Art, audio, font, code, data, model, middleware, or other |
| `credit` | Exact player-visible line |
| `creator` | Credited person or organization |
| `source` | Canonical URL or non-sensitive internal evidence reference |
| `license` | License name/version or contract grant |
| `requirements` | Attribution, notice, share-alike, source, or redistribution terms |
| `acquired` | Date and provenance evidence |
| `files` | Source and shipped repository paths |
| `derivatives` | Conversion/modification chain and tools |
| `usage` | Game build, demo, website, trailer, store media, or tooling |
| `notes` | Material restrictions or review status |

The storage format may be Markdown, YAML, JSON, or a database, but there must be
one public-safe authority and a deterministic way to produce public credits.
Sensitive receipts, contracts, and consent evidence belong in a private store
linked by the same stable ID, not duplicated into the public ledger.

## People records

Track role, approved display name, public-credit consent, and affected release
in the appropriate public/private records. Keep private contact and contract
data outside the public ledger. A public credits generator consumes only the
approved public entries; it must reject an entry with no public display name
and never substitute an email or account handle.

Only affirmative public entries appear in the checked-in public ledger. Track
the evidence and any later withdrawal privately; regenerate future credits
after a correction or withdrawal.

## Asset addition workflow

1. Verify origin and commercial/redistribution rights.
2. Assign the stable credit entry ID.
3. Store or link provenance evidence.
4. Add source and derivative paths.
5. Add exact required notice/credit text.
6. Verify the production build uses only recorded derivatives.
7. Render/audit the public credits output.

For purchased audio, also verify modification, commercial game, trailer/video,
streaming/Content ID, and source-file redistribution terms. Preserve a license
snapshot or hash/version reference so later storefront edits do not silently
change the recorded grant.

## Release audit inputs

Use the production bundle or asset manifest, package-lock/dependency license
report, storefront/trailer media sources, and public credit output. Source-tree
search alone will miss generated or copied artifacts and will include unused
development files.
