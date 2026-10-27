---
name: blast-radius-containment-mapping
description: "Measure exactly what a confirmed defect can reach and touch, and design the smallest containment that stops it from expanding while the full fix is built. Use when a defect is confirmed serious enough to ship within hours, and engineered containment is part of the interim story"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [containment, blast-radius, interim]
---

# Blast-Radius Containment Mapping

> Measure exactly what a confirmed defect can reach and touch, and design the smallest containment that stops it from expanding while the full fix is built

**Use when:** a defect is confirmed serious enough to ship within hours, and engineered containment is part of the interim story

**Avoid when:** no users are identifiable targets and the exposure is hypothetical rather than live

## Procedure

1. Map every actor the defect can act on: what objects, functions, accounts and other data it could modify.
2. Identify the smallest code, configuration or routing change that blocks the defect without breaking legitimate behavior.
3. Prefer opt-in containment that a single feature flag can reverse, so you can trigger off rather than rebuild.
4. Verify the containment change itself through the reproduction script, so the fix is at least regression-free in the narrow case.
5. Check for vertical blast-radius, since a containment change often widens the affected functionality elsewhere unintentionally.
6. Deploy containment with a monitoring window and record the exact time to reversal if the containment itself misbehaves.

## Signals

- Defect reach extends beyond the reported endpoint into sibling paths through shared authorization state.
- Minimal containment closes the reported path, but breaks a legitimate flow the report never mentioned.
- A shared-configuration change stops the leak but delays other service behavior because the coupling was underestimated.
- Containment works in the staged environment but fails in production where the coupling differs.

## Patterns

**1. Blast map**

```text
reachable: all orders for tenant-A including closed  -> impact=C+I minimal
```

**2. Contain option**

```text
clamp: /api/orders/{id} denies non-owner in service layer (reverse by flag disable:hidden-access=off)
```

**3. Verify row**

```text
repro fails after containment -> confirmed no regression on owner calls
```

## Pitfalls

- Designing containment before measuring the blast radius, and blocking paths that maintainers did not need to lose.
- Assuming the containment change is itself safe, when it breaks the legitimate behavior it was not scoped against.
- Skipping monitoring, so an ineffective containment gives false confidence for days.

## Done when

- Blast radius is mapped and a smallest viable containment is deployed and verified against the reproduction script.
- Monitoring and reversal mechanics are in place before the containment ships, in case it misbehaves in production.

## References

- [OWASP - incident management guidance](https://cheatsheetseries.owasp.org/)
- [CWE - common remediation references](https://cwe.mitre.org/)
