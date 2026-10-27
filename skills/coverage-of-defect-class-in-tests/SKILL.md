---
name: coverage-of-defect-class-in-tests
description: "Place the defect class into the test suite itself, ensuring the class's every future new variant triggers an existing test seam. Use when the fix is verified and the test suite should be extended to catch the class systematically rather than only the current instance"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [tests, coverage, hardening]
---

# Coverage of Defect Class in Tests

> Place the defect class into the test suite itself, ensuring the class's every future new variant triggers an existing test seam

**Use when:** the fix is verified and the test suite should be extended to catch the class systematically rather than only the current instance

**Avoid when:** the class is already encoded in a parametrized test fixture that new variants automatically flow through

## Procedure

1. Inventory the current test suite's coverage of this defect class across unit, integration and end-to-end layers.
2. Posit the variants a future regression could take: different entry point, different ordering, different data shape.
3. Design the test seam: a fixture, parametrization or template that lets future variants plug into the existing harness.
4. Write a test for each plausible variant under the general harness, not merely the instance that was fixed.
5. Prove the variants' coverage by mutating the fixed code deliberately and measuring every test that fires.
6. Record the class coverage in the defect record, so the test seam and variants are explicit for future maintainers.

## Signals

- Mutation of the fixed code fires only one test, so class coverage is thinner than the suite's overall appearance implies.
- Similar tests exist but none embody a parametrization, meaning each new variant would become a fresh copy-paste.
- A test exists for the class but lives in an integration suite that runs nightly rather than on merge, so bugs traverse reviews.
- The class is covered but only against positive cases, and the negative-case harness remains bypassed.

## Patterns

**1. Coverage row**

```text
class coverage: unit 2/5 variants, integ 0/5, e2e 1/5
```

**2. Harness row**

```text
parametrize fixture: ownership-variants(id=peer, ordering, bulk, nested) -> one insertion
```

**3. Mutation proof**

```text
mutation: remove ownership filter -> fires 7 of 10 added tests
```

## Pitfalls

- Adding only the test for the fixed instance, which is really the regression test rather than the class coverage.
- Placing class coverage in a slow suite, so the class is checked daily but never at the merge boundary.
- Assuming coverage without mutation proof, which tends to mislead both the engineer and the postmortem.

## Done when

- The class coverage is accounted across suite layers, the harness accepts plausible variants and a mutation run proves the tests fire.
- The coverage ledger is documented in the defect record so future maintainers can find the seam.

## References

- [OWASP - testing integration guidance](https://owasp.org/)
- [Google testing blog - mutation testing reference](https://testing.googleblog.com/)
