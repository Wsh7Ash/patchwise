---
name: minimal-fix-boundary-defining
description: "Define the smallest change that neutralizes the defect and write down explicitly what it does not attempt to change. Use when a defect sits in shared code and it is tempting to refactor broadly while the fix is in shopping distance"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [fix, scope, minimal-change]
---

# Minimal Fix Boundary Defining

> Define the smallest change that neutralizes the defect and write down explicitly what it does not attempt to change

**Use when:** a defect sits in shared code and it is tempting to refactor broadly while the fix is in shopping distance

**Avoid when:** the defective element is a self-contained leaf with no shared surface to accidentally modify

## Procedure

1. Trace the defect to the exact decision point: the branch, query, check or configuration line that permits the wrong behavior.
2. Write the fix hypothesis in one sentence: what the change does and which precondition it must not disturb.
3. Enumerate every leg that shares that decision point, since a minimal change must leave them behaving exactly as before.
4. Choose the smallest edit that moves the negative test to pass without touching the positive test's outcome.
5. Resist incidental improvements: log them instead, or they become an unpaid-for regression surface inside your fix.
6. Record the boundary in the pull request description: what changed, what deliberately did not, and why each out-of-scope item waited.

## Signals

- Two unrelated behaviors branch at the same decision point, so the minimal fix must distinguish them from the change itself.
- The fix hypothesis actually needs a second edit somewhere else, which means the boundary was drawn too early.
- Reviewers request a nicer structure inside the fix, which is a separate change with its own review cost.
- One function carries half of the deployment risk while carrying none of the defect behavior, meaning the fix drifted.

## Patterns

**1. Decision point**

```text
authz check in orders.py:97 allows when membership missing -> branch wrong; fix=ownership lookup
```

**2. Shared legs**

```text
same branch used by export path -> must remain identical -> verified via positive test
```

**3. PR boundary**

```text
changed: orders.py:97 only\nnot changed: refactor proposals filed separately
```

## Pitfalls

- Fixing the symptom at the call site instead of the decision point, leaving other callers exposed to the same defect.
- Bundling refactor into the fix, which merges two independent review costs into one unreviewable change.
- Defining the boundary after writing the diff, which means the fix shaped the scope instead of the scope shaping the fix.

## Done when

- A one-sentence fix hypothesis exists and the diff matches it exactly, touching no more than the decision point requires.
- Shared behaviors are enumerated and protected by tests that pin their current behavior.

## References

- [NIST SP 800-53 control guidance](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
- [OWASP - secure coding guidance](https://owasp.org/)
