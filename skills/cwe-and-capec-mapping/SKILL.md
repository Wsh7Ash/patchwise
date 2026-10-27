---
name: cwe-and-capec-mapping
description: "Map each confirmed defect to the precise weakness category and attack pattern, so reports, metrics and fixes share one vocabulary. Use when defects need to be tracked across releases, or severities must be normalized against a shared defect taxonomy"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [taxonomy, cwe, metrics]
---

# CWE and CAPEC Mapping

> Map each confirmed defect to the precise weakness category and attack pattern, so reports, metrics and fixes share one vocabulary

**Use when:** defects need to be tracked across releases, or severities must be normalized against a shared defect taxonomy

**Avoid when:** an internal taxonomy is already established and this mapping would add novelty rather than resolution

## Procedure

1. Read the confirmed behavior as an engineering statement, not as a marketing one, and identify which weakness it violates.
2. Search the CWE taxonomy for candidate categories, ranking them by alignment with the observed mechanism rather than title similarity.
3. Check parent categories for granularity: prefer the most specific child that explains the mechanism.
4. Record the CWE identifier with a one-line justification sentence, so future curators can challenge precisely.
5. Map the attack pattern where a CAPEC entry fits the observed chaining, keeping it optional rather than mandatory.
6. Aggregate the taxonomy across the backlog to expose defect clusters the fixes and hardening should target.

## Signals

- Two reports map to the same CWE despite differing in surface behavior, suggesting a shared root cause the fix should target.
- A parent category is chosen because no child describes the mechanism precisely, so the record should note that explicitly.
- Metrics cluster on one CWE across quarters, indicating a systemic pattern the individual fixes never addressed.
- The same defect is recorded under different CWEs over time, which points to mapping drift between triagers.

## Patterns

**1. Mapping row**

```text
behavior: object id validated against wrong ownership table -> CWE-639+ (authorization bypass scope)
```

**2. Justification**

```text
CWE-639 selected over CWE-285 because the failure is object-owner-scoped, not general-access
```

**3. Aggregate**

```text
top CWE this quarter: 639 (4), 79 (2) -> authorization clustering emerging
```

## Pitfalls

- Picking the CWE whose title sounds closest, while the mechanism actually sits under a differently-named node.
- Mapping to the parent when a child fits, wasting the specificity a future fix could target.
- Mandating CAPEC entries when no attack pattern applies, which produces decorative data rather than metrics.

## Done when

- Each defect has a specific CWE with a recorded justification sentence, at the most precise category that fits.
- The packed taxonomy view exposes clusters the fix queue and hardening should target.

## References

- [CWE list](https://cwe.mitre.org/)
- [CAPEC attack pattern index](https://capec.mitre.org/)
