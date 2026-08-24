# ADR-001: The oracle is a live org, never my model of the rules

## Status

**Accepted**

## Date

2026-08-19 (decision from the original experiments; recorded retrospectively)

## Author

Mustafa Aksu

## Context

This corpus was built while writing an analyzer that resolves Apex execution
semantics — API version, sharing declaration, mode clauses — and it exists
because of the problem every such project hits:

> If I generate the expected outcomes from my own model of the rules, I am
> testing my implementation against a re-implementation of itself.

That is a **mirror, not an oracle.** It proves consistency and says nothing
about correctness. A benchmark built that way scores highest for the tool whose
assumptions it was derived from, which is the opposite of what a benchmark is
for.

## Decision

**The platform adjudicates.** For every case that a platform can settle, the
expected outcome is what a real org did — the class deployed at its stated API
version, executed as a user with stated permissions, against a stated fixture,
and the observation recorded.

Twenty-one of twenty-eight cases are settled that way. The remainder are
labelled honestly rather than filled in by reasoning (ADR-002, ADR-003).

## Alternatives Considered

- **Derive expected outcomes from the documentation.** Rejected as the primary
  source: documentation describes intent, and the cases that matter are the ones
  where behaviour and intent diverge. Three cases carry `platform-doc` precisely
  because they could not be measured, and they are marked weak.
- **Derive them from my analyzer.** Rejected — this is the mirror.
- **Derive them from another tool's output.** Rejected for the same reason, with
  the added problem of encoding a second tool's assumptions as ground truth.
- **Ask the platform vendor.** Not available, and it would still be intent
  rather than observation.

## Consequences

- Anyone can score any analyzer against this corpus **without trusting the
  author**, which is the only property that makes it useful to someone else.
- The corpus is bound to a release. It was measured on **Summer '26**, and a case
  that stops reproducing later is a finding rather than a defect.
- Measurement is expensive, which is why the corpus is twenty-eight cases around
  one object and one user rather than an exhaustive survey.
- Where a case could not be measured, that is visible in the data rather than
  smoothed over — the subject of ADR-002.

## References

- README — "Why it exists"
- `corpus.json` — `org_adjudicated_cases`, `fixture`, `measured_on`
