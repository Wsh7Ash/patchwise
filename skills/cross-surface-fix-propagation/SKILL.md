---
name: cross-surface-fix-propagation
description: "Carry the fix across every surface that computes the same decision: clients, services, scripts and cached policy, since one surface is never the whole story. Use when the same decision logic appears in several places: API handler, background job, CLI utility, mobile client or generated policy"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [propagation, consistency, fix]
---

# Cross-Surface Fix Propagation

> Carry the fix across every surface that computes the same decision: clients, services, scripts and cached policy, since one surface is never the whole story

**Use when:** the same decision logic appears in several places: API handler, background job, CLI utility, mobile client or generated policy

**Avoid when:** the decision logic lives in exactly one authoritative place and every caller provably goes through it

## Procedure

1. Search every surface that contains the decision logic, including generated files, vendored code and client-side bundles.
2. Classify each surface: authoritative server, derived client mirror, unrelated coincidence or stale copy that should be deleted.
3. Fix the authoritative surface first, then every mirror that can act on the same data, even the ones shipped to compiled clients.
4. Confirm generation pipelines re-run, since policy files or client bundles regenerate only when the pipeline draws them.
5. Ship client binaries in a separate release if compiled mirrors cannot be patched idempotently.
6. Record each surface and its change status, so the fix's completeness claims are enumerable rather than gestured at.

## Signals

- The fix applies server-side but the mobile client still enforces the outdated rule, producing two divergent views of the same object.
- A background job computes the same check in its own copy and still authorizes what the primary fix now refuses.
- Generated policy files stay unchanged because the generator's input was updated without re-running the generation.
- A vendored copy exists whose upstream is long dead, meaning the surface should be deleted rather than patched.

## Patterns

**1. Surface row**

```text
surfaces: server handler (fixed), worker copy (fixed), mobile mirror (release pending), generated policy (regen done)
```

**2. Coincidence check**

```text
decide: mobile uses server response not local copy -> mirror wasted, file removal next release
```

**3. Release row**

```text
client release ships the same calendar day or the server fix alone leaves the decision reachable
```

## Pitfalls

- Fixing the server surface and calling propagation done, which classically leaves the worker job as the next breach path.
- Assuming clients cannot act on a mirror, especially when the mirror is a cache of what the client authorizes on its own UI.
- Skipping regeneration because policy files look deterministic, while the generator's cache hides its real inputs.

## Done when

- Every surface carrying the decision is enumerated and classified, and each one's fix status is recorded explicitly.
- Regenerate or release follow-ups are scheduled, not merely discussed, so mirrored clients converge on the same behavior.

## References

- [OWASP - API security guidance](https://owasp.org/www-project-api-security/)
- [OWASP - mobile testing guideline](https://owasp.org/www-project-mobile-security-testing-guide/)
