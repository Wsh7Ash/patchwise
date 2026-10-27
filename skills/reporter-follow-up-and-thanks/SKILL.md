---
name: reporter-follow-up-and-thanks
description: "Close the loop with the reporter: what was verified, what was fixed and which follow-ups remain, until the report is visibly resolved. Use when the fix is verified and the reporter's submission deserves closure it can actually read"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [reporter, communication, disclosure]
---

# Reporter Follow-Up and Thanks

> Close the loop with the reporter: what was verified, what was fixed and which follow-ups remain, until the report is visibly resolved

**Use when:** the fix is verified and the reporter's submission deserves closure it can actually read

**Avoid when:** the reporter has explicitly asked for no further contact, which should be honored while still filing the request

## Procedure

1. Write the summary the reporter can verify against their original report: claim, verdict, reproduction and fix.
2. State remaining follow-ups honestly, including what is scheduled and what was judged not to require a fix.
3. Use the reporter's original identifiers and terminology, so their submission remains recognizable in the closure message.
4. Include acknowledgement the reporter requested: credit, or inclusion in a public listing only after their final approval.
5. Send the closure through the channel they originally used, so no effort is shifted onto them finding it elsewhere.
6. Track the final exchange in the finding's record, including the returns and whether the closure resolved the reporter's question.

## Signals

- The reporter asks what happened weeks after the fix, indicating the closure message never reached the right channel.
- A closure message is written for internal readers rather than the reporter's context, leaving them unable to verify.
- The reporter's own terminology is not reflected in the closure, so they cannot match your words to their claim.
- Honesty is unreliable, making them doubt subsequent reports from the same reporter even when the facts are strong.

## Patterns

**1. Summary block**

```text
your claim: cross-tenant order read -> confirmed via repro\nfix: owner filter, shipped v2.3.2\nremaining: none
```

**2. Wording rule**

```text
use the reporter's term 'closed orders' -> not internal term 'tier-3 archivals'
```

**3. Tracking entry**

```text
sent 2026-11-03 via vendor-form reply; reporter queries resolve within 1 additional exchange
```

## Pitfalls

- Writing the closure summary in terminology only the internal team recognizes, which leaves the reporter unable to verify.
- Promising follow-ups without dates, which is exactly the complaint that generated the report throughput spike next quarter.
- Withholding interpreter-free explanation of what was fixed, by treating legal jargon as the closure target the reporter asked for.

## Done when

- A closure message is delivered through the reporter's original channel and uses identifiers they can trace to their claim.
- Remaining follow-ups are stated with their dates, and the reporter's resulting replies are tracked in the finding record.

## References

- [ISO 29147 - vulnerability disclosure](https://www.iso.org/standard/72311.html)
- [HackerOne - response quality guidance](https://www.hackerone.com/)
