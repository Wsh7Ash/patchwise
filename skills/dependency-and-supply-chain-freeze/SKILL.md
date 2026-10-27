---
name: dependency-and-supply-chain-freeze
description: "When a defect arrives through a dependency, pin the affected version and temporarily stop consumption of upgraded builds until the upstream fix is assessed. Use when a vulnerability or defect is confirmed live within a third-party package the product consumes"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [dependencies, supply-chain, containment]
---

# Dependency and Supply Chain Freeze

> When a defect arrives through a dependency, pin the affected version and temporarily stop consumption of upgraded builds until the upstream fix is assessed

**Use when:** a vulnerability or defect is confirmed live within a third-party package the product consumes

**Avoid when:** the dependency is a build-time-only concern with no runtime surface, so exploitable states are limited

## Procedure

1. Identify every place the vulnerable dependency appears: direct imports, transitive paths, build images and container layers.
2. Pin all consumers to the last known-safe version and freeze the upgrade pipeline before it pulls replacements automatically.
3. Assess whether upstream fixes exist, how far behind your installed version is and what changed between.
4. Connect the affected surface in your own product: where the vulnerable code path is actually invoked at runtime.
5. Test pinned builds on the staging environment tied to a known reproduction so the freeze is not itself an operational regression.
6. Track a reinstatement case: upgrade only after the upstream fix is confirmed against the reproduction, not by version number alone.

## Signals

- Pinned builds audited after upgrade show the vulnerable code returning within days, because consumers picked a compatible range rather than the fix.
- Your own package locked builds introduced tree-wide release drift while pinning changed one place only.
- Staging and production consume different lockfiles, which means the freeze covers only what one environment chooses.
- The dependency's own breakdown is exploitable but only with a low surface runtime feature that your configuration does not use.

## Patterns

**1. Freeze row**

```text
libtron-latest locked at 5.3.2 across app+infra
upgrades blocked 2026-10-28 until fix validated
```

**2. Surface check**

```text
runtime path: re-cache xml escaping only, detail feature off
exploit vector mitigates by feature disable
```

**3. Reinstate row**

```text
upgrade to 5.4.0 once [upstream fix confirmed against repro] -> reopen on confirmed
```

## Pitfalls

- Pinning inside only one consumer and leaving the production image's lockfile free to continue pulling the vulnerable version.
- Assume the exploitable path is live, when your consumer of the package does not call the vulnerable function in the deployed configuration.
- Lifting the freeze on version-number reputation rather than a reproduction-verified install.

## Done when

- Every consumer of the affected dependency is pinned at a safe version across app, build and production surfaces.
- Reinstatement criteria are declared and tested against reproduction, before upgrades resume.

## References

- [OWASP Dependency-Check](https://owasp.org/www-project-dependency-check/)
- [SBOM guidance - CISA resource](https://www.cisa.gov/sbom)
