# ADR-003: Seven cases carry no org verdict, and stay in the corpus anyway

## Status

**Accepted**

## Date

2026-08-19

## Author

Mustafa Aksu

## Context

Seven of the twenty-eight cases cannot be settled by running them. They assert
what an **analyzer must report** — an honest unknown for dynamic SOQL, or for a
SOSL with no `RETURNING` clause, or a hand-off the analyzer does not follow —
rather than what the platform does.

Executing that code simply runs the query. It cannot pronounce on whether the
analyzer was right to call its own knowledge incomplete.

> **An org cannot measure the absence of an analyzer's knowledge.**

Two tidy options present themselves, and both distort the result. Counting these
as gaps overstates what is missing. Counting them as measured overstates what is
proven.

## Decision

Publish them in a **separate list**, `not_adjudicable_cases`, labelled as such
and **excluded from the adjudicated set**.

They remain in the corpus because the behaviour they describe is worth scoring —
an analyzer that silently guesses at dynamic SOQL is doing something worse than
one that reports an unknown, and there has to be somewhere that distinction is
written down.

## Alternatives Considered

- **Drop them.** Rejected: the corpus would lose the cases that test whether a
  tool admits what it cannot see, which is a real quality of a tool.
- **Assign an expected outcome by reasoning and fold them in.** Rejected: it
  would smuggle seven reasoned labels into a set described as org-adjudicated,
  which is exactly the mirror ADR-001 exists to avoid.
- **Keep them in the same list with a flag.** Rejected: a flag inside a list
  called "adjudicated" is read past. A separate list cannot be.

## Consequences

- The headline number is honest: **21 adjudicated of 28 published**, and the gap
  is explained rather than absorbed.
- Anyone scoring a tool chooses knowingly whether to include them, and on what
  basis.
- An org verdict for one of the seven would be a genuine contribution, and
  `CONTRIBUTING.md` names it as the second most valuable thing anyone could send.
  I would rather be wrong about the label than keep it.

## References

- README — "Seven cases carry no org verdict — on purpose"
- `corpus.json` — `not_adjudicable_cases`, `counts`
