---
name: upstream-dependency-fix-priority
description: "Treat dependency-supplied defects by their true upstream status: patch, upgrade or local patch, each with a different valid timeline. Use when the defect is inside a third-party package or library rather than in the first-party codebase"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [dependencies, upgrade, pinning]
---

# Upstream Dependency Fix Priority

> Treat dependency-supplied defects by their true upstream status: patch, upgrade or local patch, each with a different valid timeline

**Use when:** the defect is inside a third-party package or library rather than in the first-party codebase

**Avoid when:** the third-party component ships sealed and unsupported, where the only honest route is replacing it entirely

## Procedure

1. Determine whether upstream has a released fix, an in-flight patch or nothing at all, since these have very different timelines.
2. Pin the vulnerable version only if needed, and freeze deploys from overriding your pinned range.
3. For an available fix, upgrade across the affected range and re-run your reproduction, confirming the exploit no longer lands.
4. For an in-flight patch, apply the local patch only if production risk exceeds the risk of a patch you must maintain yourself.
5. For no fix at all, consider isolating the dependency behind a boundary where the vulnerable path is unreachable from product input.
6. Record the upstream chronology: version, fix status, your dependency range and the date your own patch will retire.

## Signals

- Upgrading the dependency changes other behavior subtly, so the fix requires its own regression sweep rather than a version bump.
- The local patch sequence outruns upstream enough to become the de facto distribution, which means the timeline is now yours.
- Isolation boundaries require several changes at once, and the incomplete boundary is itself a risk if the isolation is partial.
- Upstream's fix version range overlaps other pinned ranges, waiting weeks until the ecosystem resolves them.

## Patterns

**1. Upstream check**

```text
libtron 5.3.2 vulnerable, 5.4.0 patched -> upgrade range app+infra together
```

**2. In-flight patch**

```text
no release yet -> local patch registered + dated: retires when 5.4.0 lands
```

**3. Isolation**

```text
unpatched + confirmed exploitable -> wrap dependency entry points; restrict caller-facing path
```

## Pitfalls

- Upgrading by version number alone without rerunning the reproduction, since aliasing defects frequently survive version bumps.
- Emitting local patches without a retire plan, which quietly overtakes the maintenance year ahead of you.
- Failing to freeze automatic deploys, which reintroduces the vulnerable version on the next nonmanual deploy.

## Done when

- The upstream status is documented and the chosen route (upgrade, patch, isolate) is recorded as a follow-up with its timeline.
- The final dependency state has been verified against the reproduction, not merely by reading a changelog.

## References

- [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)
- [SBOM CISA guidance](https://www.cisa.gov/sbom)
