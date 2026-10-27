---
name: playbook-capture-from-fix
description: "Turn this fix arc into a reusable playbook, so the next occurrence of this defect class starts at minute one with a tested sequence. Use when the fix arc is complete and the organizational playbook does not yet cover this defect class"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [playbook, runbook, process]
---

# Playbook Capture From Fix

> Turn this fix arc into a reusable playbook, so the next occurrence of this defect class starts at minute one with a tested sequence

**Use when:** the fix arc is complete and the organizational playbook does not yet cover this defect class

**Avoid when:** the playbook already exists and this fix presents nothing the playbook lacked

## Procedure

1. Reconstruct the timeline from the finding record: intake, triage, containment, fix, verification and disclosure milestones.
2. Identify each decision point and what evidence actually drove it, noting which decisions were fast and which stalled.
3. Extract the sequence into playbook form: condition, action, evidence expected, and clear escalation criteria.
4. Validate the playbook against one historical fix run different from this one, so the play text does not parrot one occurrence.
5. Publish it through the channel responders actually use, from the incident tool to the folder in the margin.
6. Record a playbook version and revision date, so future changes do not clobber previous runbooks silently.

## Signals

- The arc contained a delay that was preceded by an obvious signal, which the playbook can encode and future runs can spot.
- Two fixes of the same class disagree in shape, meaning the playbook must distinguish the variants rather than averaging them.
- Playbook is shared but not validated against a second run, so it encodes only the current arc's tone.
- Responders produce their own account of the playbook, which means the actual playbook source has become the rumor mill.

## Patterns

**1. Arc sketch**

```text
intake→triage(2h)→contain(6h)→fix(2d)→verify(4h)→disclosure
stall at: fix scope because boundary disagreement
```

**2. Play entry**

```text
condition: reported defect + stored permissions\naction: write repro + contain via flag\nescalate after 2 env mismatch
```

**3. Validate**

```text
replay playbook against F-0087 historical run -> marks 2 gaps
```

## Pitfalls

- Copying the arc into a document without testing it against a different historical run, freezing one arc's quirks into policy.
- Publishing in a channel nobody checks when the incident starts, so the playbook exists only for postmortem archaeology.
- Writing the playbook at a level too wide to execute, where each step's context has to be guessed by the responder.

## Done when

- The playbook is derived from this fix arc and validated against at least one distinct historical run.
- The playbook lives in the channel responders actually use, and carries a version, revision date and change history.

## References

- [NIST SP 800-61 - incident response guidance](https://csrc.nist.gov/pubs/sp/800/61/r2/final)
- [ISO 29147 - disclosure process](https://www.iso.org/standard/72311.html)
