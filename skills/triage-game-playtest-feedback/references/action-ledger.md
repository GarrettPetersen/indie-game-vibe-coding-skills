# Playtest action ledger

## Source structure

Maintain raw reports separately from the deduplicated action ledger. Every
action links back to one or more source reports without copying private contact
details.

A useful action record contains:

- stable action ID;
- concise problem/outcome statement;
- source report IDs and dates;
- affected build/platform;
- status;
- verified current behavior and evidence;
- acceptance criteria;
- severity, frequency, confidence, and effort;
- owner or next decision when the project tracks ownership;
- fix/test/revision or change references; and
- explicit remaining gap.

## Status meanings

- **Open:** verified or accepted work remains.
- **Partial:** meaningful work shipped, with a precise residual gap.
- **Review:** optional design decision or insufficiently accepted proposal.
- **Blocked:** a concrete external dependency prevents progress; name it.
- **Implemented:** acceptance criteria are verified in the audited development
  revision but not yet confirmed in the intended tester/release channel.
- **Shipped:** the verified implementation is present in the intended
  tester/release channel; confirm its delivered revision when the channel
  exposes one.
- **Closed by design:** intentionally not changing; record the reason.
- **Stale:** applied to an old build and current behavior has been verified.

Do not use “Shipped” for code that has not been exercised through the relevant
player path. Do not leave an item “Open” merely because more polish is always
possible.

## Audit routine

Before prioritizing or declaring the ledger current:

1. compare its audited revision/change to the project's current source state;
2. inspect changes since that point for related implementation;
3. verify entries whose wording claims absence of an existing feature;
4. ensure partial entries state what remains rather than repeating history;
5. move completed work out of active counts;
6. update summary/callout text as well as the individual row; and
7. check that private emails, tokens, saves, and raw telemetry are absent.

## Completion discipline

When implementation and documentation are separate changes, the documentation
change must still reference the implementation revision/change. If the project
requires remote publication, confirm its authoritative remote contains both.
If deployment is in scope, verify the delivered build revision before
describing the player-facing release as shipped.
