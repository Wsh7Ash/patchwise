---
name: wave-pattern-canary-recheck
description: "Deploy a synthetic canary that continuously exercises the fixed surface for a bounded window, since the defect's clock does not stop at first verification. Use when a defect is high-impact and the surface is reachable in a changing environment where the fixed behavior could reappear"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [canary, synthetic, monitoring]
---

# Wave-Pattern Canary Recheck

> Deploy a synthetic canary that continuously exercises the fixed surface for a bounded window, since the defect's clock does not stop at first verification

**Use when:** a defect is high-impact and the surface is reachable in a changing environment where the fixed behavior could reappear

**Avoid when:** the surface is static and nobody re-deploys it, in which case periodic canaries are a checklist ritual rather than a signal source

## Procedure

1. Build the canary below the surface level: two controlled identities and a scripted request path identical to the original reproduction.
2. Run the canary on a schedule that reflects traffic and release rates, rather than at calendar-friendly intervals.
3. Have the canary emit its result as a metric and an alert threshold, not merely as a log line a human must read daily.
4. Track canary state so a failure distinguishes between its own infrastructure and a regression in the surface under test.
5. Calibrate the canary against a clean period and against the pre-fix behavior, capturing both distributions.
6. Retire or cadence the canary when the defect class is closed and the surface stabilizes, recording how long each canary ran.

## Signals

- Canary results flip between two states because environment variance rather than the fix's surface changed.
- Alert thresholds inherited from another surface, which means the canary fires far later than it should during real regression.
- The canary is successfully observed but nobody is on the alert route, which makes it fully decorative.
- The canary surfaces the stabilized fix behavior, but only because the canary itself was rebuilt, not because the surface changed.

## Patterns

**1. Canary script**

```text
synthetic: alice + bob, scripted peer-order request every 15m
output: metric ok=1/fail=0 + alert if fail
```

**2. Calibration**

```text
clean 48h: fail=0\npre-fix replay: fail rate=1 -> distributions separated
```

**3. Coverage map**

```text
runs in each region + worker channel separately
```

## Pitfalls

- Operating one identity only, which tests less of the pre/post state and often misses the whole fake-object detection path.
- Setting the canary interval to match calendar convenience rather than release traffic, making detection slower than the fix might break.
- Never retiring a canary, so the fix and monitoring pile up indefinitely as if every fixed surface is still in lockdown.

## Done when

- A canary runs at a rate derived from traffic and release shape, and emits a metric-based alert rather than a human-only log.
- Canary behavior is calibrated against both clean-period and pre-fix distributions, and the alert route reaches a real responder.

## References

- [OWASP - synthetic monitoring guidance](https://cheatsheetseries.owasp.org/)
- [NIST SP 800-53 - continuous monitoring controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
