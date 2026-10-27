---
name: fix-review-checklist
description: "Review every fix against the same invariant list, so reviewers validate the relevant claims instead of skimming fashionably. Use when a fix has been generated locally and is about to be reviewed or approved by someone other than the author"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [review, checklist, fix]
---

# Fix Review Checklist

> Review every fix against the same invariant list, so reviewers validate the relevant claims instead of skimming fashionably

**Use when:** a fix has been generated locally and is about to be reviewed or approved by someone other than the author

**Avoid when:** a formal code review flow is not in place, and the author holds both sides of the review alone

## Procedure

1. Confirm the reviewer can see the defect reproduction and the failing test carried by the PR description.
2. Verify the diff touches only the declared fix boundary, and every touched file has a reason recorded for being there.
3. Check for the fix re-delegating trust: any new caller-supplied value included in a security decision is a warning sign.
4. Confirm fixes to stateful surfaces account for stale data written before the fix lands, which often need backfill or a revocation.
5. Confirm the fix's changed authority is validated against both the negative and the positive test states.
6. Record the checklist decisions with as-built evidence tied to each: the diff is what's reviewed, the tests that run are what's proven.

## Signals

- A reviewer approves on a diff with tests that only the author ran, leaving omniscience claims unchallenged.
- Fix branches differ widely in boundary discipline, so a checklist would have flagged the drift earlier.
- Stateful surfaces ship fixes that only affect newly written state, leaving historical records perpetually vulnerable.
- A proposed fix treats the symptom in surface logic when an upstream invariant already lacked validation, so the fix has a sibling.

## Patterns

**1. Check row**

```text
repro attached yes, failing test first commit yes, boundary matches diff yes
```

**2. Warning row**

```text
fix adds client-controlled field to authz decision -> reject: server derives authority
```

**3. Stale data**

```text
fix covers write path only; device records already written -> revoke + backfill needed
```

## Pitfalls

- Reviewing style instead of authority, with style feedback dominating while the security claim goes unexamined.
- Accepting the author's explanation that the stateful legacy data is unimportant, which makes a fix that never closes the record.
- Skipping the checklist when the fix looks small, which is exactly the size whose security claim often goes unexamined.

## Done when

- Review confirms the diff, boundary, and authority indicators against the checklist and each check is checked item by item.
- Any stateful exemptions found by the checklist are tracked as follow-ups, not merely noted in the PR discussion.

## References

- [OWASP - code review guidance](https://owasp.org/)
- [Google engineering practices for code review](https://google.github.io/eng-practices/review/)
