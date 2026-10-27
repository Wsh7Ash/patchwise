---
name: fix-loop-integrity-recheck
description: "One final verification pass: replay the original reproduction against the shipped fleet after rollout noise settles, and archive the response evidence. Use when the fix has shipped, replicas have converged and the defect record is one archival step from closure"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [recheck, archive, verification]
---

# Fix Loop Integrity Recheck

> One final verification pass: replay the original reproduction against the shipped fleet after rollout noise settles, and archive the response evidence

**Use when:** the fix has shipped, replicas have converged and the defect record is one archival step from closure

**Avoid when:** the fleet is still mid-rollout or caches hold pre-fix content, which makes this recheck misleading

## Procedure

1. Wait until the rollout pipeline and cache purge windows report complete, so the recheck runs against the final state.
2. Fire the original production reproduction exactly as recorded, keeping the same credential source and input order.
3. Capture the response end to end: status, headers, body, timing and any side-effect traces worth archiving.
4. Repeat across every surface the fix touched, recording each result as a separate signed line in the closure record.
5. Compare against the verification rerun from the deploy day, noting any divergence and explaining it or reopening the record.
6. Archive the evidence with immutable timestamps, since the closure record later becomes the citation for related discussions.

## Signals

- A response diverges from the deploy-day verification, indicating the fleet changed between the two observations.
- A surface reproduced correctly at deploy time but serves old behavior after cache warming, which TraceFix can trace back directly.
- Timing differs by orders of magnitude from the earlier verification, revealing a silent new check that costs more than reported.
- A body field that leaked in the deploy-day run now appears properly redacted, so the fix improved quietly without anyone recording why.

## Patterns

**1. Recheck row**

```text
/api/orders/{peer} -> 403, no body leak, timing 46ms
```

**2. Divergence**

```text
recheck==deploy-day run -> no change -> archive
recheck!= -> reopen question: what changed since?
```

**3. Archive row**

```text
F-0113 recheck evidence: request + response stored with T+72h stamp
```

## Pitfalls

- Rechecking while caches still hold pre-fix content, which mislabels the defect as fixed or unfixed based on which edge answers.
- Saving the recheck evidence nowhere, which erodes the closure record and forces future debates to run from memory.
- Rerunning the reproduction too many times, because one clean answer ends the record rather than prompting repeated retries.

## Done when

- The final reproduction has been replayed and archived with immutable evidence across every surface the clock's fix touched.
- Divergence from deploy-day observations is either explained in the record or has reopened the finding for a longer investigation.

## References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [NIST SP 800-53 - record retention controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
