---
name: maintain-game-credits
description: Create, update, or audit a game's authoritative credits and asset-provenance ledger. Use whenever third-party or attribution-bearing art, audio, fonts, code, data, models, translations, commissioned work, middleware, or playtester contributions enter the project, and before a release credit audit; do not publish private contact details or infer permission to credit a real person.
---

# Maintain Game Credits

Record provenance when an asset or contribution enters the repository. Waiting
until release turns ordinary bookkeeping into archaeology and risks missing
attribution, license terms, source files, or people.

## Keep one authoritative ledger

Find the project's existing credits/provenance file. If none exists, adapt
[assets/CREDITS.md](assets/CREDITS.md) rather than inventing several competing
lists. Generated in-game credits may consume this ledger, but must not become a
second manually maintained source of truth.

Keep stable entry IDs independent of display credit text. Renaming a file,
moving an asset, or changing the public credit must not create a duplicate
record. The checked-in ledger contains public-safe provenance and drives public
credits. Link sensitive receipts, contracts, and consent evidence by stable ID
from a private evidence store; do not copy them into a public repository.

Read [references/credit-record-contract.md](references/credit-record-contract.md)
before adding a new category or auditing a release.

## Record assets immediately

For every external or commissioned asset, record:

- stable asset/entry ID and category;
- exact public credit line;
- creator or organization;
- canonical source URL or non-sensitive acquisition reference;
- license/version and required attribution wording;
- date acquired and evidence retained;
- repository files and generated derivatives;
- modifications and tools used;
- redistribution/source-file restrictions; and
- for audio, modification rights, trailer/marketing use, source-file
  redistribution limits, and known automated Content ID implications; and
- whether the asset appears in shipped builds, marketing-only material, or
  development tools.

Do not add an asset when its license or provenance is unknown. A downloaded
file, marketplace receipt, permissive-looking repository, or AI tool output is
not self-documenting. Preserve local evidence where the license permits.

## Credit people without exposing them

Ask for or locate the exact public display name a contributor has approved.
Never publish email addresses, account IDs, contracts, payment details, or
private correspondence. Do not infer that submitting feedback grants public
credit permission.

Track playtesters, translators, voice performers, contractors, and special
thanks independently from asset licenses. If a person declines public credit,
record the private consent state outside the public ledger as appropriate; do
not insert a placeholder that identifies them indirectly.

Define how a person can correct or withdraw a public credit. Remove or revise
future public output while retaining only the private evidence required by the
project's legal/accounting obligations.

## Keep derivatives traceable

When an asset is cropped, recolored, retopologized, converted, baked, mixed,
upscaled, or incorporated into an atlas, link every shipped derivative to the
source entry. Keep generated output reproducible where practical. Record all
material sources in a composite, not only the final assembly tool.

## Audit before release

Compare the ledger against the actual production asset graph, dependency
licenses, content directories, build manifests, storefront media, and in-game
credits. Flag:

- shipped files with no ledger entry;
- ledger entries whose files no longer ship;
- missing required attribution or license text;
- unclear commercial or redistribution rights;
- derivatives disconnected from their source;
- duplicate people or inconsistent display names; and
- personal contact details in public output.

Do not mark the audit complete from the ledger alone. Verify what the build
actually contains and what the player can read.

## Completion rule

When a task introduces an asset or contributor, it is incomplete until the
authoritative ledger and player-visible credits, when applicable, are updated
and committed with the feature. Do not defer the entry to a release checklist.
