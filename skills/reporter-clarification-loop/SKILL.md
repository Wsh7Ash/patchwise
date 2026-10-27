---
name: reporter-clarification-loop
description: "Ask each reporter the one or two questions whose answers unblock triage, instead of silently guessing their intent or step order. Use when reports leave critical detail ambiguous: environment, step order, preconditions or the actual request sent"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [reporter, communication, triage]
---

# Reporter Clarification Loop

> Ask each reporter the one or two questions whose answers unblock triage, instead of silently guessing their intent or step order

**Use when:** reports leave critical detail ambiguous: environment, step order, preconditions or the actual request sent

**Avoid when:** the report already includes everything reproduction needs, in which case a reply would merely add latency

## Procedure

1. Identify precisely which fields, if clarified, would move reproduction or severity from uncertain to actionable.
2. Draft the minimal question set and link each question to the blocked decision it unblocks.
3. Send the questions through the channel that keeps context intact, including the reasoning so the reporter sees why it matters.
4. Set the review window in terms the reporter can act on, so waiting replaces guessing rather than resetting the clock.
5. When the reply arrives, update the normalized record and re-run the affected triage steps from that point.
6. Record the exchange so disputes about what was claimed later resolve from the conversation rather than memory.

## Signals

- Reproduction repeatedly fails because two conditions seem to conflict, until the reporter explained their sequence.
- The reporter's environment detail arrives after your reproduction attempt and changes most assumptions.
- Clarifications shift the affected scope, which typically means triage is restarting rather than resuming.
- Several reports stall over the same repeated question, which indicates an intake template gap rather than reporter behavior.

## Patterns

**1. Question**

```text
for F-0113: what exact version are you running, and were the two tokens used in the same session?
```

**2. Blocked on**

```text
reproduction blocked on precondition=version; severity blocked on scope=one-vs-all
```

**3. Update row**

```text
reply 2026-10-28: tokens in same session -> scope narrower; update record, resume repro
```

## Pitfalls

- Asking every question at once, when only one or two answers truly change the next decision.
- Guessing the reporter's environment rather than asking, which propagates unverifiable assumptions into records.
- Forgetting to update the record after clarification arrives, so the queue's later reading forgets the newer information.

## Done when

- Ambiguous fields are asked through the reporter with the reasoning attached, then answers reflected in the record.
- The question-exchange trail is stored so the trajectory from claim to confirmed evidence is reconstructable.

## References

- [ISO 29147 - disclosure process](https://www.iso.org/standard/72311.html)
- [HackerOne - triage guidance for teams](https://www.hackerone.com/)
