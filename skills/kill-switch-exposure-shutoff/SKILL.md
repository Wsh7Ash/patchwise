---
name: kill-switch-exposure-shutoff
description: "Pull the nearest feature flag, gate or toggle the moment it isolates the affected surface, buying hours without a code change or release train. Use when the affected surface sits behind an existing feature flag, experimental gate or deployment-level toggle"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [kill-switch, feature-flag, containment]
---

# Kill-Switch Exposure Shutoff

> Pull the nearest feature flag, gate or toggle the moment it isolates the affected surface, buying hours without a code change or release train

**Use when:** the affected surface sits behind an existing feature flag, experimental gate or deployment-level toggle

**Avoid when:** no such switch exists, in which case shutoff means code or dependency containment instead

## Procedure

1. Locate every flag, gate or toggle that includes the affected surface in its off-state blast radius.
2. Check what else each switch disables, since a flag too coarse will stop unrelated traffic the containment should spare.
3. Flip the narrowest switch that fully isolates the affected surface, ideally in a config store that applies without redeploy.
4. Re-run the reproduction script within minutes and confirm the reported behavior no longer returns its payload.
5. Watch downstream consumers of the switched surface, and record which legitimate flows died with it.
6. Leave the switch in place until the real fix ships, and record the exact reversal instructions next to the flag definition.

## Signals

- A single flag isolates the surface but its off-state also disables a healthy co-tenant feature the business relies on.
- The flag applies at the edge but the vulnerable handler also runs in a background job the flag never reaches.
- Config propagation is slower than the exposure, so some instances keep serving the affected path for minutes after the flip.
- Reversal instructions exist only in one engineer's head, which makes return-from-containment riskier than the containment itself.

## Patterns

**1. Switch picker**

```text
flag=premium-export off-state also kills billing-alerts -> too coarse
flag=export-v2 isolates precisely -> flip
```

**2. Propagation**

```text
config TTL=60s -> instances converged at t+90s -> re-run repro: 403 -> contained
```

**3. Record row**

```text
flag=export-v2 flipped=15:04 verified=15:06 collateral=billing-alerts (waived, temporary)
```

## Pitfalls

- Grabbing the coarsest available switch because it is fastest, which stops far more traffic than the exposure justifies.
- Assuming a flag reaches every runtime path, when background jobs and other entry points read config through different layers.
- Skipping the verification rerun, leaving the flag flipped but the vulnerable handler still reachable in another surface.

## Done when

- The narrowest sufficient switch is flipped, and the reproduction confirms the affected behavior is unreachable within minutes.
- Collateral effects are recorded plus the reversal instructions, so the outage state is describable hours later.

## References

- [OWASP - configuration management guidance](https://cheatsheetseries.owasp.org/)
- [NIST SP 800-61 - incident handling](https://csrc.nist.gov/pubs/sp/800/61/r2/final)
