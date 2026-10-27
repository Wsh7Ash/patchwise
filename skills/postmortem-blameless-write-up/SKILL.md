---
name: postmortem-blameless-write-up
description: "Write the postmortem without naming culprits, so the record focuses on what the system permitted rather than on whom it embarrassed. Use when the incident is closed and a shared learning document is expected to prevent the next occurrence"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [postmortem, blameless, process]
---

# Blameless Postmortem Write-Up

> Write the postmortem without naming culprits, so the record focuses on what the system permitted rather than on whom it embarrassed

**Use when:** the incident is closed and a shared learning document is expected to prevent the next occurrence

**Avoid when:** the organization already tracks learning elsewhere, and a second postmortem would duplicate existing records

## Procedure

1. Reconstruct the timeline factually, with timestamps, and specify what each step of evidence confirmed at the time.
2. Narrate the system's state at each point: what checks existed, what monitoring was watching, what permissions were exercised.
3. Analyze contributing factors without naming individuals, and prefer phrasing like 'the interface permitted' over 'engineer X missed'.
4. Track each action item: what it addresses, who owns it, when it lands and when it is verified to have landed.
5. Pre-register what would make the same incident recur, turning vague bet-hedging into a concrete follow-up.
6. Share the postmortem with the people who will use it: responders, reviewers and maintainers of the affected surface.

## Signals

- Action items from the postmortem drift toward non-technical or hypothetical once they begin landing later.
- Timeline facts do not match the fix record, indicating two contradictory truths one needs to be authoritative.
- The postmortem skips pre-registering recurrence conditions, so vague claims ride freely without facing rules.
- Same-class defect reappears with no correspondence to any of the postmortem's pre-registered recurrence signals.

## Patterns

**1. Timeline**

```text
T-0 report accepted       T+2h triage
T+6h contained           T+2d fix shipped
T+3d verified            T+4d disclosed
```

**2. Contributing**

```text
system permitted query w/o owner filter
monitoring had no detection rule
playbook missing
```

**3. Action item**

```text
AC-01: query lint rule, owner: platform, due 2026-11-15, verified on merge
```

## Pitfalls

- Writing postmortem culprits into a document that is otherwise blameless, which restores the shame geometry in one paragraph.
- Recording action items without owners, which converts the postmortem into an aspiration rather than a plan.
- Failing to distinguish between the timeline and the judgments, so future readers cannot tell evidence from narrative.

## Done when

- The postmortem carries a factual timeline, narrative system state, contributing factors and no individual blame.
- Every action item has an owner, a due date and a verification path, and recurrence signals are pre-registered rather than vague.

## References

- [Google SRE - postmortem culture](https://sre.google/sre-book/postmortem-culture/)
- [NIST SP 800-61 - incident response](https://csrc.nist.gov/pubs/sp/800/61/r2/final)
