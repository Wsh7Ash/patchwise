---
name: disclosure-timing-and-approval
description: "Choose the disclosure moment deliberately: enough fix penetration that the story matches reality, with approval and notice routes pre-agreed. Use when reporter agreement and internal policy point toward public disclosure, whether through ask-the-reporter channels or a coordinated advisory"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [disclosure, timing, approval]
---

# Disclosure Timing and Approval

> Choose the disclosure moment deliberately: enough fix penetration that the story matches reality, with approval and notice routes pre-agreed

**Use when:** reporter agreement and internal policy point toward public disclosure, whether through ask-the-reporter channels or a coordinated advisory

**Avoid when:** legal or policy constraints mandate omission, in which case the disclosure decision is for the policy owner rather than the fixer

## Procedure

1. Confirm the fix's penetration: publication is honest only when users are actually protected, not when a patch exists on one channel.
2. Decide the disclosure form: coordinated advisory, CVE entry, release note, or credit in a public bulletin the reporter consented to.
3. Set a disclosure window based on who must see it first: reporters, partners, users, and the press if any, in that order.
4. Get the reporter's explicit approval for any listing including their name or handle, and honor their refusal without re-asking.
5. Draft the public text: defect class, affected versions, fixed versions, and workarounds, with no patch material that doubles as a guide.
6. Record the approval trail so every disclosure claim can trace to a decision and a date rather than a consensus memory.

## Signals

- The press hears about it before partners, which reflects ad hoc rather than deliberate disclosure ordering.
- A release note claims a fix shipped on a date when an LTS channel is still without it, leaving users to discover the gap themselves.
- Advisory text doubles as a pattern manual for abuse, which is a disclosure quality issue independent of technical correctness.
- CVE metadata and internal records disagree on the affected range, which mangles future historical analyses reading only one source.

## Patterns

**1. Penetration**

```text
main ok, LTS 2d behind, images rebuilt -> disclosure after LTS ships, not before
```

**2. Ordering**

```text
reporters -> partners -> users -> public, 24h apart
```

**3. Text frame**

```text
advisory: class + affected + fixed + workaround; no exploit procedure
```

## Pitfalls

- Announcing the fix as complete when a channel remains vulnerable, which undercuts every estimate for the same class later.
- Skipping reporter approval for credit, which turns a good reporter into a leaked contributor.
- Including exploitation guidance in an advisory, which makes the disclosure entry active as a training document for the next wave.

## Done when

- Disclosure matches fix penetration reality, with the public text approved and pre-agreed recipients named in order.
- Approval trail is recorded and the advisory text avoids exploitation detail while carrying honest class and version information.

## References

- [ISO 29147 - vulnerability disclosure](https://www.iso.org/standard/72311.html)
- [CVE - entry requirements reference](https://cve.org/)
