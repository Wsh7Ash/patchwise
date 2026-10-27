---
name: replica-and-cache-version-sweep
description: "Verify every replica, cache layer and edge location runs the fixed build, since rollout success on one instance proves nothing about the fleet. Use when the deployment pipeline reports success but you must confirm the fleet, not merely the deployment job"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [rollout, replicas, fleet]
---

# Replica and Cache Version Sweep

> Verify every replica, cache layer and edge location runs the fixed build, since rollout success on one instance proves nothing about the fleet

**Use when:** the deployment pipeline reports success but you must confirm the fleet, not merely the deployment job

**Avoid when:** the deployment is a single instance with no cache or edge layer, where fleet-level checks are a formality

## Procedure

1. List every surface the fix touched and every instance that serves it: replicas, workers, schedulers, batch scripts.
2. Query each instance's running version through a health endpoint or its own build stamp, never assuming uniformity from the pipeline.
3. Sweep edge caches and shared caches for content authored by the pre-fix version, and purge or expire them deliberately.
4. Re-run the reproduction once per unique version stamp rather than once per cluster, so partial rollouts are excluded from the claim.
5. Catch stragglers by allowing time for rolling deploys to settle, and re-query after the pipeline's configured window.
6. Record the fleet map: each instance, its version stamp, and the verification date, so rollout completeness is auditable.

## Signals

- One replica keeps the old build, usually because a health check passed too early during rolling deploy.
- A background worker computes the affected decision from its own build but is not part of the tracked rollout.
- Edge caches serve pre-fix content in one region while origin responds correctly after, letting an attacker exploit the edge.
- Canary metrics showed success but full rollout never re-verified, leaving untracked instances to diverge.

## Patterns

**1. Fleet map**

```text
replicas r1..r6: v2.3.2 x5, v2.3.1 x1 -> straggler found
```

**2. Cache purge**

```text
cdn-cache-keys: /api/orders/* invalidated at t+15m; re-fetch old value -> gone
```

**3. Record row**

```text
fleet verified t+40m: 6/6 replicas v2.3.2, workers 3/3, cache purged
```

## Pitfalls

- Trusting the pipeline's green status without querying actual instance versions, which rolls incomplete deploys past the check.
- Purging caches once and assuming regions do not differ for expiry reasons.
- Overlooking background workers, which therefore run pre-fix code and silently perpetuate the defect.

## Done when

- Every replica, worker and cache layer is enumerated with its version stamp, and reproduction verifies the correct one.
- Rollout-completeness evidence is recorded with dates and timestamps so anomalies can be traced back to specific stragglers.

## References

- [OWASP - deployment hardening guidance](https://cheatsheetseries.owasp.org/)
- [NIST SP 800-53 - rollout and change control](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
