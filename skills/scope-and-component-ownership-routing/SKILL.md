---
name: scope-and-component-ownership-routing
description: "Route each confirmed defect to the team that owns the affected component, so fixes do not wait on discovering whose responsibility it was. Use when the product spans several components, repositories, teams or vendors, and ownership boundaries are not written on reports"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [ownership, routing, triage]
---

# Scope and Component Ownership Routing

> Route each confirmed defect to the team that owns the affected component, so fixes do not wait on discovering whose responsibility it was

**Use when:** the product spans several components, repositories, teams or vendors, and ownership boundaries are not written on reports

**Avoid when:** the product is a single component one team owns end to end

## Procedure

1. Identify the affected artifacts precisely: file, endpoint, package, deployment configuration or build step.
2. Match each artifact to its owning team through the ownership directory, confirming the owner still matches reality.
3. Record whether the defect is localized to one component or spans several, since spans decide who coordinates.
4. Check for vendor or third-party attribution, since those routes have external timelines rather than internal ones.
5. Route the defect with the normalized reproduction and severity attached, so the receiving team starts with facts rather than a summary.
6. Record the handoff, the receiving team and the date, so escalation has a real reference if the queue sits.

## Signals

- Two components were touched and each blames the other, so ownership disagreement surfaced rather than routing failure.
- A first-party wrapper over a third-party library carries the defect, which makes the fix reach someone else on someone else's schedule.
- The organization chart names a team that no longer owns the component, pointing to a stale ownership record.
- A defect spans deployment configuration plus application code, and no one recorded who answers for the combined behavior.

## Patterns

**1. Artifact row**

```text
artifact=src/auth/token.go owner=identity-team repo=backend
artifact=ingress config owner=platform-team
```

**2. External row**

```text
defect in libtron-latest v5.4 upstream issue filed 2026-10-27 internal tracker F-0113
```

**3. Handoff row**

```text
F-0113 -> identity-team 2026-10-27 with repro script + severity=medium
```

## Pitfalls

- Routing by report headline instead of artifact ownership, which sends container defects to UI teams iteratively.
- Ignoring external dependency transport issues, which produce fix timelines nobody inside controls.
- Routing without the evidence attached, forcing the receiving team to restart reproduction from a one-line summary.

## Done when

- Every confirmed defect has an artifact-verified owner recorded, with third-party routes marked as such.
- Handoffs carry the reproduction evidence and severity so the receiving team can act without re-deriving facts.

## References

- [Google - vulnerability disclosure handling reference](https://bughunters.google.com/)
- [OWASP - dependency checking](https://owasp.org/www-project-dependency-check/)
