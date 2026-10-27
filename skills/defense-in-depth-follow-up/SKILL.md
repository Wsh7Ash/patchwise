---
name: defense-in-depth-follow-up
description: "Layer one additional independent check behind the primary fix, so the next bug in this surface meets resistance rather than a clean runway. Use when the defect exploits a missing or bypassed check, and the fix adds the missing check"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [defense-in-depth, hardening, fix]
---

# Defense-in-Depth Follow-Up

> Layer one additional independent check behind the primary fix, so the next bug in this surface meets resistance rather than a clean runway

**Use when:** the defect exploits a missing or bypassed check, and the fix adds the missing check

**Avoid when:** the fix is a configuration value already enforced by an external system that defends independently

## Procedure

1. Identify the single point of failure the fix reintroduces: once the new check fails, the defect is back.
2. Design a second, independent check that would have stopped the same exploit through a different mechanism.
3. Verify the second check does not degrade legitimate flow under normal and degraded conditions.
4. Add the second check as a separate change with its own tests, so its failure modes are attributable individually.
5. Document the layering in the fix PR: what the primary check does, what the backup handles and where each might fail.
6. Record the follow-up as a tracked item so the second layer is not a promise that quietly dissolves.

## Signals

- The primary fix closes one path while an adjacent path already exhibited the same weakness but was simply not reported.
- The independent check is pure duplication with the same logic, which fails together with the first rather than independently.
- The second check has no failure story, so nobody knows whether it fails open or closed when the primary layer breaks.
- A limitation in the first layer was known before but nobody had tracked its backup mechanism.

## Patterns

**1. First layer**

```text
ownership check in handler -> prevents direct manipulations
```

**2. Second layer**

```text
database row filter on tenant_id -> independently blocks a handler omission
```

**3. Independent**

```text
first: application logic: second: query builder -> failure modes distinct
```

## Pitfalls

- Stacking two copies of the same check and calling it defense in depth, when both fail identically and simultaneously.
- Assuming the second layer will be honored without verifying its failure mode, which is precisely the story the layer promises.
- Deferring the second layer without a tracker entry, which guarantees the layer never actually lands.

## Done when

- An independent second mechanism exists behind the fix and demonstrably would have stopped the same exploit.
- Failure modes of each layer are documented separately so the layers' backup contract is explicit.

## References

- [OWASP - defense in depth principles](https://owasp.org/)
- [NIST SP 800-53 layered control guidance](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
