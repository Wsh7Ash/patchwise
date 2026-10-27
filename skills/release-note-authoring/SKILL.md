---
name: release-note-authoring
description: "Write the release note that states reality: what was affected, what changed and what users must do, rather than a version-number-less 'security improvement'. Use when the fix has shipped and the release notes are being prepared for the build that carries it"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [release-note, documentation, disclosure]
---

# Release Note Authoring

> Write the release note that states reality: what was affected, what changed and what users must do, rather than a version-number-less 'security improvement'

**Use when:** the fix has shipped and the release notes are being prepared for the build that carries it

**Avoid when:** the fix is internal-only and no release note is owed; document it in the audit trail instead

## Procedure

1. State the affected surface in terms users can recognize: which endpoint, command or feature they interacted with.
2. State the fixed versions per channel, so every audience can verify their own build against their own channel's promise.
3. Write the workaround exactly, if one exists, and state whether it is still needed after the fix or just belt-and-braces.
4. Mention any forced action users must take: re-authentication, key rotation, package upgrade, config flip.
5. Verify the note against shipping reality: does every claim match the fleet fix status, or does a channel lag until tomorrow?
6. Keep the note retrievable linking the affected defect identifier, since the release note is the layer every future analysis reads first.

## Signals

- A release note says 'security improvement', which means users cannot tell whether their builds are protected from a specific class.
- Affected-version lists omit an LTS branch that still carries the vulnerability, so the note overpromises protection.
- Instructions imply a package upgrade but do not mention the accompanying key rotation, which means users miss a real step.
- Notes achieved through automation drift from reality, since a stale pipeline generated the chart and nobody reviewed against the ship state.

## Patterns

**1. Note row**

```text
affected: /api/orders cross-tenant read in SOAP v2.3.0-2.3.1 (LTS v2.2.x not affected)
```

**2. Action row**

```text
users rotate API keys if issued between 2026-10-20..10-28
```

**3. Linkage row**

```text
defect: F-2026-0113, covered by release tracking 2026-11-05
```

## Pitfalls

- Writing to the changelog's cheer taxonomy rather than a user's recognition, which prevents self-service discovery.
- Omitting forced actions, especially key rotation, because the fix appears to be the authoritative prompt after the deployment.
- State-building channel specificity as a placeholder, since release notes are only meaningful when a channel identity is exact.

## Done when

- The release note identifies the affected surface, per-channel fixed versions, workarounds, and forced user actions.
- Every claim in the note matches reality on the shipping fleet, including honest lag in channels that have not caught up.

## References

- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
- [OWASP - user communication guidance](https://cheatsheetseries.owasp.org/)
