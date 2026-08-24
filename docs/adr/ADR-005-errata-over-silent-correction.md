# ADR-005: A wrong published value gets an errata entry and a version bump, never a quiet fix

## Status

**Accepted**

## Date

2026-08-19 (established by the v1.1 correction)

## Author

Mustafa Aksu

## Context

v1.0 published a defect. `org_verdict.bounded_by_running_user` was derived as
*"the field did not come back"* for **every** case — which is correct only for
the field-level-security read cases.

The record-axis cases observe **rows**, not a field. The write and publish cases
observe whether the **DML landed**. None of those carries a "field came back"
observation, so all of them were published as `bounded: true`, including v58
cases whose own rationale says the operation lands past the user. Worse, the two
read cases that return a field the user **is** entitled to — the negative control
— were published as an escape.

Five of twenty-one adjudicated cases were wrong in a scoring field.

The convenient response is to fix the derivation and republish. Anyone who had
already scored a tool against v1.0 would then hold results they could not
reconcile with the corpus, with nothing to tell them why.

> A benchmark that changes silently is not a benchmark either.

## Decision

Every change to a published value is recorded in an **Errata**, in the README
and in `corpus.json` under `errata`, **in the same words**, with a version bump.

The v1.1 entry names all five affected ids in a table — old value, new value, and
why — and separately lists the changes that touched presentation without altering
a measured outcome: per-axis derivation, two renamed ids, a provenance spelling,
and rationale text stripped of the exporting project's vocabulary. It closes by
stating what did **not** change: no case source, no API version, no fixture, no
measured outcome, and identical case-file hashes for every unrenamed case.

The README carries the pointer at the very top, above the description of the
benchmark itself, so a reader holding an older copy sees it before comparing
anything.

## Alternatives Considered

- **Fix it silently.** Rejected: it invalidates other people's results with no
  way for them to know.
- **Fix it and note it in the commit message.** Rejected: a commit message is not
  where someone holding a downloaded copy looks.
- **Withdraw v1.0 entirely.** Rejected: the defect was in one derived field, not
  in the measurements. Withdrawing would discard twenty-one correct observations
  to correct a derivation.
- **Keep both versions live.** Rejected: two current corpora is worse than one
  corpus with a stated history.

## Consequences

- A reader holding v1.0 can tell exactly what moved and decide whether it affects
  their result.
- The corpus's hashes changing between versions is the intended signal rather
  than an inconvenience (ADR-004).
- The project publishes its own defect prominently, and that is the correct cost
  of a benchmark whose whole claim is that it can be trusted without trusting the
  author.

## References

- README — "Errata", and the pointer in the opening lines
- `corpus.json` — `errata`, `version`
