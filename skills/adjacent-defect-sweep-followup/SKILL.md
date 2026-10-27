---
name: adjacent-defect-sweep-followup
description: "Sweep the class's adjacent surfaces for the same defect pattern after the fix, so the sweep's findings and non-findings both live in the record. Use when the defect class is mechanistically transferable and neighboring surfaces carry the same vulnerable pattern"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [sweep, followup, hardening]
---

# Adjacent Defect Sweep Followup

> Sweep the class's adjacent surfaces for the same defect pattern after the fix, so the sweep's findings and non-findings both live in the record

**Use when:** the defect class is mechanistically transferable and neighboring surfaces carry the same vulnerable pattern

**Avoid when:** the defect required a surface-specific misconfiguration that no shared component recreates in adjacent surfaces

## Procedure

1. Define what counts as adjacent for this defect: same code path, same component reuse, same data model, or same trust boundary.
2. Enumerate adjacent surfaces using the class's mechanism rather than guesses about code structure.
3. Test each adjacent surface with the class's mechanism reproduced in the original fix arc, capturing evidence per surface.
4. For surfaces found clean, record the specific property that made them clean, since clean-on-inspection without recording the property collapses under re-investigation.
5. For surfaces found affected, file their defects as separate records with their own sweep notes rather than merging with the original fix.
6. When the sweep concludes, record the sweep class and its coverage boundary, since a sweep is finished only when its boundary is explicit.

## Signals

- Sweeps continue to return clean verdicts, and the record can no longer state which surfaces the sweep actually covered versus skipped.
- Surface's clean verdicts trace to a stale sweep line the current fix class should not be bound to.
- One adjacent surface reproduced and returned the same defect variation the fix arc closed, which is a high-value finding.
- The class's mechanism works differently on a surface because data shape changed, so the sweep vocabulary needs a new row.

## Patterns

**1. Boundary**

```text
adjacent = surfaces reusing OrderStorage component: api, jobs, dashboard
```

**2. Per-surface**

```text
dashboard: clean (grid read never crosses tenant)
jobs: affected -> filed F-0089
```

**3. Boundary note**

```text
sweep covers OrderStorage reuse; not file uploads (different component)
```

## Pitfalls

- Sweeping only what code search shows, missing records where the defect class manifests through data rather than code reuse.
- Recording clean verdicts without the property, so a future reviewer cannot distinguish swept-clean from never-considered.
- Merging adjacent-surface findings into the original defect record, which buries each surface's separate history.

## Done when

- Adjacent surfaces are enumerated by mechanism and tested with the class's original reproduction, with evidence per surface.
- Clean verdicts record the specific property that makes them clean, and affected verdicts become separate defect records.

## References

- [OWASP - testing guidance for adjacent code](https://owasp.org/www-project-web-security-testing-guide/)
- [NIST SP 800-53 - bug tracking and defect management](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
