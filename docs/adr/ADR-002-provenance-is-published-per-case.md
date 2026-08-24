# ADR-002: Every case publishes where its ground truth came from

## Status

**Accepted**

## Date

2026-08-19

## Author

Mustafa Aksu

## Context

ADR-001 makes a live org the oracle. Not every case reaches that bar. Some
describe documented semantics that were not re-measured here; a few rest on
reasoning alone.

A corpus that presents all of its labels with equal confidence hides exactly the
information a sceptical reader needs. The pass rate a tool scores against it
would then be a number whose meaning varies case by case, invisibly.

## Decision

Every case carries a **`label_provenance`** field naming its source, on a scale
that states its own strength:

| provenance | meaning | strength |
|---|---|---|
| `experiment:E<n>` | measured in a real org during the original experiments | strongest |
| `experiment:oracle` | measured by the runtime oracle — deployed, executed as the modelled user | strongest |
| `platform-doc` | documented semantics, **not measured here** | weak |
| `reasoned` | my own reasoning — proves consistency, **not correctness** | weakest |

Current distribution: **21 experiment · 3 platform-doc · 4 reasoned.**

The README says this outright: provenance is the benchmark's real quality
metric, more than any pass rate. **The weak labels are published, not hidden**,
and describing my own reasoning as the weakest evidence in the corpus is the
point of the scale rather than a caveat attached to it.

## Alternatives Considered

- **Publish only the measured cases.** Rejected: it would shrink the corpus to
  its strongest third and lose the shapes that are worth scoring even when the
  expected outcome is weaker.
- **Present all labels uniformly.** Rejected: it inflates confidence in seven of
  twenty-eight cases without saying so.
- **A numeric confidence score.** Rejected: a number invites averaging, and
  "measured" versus "reasoned" is a difference in kind, not in degree.

## Consequences

- A reader can discount the corpus exactly as far as the evidence warrants, per
  case, without asking anyone.
- Improving a weak label is a well-defined contribution: measure it, and it moves
  up the scale.
- CI checks that `label_provenance` holds a recognised value and that the
  distribution quoted in the README still matches the data — because prose that
  restates data is a copy that can drift.

## References

- README — provenance table under "Why it exists"
- `corpus.json` — `label_provenance` on every case
- `.github/workflows/ci.yml` — provenance validation
