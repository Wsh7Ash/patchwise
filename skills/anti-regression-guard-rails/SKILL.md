---
name: anti-regression-guard-rails
description: "Install mechanical guard rails that prevent this defect class from re-appearing, so the fix's protection does not rest on the next reviewer's memory. Use when the defect class is mechanically checkable, by lint rule, schema constraint, type system or test template"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [guard-rails, lint, prevention]
---

# Anti-Regression Guard Rails

> Install mechanical guard rails that prevent this defect class from re-appearing, so the fix's protection does not rest on the next reviewer's memory

**Use when:** the defect class is mechanically checkable, by lint rule, schema constraint, type system or test template

**Avoid when:** the defect is an engineering judgment question, which guard rails cannot mechanically verify and no rule should pretend to encode

## Procedure

1. Express the defect class as a mechanical rule: what constitutes violation, what constitutes compliance, and what is out of scope.
2. Choose the enforcement point: lint rule, static analysis, type-level constraint, database default, CI check or test scaffold.
3. Implement the rule with a suppression mechanism for genuine exceptions, so suppressions are explicit and reviewable.
4. Run it against the fixed code first, then against the codebase's clean history and report how many pre-existing violations it catches.
5. Tune it against false positives and record the tuning margin, so later engineers can see the guard's precision claims honestly.
6. Document the rule next to the finding's closure, so its existence is tied to the defect class rather than to a personal preference.

## Signals

- The rule fires on every pre-existing file, indicating the defect class is systemic rather than the single fix's surface.
- Suppression exceptions accumulate, which means the rule is too broad rather than suppressions being justified case by case.
- The rule would have flagged the defect pattern in earlier commits, proving the class was already present much earlier.
- Compliance drifts because the rule lives only in CI and not in pre-commit or editor tooling, so developers find out late.

## Patterns

**1. Rule form**

```text
lint rule: forbid queries touching orders without owner scoping -> custom rule in lint config
```

**2. Retrofire**

```text
rule against last 6 months: 3 pre-existing violations -> filed as sibling defects
```

**3. Suppression**

```text
allow with // reason: <line> -> suppressions reviewed at each PR
```

## Pitfalls

- Writing the rule so broad that legitimate uses always trigger, which causes developers to blanket-disable it when nobody is watching.
- Implementing the rule but leaving its enforcement optional, so refactors quietly drop the coverage line.
- Assuming the guard rail covers the class when it actually detects only one variant out of several forms.

## Done when

- A mechanical guard rail exists in the pipeline, demonstrates evasion against historical findings, and carries documented tuning.
- The suppression mechanism is explicit in the documentation, and suppressions are auditable during review.

## References

- [OWASP - secure coding practices checklist](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [CWE - preventive design guidance](https://cwe.mitre.org/)
