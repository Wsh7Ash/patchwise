---
name: post-deploy-observation-sweep
description: "Watch deployed behavior for a bounded window after the fix, distinguishing fix effects from unrelated traffic noise before declaring victory. Use when the fix is deployed and you must verify it performs as expected against live traffic, not just on the test bench"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [monitoring, verification, deployment]
---

# Post-Deploy Observation Sweep

> Watch deployed behavior for a bounded window after the fix, distinguishing fix effects from unrelated traffic noise before declaring victory

**Use when:** the fix is deployed and you must verify it performs as expected against live traffic, not just on the test bench

**Avoid when:** the fix is identical to a previous, already-verified deployment shape racing through the same surfaces

## Procedure

1. Pick the metrics that would show this fix succeeding: refused-count on the affected surface, latency, error rate, and reproduction success.
2. Pick the metrics that would show it failing: a regression refusal rate, spiky latency, an availability gap, or a re-offence.
3. Watch the deployment in stages, reading a canary or set of replica instances before full rollout when the pipeline allows.
4. Verify the reproduction fails in an environment carrying the new build, both before and after the pipeline verifies it fully.
5. Compare against a matched pre-deploy window of the same size, because daily rhythm can be misleading without one.
6. Record the observation period, its metrics, and any anomalies with explanations, keeping matched-window evidence beside each claim.

## Signals

- Refusal rates rise dramatically because the fix changed some behavior the pre-deploy baseline never exercised.
- Latency increases on the affected surface because a check became expensive somewhere else in the returned result.
- Availability drops in a small window, usually around the deployment, then recovers.
- The reproduction still succeeds for a subset of instances that missed the rollout, proving the fix only went partway.

## Patterns

**1. Metrics shift**

```text
refused / hour on affected surface: pre=0 (vulnerable-open), post=181 (guard engages)
```

**2. Matched window**

```text
compare T-24h to T+24h two control groups
refused stable, latency +7ms from check execution
```

**3. Stage view**

```text
canary=2 instances, success -> rollout full -> verified window 24h post
```

## Pitfalls

- Watching for a single day, which always looks 'fine' and never explains whether a client update could push variation.
- Comparing the post window to nothing, thereby never measuring the cost or benefit the deploy actually introduced.
- Skipping canary-based metrics, which leaves a misbehaving fix halfway live in a shard nobody notices.

## Done when

- Metrics establish fix effects using matched pre/post windows, and no anomaly claims explanations without evidence.
- Observations continue until the original owner defines the window as validated and marks the fix confirmed in production.

## References

- [OWASP - logging and monitoring integration](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [NIST SP 800-53 - performance monitoring](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
