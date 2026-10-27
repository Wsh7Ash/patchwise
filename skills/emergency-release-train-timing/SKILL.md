---
name: emergency-release-train-timing
description: "Schedule the emergency fix so it lands inside the normal release window where possible, and confirm the fuse is never longer than the severity merits. Use when the fix is ready or imminent and the organizational release cadence normally runs on slower cycles"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [release, emergency, process]
---

# Emergency Release Train Timing

> Schedule the emergency fix so it lands inside the normal release window where possible, and confirm the fuse is never longer than the severity merits

**Use when:** the fix is ready or imminent and the organizational release cadence normally runs on slower cycles

**Avoid when:** release trains are already frequent enough that the fix ships within the period severity agrees with

## Procedure

1. Compute the target ship window from severity: how long this defect can exist publicly without forcing your organization's own risk threshold upward.
2. Identify the next regular release slot and whether it arrives inside the target window.
3. Route the fix inside that regular slot when possible, which is always operationally cheaper than a dedicated release.
4. When cadence misses the window, trigger the out-of-band path with the criteria pre-recorded: who signs, what test coverage is sufficient.
5. Coordinate communications so reporter notifications do not precede the deployment that fixes the reported behavior.
6. Record both the scheduled and shipped timestamps so future cadence reviews see real numbers rather than plans.

## Signals

- The next regular slot arrives within the risk window, yet a rushed out-of-band release was still prepared out of habit.
- Out-of-band release jumps through the pipeline shorter than usual, producing a bugfix that regresses something elsewhere.
- Reporter notification precedes the deployed fix, which grabs public attention before the vulnerability is actually patched.
- The release includes unrelated changes, so regression risk is broader than the remediation it carries.

## Patterns

**1. Risk window**

```text
high severity target 7 days max
next regular slot=2026-10-30 inside window -> ship there
```

**2. OOB criteria**

```text
OOB only when severity=high and no slot inside window or exploit-in-flight
```

**3. Ship row**

```text
F-0113 shipped in 2026-10-30 regular release; declaration=none extra
```

## Pitfalls

- Jumping to out-of-band release for cadence reasons rather than risk reasons, which pays a coordination cost silently every time.
- Not bundling other fixes into the emergency release, which makes their regression surface part of the emergency blast radius.
- Forgetting to front-load reporter notification, which claims the work is done while users remain exposed.

## Done when

- Every emergency fix has a severity-derived target window and a matching release plan shape.
- Reporters and operators are told the same thing about the fix due to a coordinated communication timeline.

## References

- [ISO 29147 - vulnerability disclosure](https://www.iso.org/standard/72311.html)
- [Google Project Zero - disclosure policy reference](https://bughunters.google.com/)
