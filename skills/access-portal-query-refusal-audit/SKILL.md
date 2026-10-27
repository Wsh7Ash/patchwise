---
name: access-portal-query-refusal-audit
description: "Re-audit the affected surface's query refusals after the fix, confirming every entry path's verdict aligns, not only the path the report described. Use when the fixed surface has multiple entry paths and each is expected to refuse the same unauthorized access"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [audit, entry-paths, verification]
---

# Access Portal Query Refusal Audit

> Re-audit the affected surface's query refusals after the fix, confirming every entry path's verdict aligns, not only the path the report described

**Use when:** the fixed surface has multiple entry paths and each is expected to refuse the same unauthorized access

**Avoid when:** the surface has exactly one entry path, which the fix addressed and the audit would restate

## Procedure

1. Enumerate the entry paths into the fixed surface: API handlers, jobs, scheduled tasks, upstream led reads, direct DB checks.
2. Prepare the test credential pair used during the original finding, matching exactly the permissions each path needs.
3. For each entry path, fire the identical peer-request and confirm the refusal response, recording each independently.
4. Check consistency of the status and body shape across paths, since a mismatch leaks that the check differs per surface.
5. Track any path that refuses with a different shape, and mark the inconsistency as a separate finding rather than one blob.
6. Record the audit matrix in the finding record so future reviewers can see that entry-path parity, not just the reported one, held.

## Signals

- The reported API path refuses, while the background job still inserts updates under the same unauthorized identity.
- Both paths refuse, but with different message shapes, which is itself a leak that makes one handler's check identities obvious.
- One entry path is a direct internal worker that bypasses the fixed surface entirely, and its credentials never inherit the fix.
- The refusal message is uniform, but the refusal status differs and one test-endpoint's base guard silently uses another actor's role.

## Patterns

**1. Path list**

```text
api-handler(v1), background-import, scheduled-summarize, daily-export, direct-table-read via dashboard
```

**2. Entry check**

```text
api-handler: 403 ok\ndaily-export run as peer-identity: second-row also 403 ok\nscheduled-summarize: -> 200 leak
```

**3. Matrix row**

```text
consistency: status=403 uniform, body shapes=uniform:\nno leak of defect identity across entry paths
```

## Pitfalls

- Auditing only the reported path, which means the audit checks the claim rather than the breadth of where the claim existed.
- Reusing different credentials across entry paths, making results non-comparable and masking a portion of the surface.
- Recording the audit matrix without an explicit consistency verdict, so a future reader cannot confirm that parity held.

## Done when

- Every entry path into the fixed surface has been tested with identical credentials and its refusal is recorded.
- Any refusal shape or status inconsistency is recorded separately with the path that generated it.

## References

- [OWASP - access control guidance](https://cheatsheetseries.owasp.org/)
- [NIST SP 800-53 - audit controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
