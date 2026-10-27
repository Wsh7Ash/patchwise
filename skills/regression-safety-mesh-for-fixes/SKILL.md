---
name: regression-safety-mesh-for-fixes
description: "Build the safety net before the fix lands: negative and positive tests that capture both the vulnerable behavior and the legitimate behavior the fix must preserve. Use when a confirmed defect is heading toward a code change and neither the bug nor the working behavior is yet pinned by tests"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [tests, regression, fix]
---

# Regression Safety Mesh for Fixes

> Build the safety net before the fix lands: negative and positive tests that capture both the vulnerable behavior and the legitimate behavior the fix must preserve

**Use when:** a confirmed defect is heading toward a code change and neither the bug nor the working behavior is yet pinned by tests

**Avoid when:** the fix is a one-line configuration revert already covered by existing tests

## Procedure

1. Write the failing test that reproduces the defect against the current code, and run it once to see it fail for the right reason.
2. Write the passing test for the legitimate behavior the fix must not break using the exact same access surface.
3. Compare the two tests: they should differ only in the precondition that triggers the defect, which is the boundary the fix must locate.
4. Add boundary cases just outside the fix: slightly-off inputs that should behave like the legitimate side, not like the defect.
5. Run the full suite before and after the fix, recording which other tests changed and why.
6. Commit the tests together with the fix, with a commit message that names the defect and the preserved behavior.

## Signals

- The negative test fails in a way that also fails on an unrelated code path, indicating the test is broader than the defect.
- The fix makes the positive test fail alongside the negative, meaning the fix boundary is too coarse.
- Boundary cases behave the same before and after the fix, so the fix's effect is invisible at the edges.
- Unrelated tests change because the fix touched a shared layer they depend on, which must be reviewed before merging.

## Patterns

**1. Negative test**

```text
test_dependency_fix: non-owner writes -> asserts refused (403) currently fails
```

**2. Positive test**

```text
test_passing_access: owner writes same surface -> asserts accepted, agrees with behavior
```

**3. Boundary test**

```text
just-outside: no permission but differently-shaped input -> stays on legitimate path
```

## Pitfalls

- Writing only the negative test, which proves nothing about what the fix removed from the successful code path.
- Allowing the positive test to co-vary with preconditions so loosened by oversight that both pass before the fix arrives.
- Shipping the fix without running the full suite, catching only the targeted behavior while breaking neighbors.

## Done when

- Negative, positive and boundary tests exist, the negative one fails before the fix and nothing else regresses.
- Test and fix land in one commit with the defect identifier, so reverts and archaeology retain their context.

## References

- [OWASP - testing guidance for fixes](https://owasp.org/)
- [NIST SP 800-53 control references for testing](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
