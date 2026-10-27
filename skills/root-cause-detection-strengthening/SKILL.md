---
name: root-cause-detection-strengthening
description: "Strengthen the detection that found this defect, so the next occurrence is caught earlier, ideally before any human reports it. Use when the defect was caught by a human report, external leak or lucky observation rather than by your own pipeline"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [detection, monitoring, hardening]
---

# Root-Cause Detection Strengthening

> Strengthen the detection that found this defect, so the next occurrence is caught earlier, ideally before any human reports it

**Use when:** the defect was caught by a human report, external leak or lucky observation rather than by your own pipeline

**Avoid when:** the defect surfaced through a monitoring alert that already existed, in which case the alert should be reviewed rather than invented

## Procedure

1. Trace how the defect was actually discovered, and distinguish the detection that worked from the luck that made it visible.
2. Identify the earliest signal the defect produced that your systems already observed: an anomaly in logs, counters, traces or test results.
3. Design the detector that would have fired on that signal: what threshold, pattern or assertion it requires, with its noise floor measured.
4. Add the detector where it can see the signal: tests, CI checks, runtime alerts or synthetic probes, and let the detector prove itself.
5. Run the detector against the historical finding once to confirm it fires, and against a clean period to estimate its false-positive rate.
6. Record the detector, its tuning, and which class of defect it covers, so future hardening builds a coherent set rather than bespoke ones.

## Signals

- The defect produced obvious log noise for months that no rule was watching, so the gap was attention rather than observability.
- Detector noise is too high, which would have been checked if the false-positive rate had been measured before rollout.
- A sibling surface produces the same signal now, and only the deployed detector row catches one of the two paths.
- A detector exists for the signal but its threshold never fires under the relevant traffic pattern.

## Patterns

**1. Discovery trace**

```text
found-by: external report
earliest signal: log pattern 'peer-order-denied' repeating in denied flows for 4 weeks
```

**2. Detector v1**

```text
rule: same actor + refused object + retry within N minutes -> alert; FP rate from clean week = 2/day
```

**3. Coverage row**

```text
detectors added: denied-ownership-retry (runtime), no-owner-filter query (CI static check)
```

## Pitfalls

- Building the detector after the incident and never replaying it against the old finding, so its usefulness stays aspirational.
- Tuning thresholds from intuition rather than a clean measurement window, which guarantees noise debates instead of useful signals.
- Installing detectors nobody watches, which converts a real signal into one more unacknowledged alert.

## Done when

- The discovery path is traced and at least one detector now covers the earliest observable signal of this defect class.
- Detector precision is measured against a clean period, and its coverage is recorded in the same finding record as a follow-up.

## References

- [OWASP - logging cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [NIST SP 800-53 - SI control family](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
