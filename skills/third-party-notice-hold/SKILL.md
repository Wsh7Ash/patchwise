---
name: third-party-notice-hold
description: "Decide which external parties hear about a confirmed defect and when, so fixes and notifications land coherently instead of leaking through gossip. Use when the defect affects shared infrastructure, partner integrations or downstream customers who will notice the containment"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [disclosure, notification, containment]
---

# Third-Party Notice Hold

> Decide which external parties hear about a confirmed defect and when, so fixes and notifications land coherently instead of leaking through gossip

**Use when:** the defect affects shared infrastructure, partner integrations or downstream customers who will notice the containment

**Avoid when:** the product is single-tenant and no external party can observe the containment work

## Procedure

1. List every external party who can observe containment: users, integration partners, downstream services and vendors.
2. Decide each party's notice timing relative to the fix, so nobody learns of the defect from a broken integration before anyone tells them.
3. Draft the notice per audience: what happened, what you did, what they should do, expressed without internal jargon.
4. Hold commercial-sensitive detail until the fix is deployed, since premature notice can leak the unpatched surface into the wild.
5. Send notices through the channel each party already monitors, then record delivery against each party.
6. Record the notice timeline along with the fix timeline, so later disclosure reviews see one coherent story.

## Signals

- A partner's monitoring alerts the moment the containment switch flips, before any notice reaches them.
- Two notices describe the same incident with different timelines, confusing downstream responders about what to trust.
- The user-facing notice goes out before the fix is deployed, which attracts probing to the still-open surface.
- A vendor whose component carries the defect learns about it only when a customer routes the report back to you.

## Patterns

**1. Audience map**

```text
partners(n=3): notice after deploy
users: notice after deploy
vendor: notice immediately
```

**2. Notice draft**

```text
partner: rotate shared keys by 18:00 today; integration unaffected post 17:30
```

**3. Timeline**

```text
fix deployed 17:30 -> notices 17:40 -> public post next morning
```

## Pitfalls

- Notifying before the fix exists, which publishes the unpatched surface precisely when it is most valuable to abuse.
- Telling the vendor who ships the broken component but not the partners who consume it, leaving them blind to the real risk.
- Writing notices so vague that every downstream team invents its own response, each more extreme than the last.

## Done when

- Every externally affected party has a planned notice with timing relative to fix deployment, and the drafts exist.
- Delivery is recorded per party, and the notice timeline aligns with the fix timeline in one coherent sequence.

## References

- [ISO 29147 - vulnerability disclosure](https://www.iso.org/standard/72311.html)
- [Google BugHunter disclosure policy](https://bughunters.google.com/)
