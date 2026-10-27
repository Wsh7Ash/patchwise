---
name: skills-and-training-loop
description: "Convert the fix's lessons into the team's working practices, so what one engineer learned does not stay in one engineer's head. Use when multiple engineers touch the affected surface and the fix encodes a practice the same team routinely got wrong"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [training, practices, hardening]
---

# Skills and Training Loop

> Convert the fix's lessons into the team's working practices, so what one engineer learned does not stay in one engineer's head

**Use when:** multiple engineers touch the affected surface and the fix encodes a practice the same team routinely got wrong

**Avoid when:** the affected surface is maintained by a single engineer who holds the complete context

## Procedure

1. Extract the practice the fix implies in operational terms: input you must supply, the check you must run, and how you interpret the signal.
2. Decide where the practice belongs: concrete review checklist, onboarding doc, session walkthrough or automated gate.
3. Distribute using the channel and format the team uses, and prefer working examples over slogans.
4. Validate the practice by observing the next fix or review on the same surface, refining based on what people actually do.
5. Fold the practice into the code review template so it triggers on every relevant change rather than on good intentions.
6. Record the iteration in the defect record, so the practice's provenance and origin stay traceable to the fix that motivated it.

## Signals

- Reviewers apply the practice ad hoc, meaning the checklist is either absent from the tooling or written in a format they skip.
- Two engineers interpret the same instruction differently, so the practice text is ambiguous or its examples miss the real cases.
- Practice sessions only cover one variant of the class and the team guesses elsewhere.
- An engineer independently develops the same practice without knowing it existed, which is exactly what a recorded loop should have delivered.

## Patterns

**1. Practice**

```text
reviewer prompt: does every query touching orders carry owner filter?
```

**2. Distribution**

```text
onboarding doc: add section + link from PR template
```

**3. Validate**

```text
next fix on surface: reviewer applied checklist, practice captured +wo iteration#2
```

## Pitfalls

- Turning practice into training rather than tools, which converts a momentary signal into an enduring performance gap.
- Sharing a practice without a working example, which produces a contradiction that is technically rehearsed but never applied.
- Never iterating the practice as seen in real use, so it drifts into folklore without evidence that it prevents anything.

## Done when

- The practice is recorded as an artifact the whole team can use, and its distribution is justified by how they actually work.
- Subsequent touches of the affected surface show the practice applied, and iteration notes are recorded from what worked.

## References

- [OWASP - secure coding and training guidance](https://owasp.org/)
- [Google SRE - operations hygiene references](https://sre.google/sre-book/postmortem-culture/)
