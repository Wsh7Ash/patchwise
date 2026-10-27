---
name: adjacent-code-neighborhood-sweep
description: "After a fix, sweep the same pattern through neighboring code, because the copy-paste origin of this defect almost always generated siblings. Use when the fixed defect is a classic pattern class, meaning other implementations of the same shape exist and probably predate this one"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [sweep, pattern, hardening]
---

# Adjacent Code Neighborhood Sweep

> After a fix, sweep the same pattern through neighboring code, because the copy-paste origin of this defect almost always generated siblings

**Use when:** the fixed defect is a classic pattern class, meaning other implementations of the same shape exist and probably predate this one

**Avoid when:** the defect is demonstrably unique to one function that nothing else copies or mirrors

## Procedure

1. Abstract the fixed defect into a searchable pattern: the check shape, data flow, or common call sequence it uses.
2. Search the codebase for that pattern, including tests, scripts, configuration templates and vendored code.
3. For each candidate surface found, apply the reproduction logic and observe where the defect shape recurs.
4. Treat confirmed siblings as defects in their own right, recording them in the same taxonomy rather than absorbing them into this finding.
5. Decide per sibling whether it ships in the same fix or a scheduled follow-up, but never leave it unrecorded.
6. Add a lint or test check where possible, so the pattern is mechanically excluded going forward.

## Signals

- The same error shape exists in a file one directory over, produced by copy-paste from the vulnerable code path.
- A variant surface computes an equivalent decision with slightly different conditions, meriting the same defect inquiry.
- The pattern appears in generated or vendored code, and an upstream maintainer had probably fixed it first.
- Conservative sweep misses the pattern, so a refined search string finds candidates the first pass overlooked.

## Patterns

**1. Pattern expr**

```text
pattern: if(!query.param).doSomething(); -> grep token: 'if(!.*\.param' -> candidates 4
```

**2. Sibling row**

```text
candidate: reports.py:212 same shape -> confirmed -> F-0118 (separate defect)
```

**3. Mechanical**

```text
lint rule: forbid raw user-supplied id in ownerless query context -> blocks future copies
```

## Pitfalls

- Searching only the exact code shape without abstracting the pattern, which finds identical lines but misses logical siblings.
- Recording siblings as one defect, which hides the count the fix pattern actually earned.
- Skipping the mechanical check, so re-implementing the pattern remains untouched for tomorrow's reviewer.

## Done when

- The pattern is abstracted, searched across the codebase, and every candidate has an explicit verdict recorded.
- Mechanical exclusion, where feasible, is added to tests or lint so recurrence is checked automatically rather than promoted by memory.

## References

- [CWE - pattern catalog for searching](https://cwe.mitre.org/)
- [OWASP - secure coding practices checklist](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
