---
name: failure-open-vs-failure-closed-audit
description: "Audit what happens when every component of the fixed decision fails, since a fix that fails open has already conceded to the next outage. Use when the fix depends on external systems: identity providers, policy engines, caches or feature flags that can be unavailable"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [fail-open, availability, audit]
---

# Failure-Open vs Failure-Closed Audit

> Audit what happens when every component of the fixed decision fails, since a fix that fails open has already conceded to the next outage

**Use when:** the fix depends on external systems: identity providers, policy engines, caches or feature flags that can be unavailable

**Avoid when:** the fix is a pure data-local change with no external dependency in the decision path

## Procedure

1. Enumerate the dependencies of the fixed decision: identity source, policy store, cache, external service and configuration layer.
2. For each one, simulate unavailability and observe whether the decision path denies or grants access.
3. For each degraded mode, check whether the degradation is silent or produces an error that still returns data.
4. Set the desired default for each failure mode: a correct one is denying access, never assuming permission.
5. Add tests that pin failure behavior for each dependency so a future refactor cannot quietly invert the default.
6. Record every dependency's failure mode in the fix documentation, so operators know what an outage means for this surface.

## Signals

- The policy cache falls back to a default that happens to permit access, meaning the fix silently fails open.
- An identity provider downtime produces a generic error that returns data anyway, reconstructing the exploited flow precisely.
- One dependency's failure is acceptable because the fix was designed to deny rather than grant on it.
- The fix includes a dependency whose availability is never sampled, so failure behavior is unknown until production outage tests it.

## Patterns

**1. Dependency list**

```text
decision deps: authn(S) + policy(P) + cache(C) + config(K)
```

**2. Failure probe**

```text
S down -> deny\nP down -> deny\nC stale -> deny until refreshed\nK down -> forbid
```

**3. Pinned check**

```text
test: P returns 503 -> expect 403 not 200 -> pin
```

## Pitfalls

- Assuming a fix that performs in the happy path will fail sensibly when its dependencies are unreachable.
- Designating failure-open as acceptable for convenience, which recreates the vulnerability for anyone who can cause the failure
- Testing dependencies one at a time and never producing a combined failure behavior for two outages concurrently.

## Done when

- Every dependency failure mode has been probed and either denied or explicitly accepted, and no path silently grants access.
- Tests and documentation pin those failure behaviors so a future maintainer cannot accidentally invert them.

## References

- [OWASP - error handling cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Error_Handling_Cheat_Sheet.html)
- [NIST SP 800-53 - access control failure handling](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
