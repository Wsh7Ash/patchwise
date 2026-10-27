---
name: fix-effectiveness-verification
description: "Re-run the exact reproduction against the deployed fix with the same credentials, and record the pass result as evidence rather than memory. Use when the deployed fix must be proven ineffective against the original exploit path before anyone calls the defect closed"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [verification, reproduction, closure]
---

# Fix Effectiveness Verification

> Re-run the exact reproduction against the deployed fix with the same credentials, and record the pass result as evidence rather than memory

**Use when:** the deployed fix must be proven ineffective against the original exploit path before anyone calls the defect closed

**Avoid when:** fix verification is already covered by CI running the negative test with the production-like build artifact

## Procedure

1. Fire the original reproduction, unchanged, against the deployed environment now carrying the fix.
2. Record the whole response: status, headers, body and any side effects that did or did not occur.
3. Re-run after the propagation or cache invalidation window the deployment pipeline promises, not just the first-testing minute.
4. Verify the response carries the expected refusal rather than merely a different status code, since content still leaks under some refusals.
5. Repeat the same sequence on each surface the fix was expected to land on: API, web, background, mobile-reachable endpoints.
6. Record each result as a row in the verification statement, with the exact request, timestamp and environment version attached.

## Signals

- The status changes but the body still leaks the sensitive content, so the update was partial rather than complete.
- The first run refuses while one replica still serves the old build, which indicates rolling deploys left stragglers.
- A surface that the fix list covered still returns the vulnerable behavior, which is a missing deployment slice or client release.
- Verification succeeds through the test harness but fails when captured exactly as the exploit originally sent it.

## Patterns

**1. Repro rerun**

```text
GET /api/orders/{peer} with bob's token -> 403 -> no data in body -> closed
```

**2. Replica sweep**

```text
instances 1..4: 403,403,200,403 -> instance 3 missed rollout -> re-roll
```

**3. Record block**

```text
verified 2026-10-31 09:12, request snapshot attached, environment v2.3.2-oct31
```

## Pitfalls

- Recording 'verified' from a status code while the body carries leaked content across several surfaces.
- Skipping replica sweeps, allowing stragglers to persist in the deployed environment after the fix pipeline succeeds.
- Using a re-created reproduction instead of the original one, accidentally testing the desired behavior rather than the actual defect.

## Done when

- The original reproduction fails against every deployed surface with full response evidence preserved.
- Verification statements carry timestamps and environment versions so the closure is auditable rather than memorable.

## References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [NIST SP 800-53 - verification control references](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
