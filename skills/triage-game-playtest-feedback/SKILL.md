---
name: triage-game-playtest-feedback
description: Reconcile game playtest reports with the current build, available telemetry, saves, and change history; deduplicate findings, distinguish bugs from stale reports and design proposals, prioritize high-impact fixes, and maintain a feedback action ledger. Use when importing feedback, asking what remains outstanding, choosing low-hanging fruit, verifying whether something was already fixed, or closing feedback after implementation; use an instrumentation skill to build telemetry or recovery infrastructure itself.
---

# Triage Game Playtest Feedback

Treat a playtester report as valuable evidence, not a perfectly scoped ticket.
Preserve what the player experienced while verifying the cause against the
exact build they played and the current code.

## Preserve the raw report

Store the original wording, date, build/revision when known, platform/runtime,
save or scenario reference, and consented telemetry correlation. Do not rewrite
the raw report into a conclusion and discard the evidence. Remove private
contact information from public or checked-in documents.

## Verify before classifying

Search the current implementation, recent revisions/changes, tests, feedback
ledger, and available consented telemetry. Reproduce on the reported build when practical, then check
the current build. A report may be:

- reproducible defect;
- fixed since the tester's build;
- stale or no longer applicable;
- partially implemented with a narrower remaining gap;
- discoverability/onboarding problem despite existing mechanics;
- optional design proposal;
- intentional design needing clearer communication; or
- unverified because required evidence is missing.

Do not mark a report fixed because code resembling a fix exists. Exercise the
player-visible path. Conversely, do not reimplement a feature because the
tester did not discover it; identify whether discoverability is the real gap.

Read [references/action-ledger.md](references/action-ledger.md) when creating or
auditing the project feedback document.

## Prioritize concretely

Rank by player harm, affected frequency, data-loss/crash risk, first-session
impact, demo/release relevance, confidence, implementation effort, regression
risk, and whether telemetry can verify the result.

Prefer low-risk fixes that unblock play, prevent loss, correct misleading UI,
or remove a repeated first-session trap. Do not let a pile of easy cosmetic
tasks displace one severe save-loss or progression blocker.

Separate acceptance criteria from the player's suggested solution. The report
defines the experienced problem; implementation should fix the violated
invariant with the smallest coherent design.

## Implement and close the loop

For a fix:

1. reproduce or establish the violated invariant;
2. add a regression test that fails for the cause;
3. implement the narrow coherent correction;
4. verify relevant saves, inputs, locales, layouts, and production recovery;
5. when suitable consented telemetry exists, review the affected build and
   define the post-fix signal;
6. when the project maintains a feedback ledger, update it with status, exact
   scope, remaining gap, tests, and revision/change reference; and
7. record and publish the change according to the project's version-control,
   authorization, and delivery workflow.

If the project defines ledger update and remote publication as part of
completion, do not call the task complete before both have occurred. Never
infer permission to deploy, message a tester, or publish private evidence.

## Report outstanding work honestly

When asked what remains, derive it from active ledger entries after auditing
recent commits. Show completed subtasks and the precise residual gap. Keep
“Review” proposals separate from committed “Open” defects so optional scope
does not masquerade as unfinished correctness work.
