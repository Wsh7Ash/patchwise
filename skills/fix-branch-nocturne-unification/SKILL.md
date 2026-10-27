---
name: fix-branch-nocturne-unification
description: "Ensure the fix reaches every release channel that carries the vulnerable code, including LTS branches, vendor forks and patched images. Use when the product ships from several branches, images, containers or distributions that may each carry the vulnerable surface"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [channels, branches, releases]
---

# Fix Branch and Release Channel Unification

> Ensure the fix reaches every release channel that carries the vulnerable code, including LTS branches, vendor forks and patched images

**Use when:** the product ships from several branches, images, containers or distributions that may each carry the vulnerable surface

**Avoid when:** the product ships from one branch, and the release pipeline leaves nothing behind for a second channel to inherit

## Procedure

1. List every release channel: LTS, current, mobile platform stores, container images, installers and snapshots kept for customers.
2. Determine which channels carry the vulnerable code today, using the reproduction as the classifier rather than version-range guesses.
3. Cherry-pick or rebase the same fix into each affected channel's branch, preserving test-first ordering in every one.
4. Re-test each channel with the reproduction script plus its own boundary cases, since channel behavior differs in small ways.
5. Coordinate channel release timings so observers cannot assemble a clean timeline comparing an unpatched and patched channel.
6. Record each channel's shipped version and date, so the fix's reach is verifiable rather than assumed to cover everything.

## Signals

- The main branch is fixed while an LTS branch serves most of the deployed fleet and inherits the vulnerable code silently.
- Container images retain the old code in cached layers, so the fix only lands when the base image is rebuilt and users pull it.
- Mobile store releases lag the fix by weeks, and neither the release plan nor the communication names that gap as a real risk.
- Customer snapshots built from old code are still in use and were never covered by any release plan.

## Patterns

**1. Channel map**

```text
branches: main, v2-lts, v1-legacy\ncontainer: app:2.3 (fixed), app:2.3-r1 rebuild needed\nmobile: store release follows
```

**2. Cherry row**

```text
fix cherry-picked to v2-lts: same failing/positive tests rerun -> PASS
```

**3. Timeline**

```text
main ships Tue 2026-11-04, LTS ships Wed, images rebuilt Thu, mobile Fri
```

## Pitfalls

- Fixing main alone and asking whether the LTS branch matters, when LTS is usually the more numerous deployed path.
- Treating image layers as updated merely because the repository shows newer code, when running containers hold the old cache.
- Assuming patch versions align with a fix, and relying on the same version tag to denote all channels and their respective states.

## Done when

- Every channel carrying the vulnerable code is enumerated and has a fix or an explicitly recorded carry-forward date.
- Each channel's shipped version is verified by reproduction, or by an explicit statement that the fix is not reachable there.

## References

- [OWASP - application release guidance](https://owasp.org/)
- [NIST SP 800-53 CM controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
