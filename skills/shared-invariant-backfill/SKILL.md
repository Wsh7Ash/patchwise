---
name: shared-invariant-backfill
description: "When the fix tightens an invariant, backfill the historical data that violates it, or the fix remains true only for records from today onward. Use when the fixed check depends on state, and records written before the fix fail the new invariant"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [data, backfill, migrations]
---

# Shared Invariant Backfill

> When the fix tightens an invariant, backfill the historical data that violates it, or the fix remains true only for records from today onward

**Use when:** the fixed check depends on state, and records written before the fix fail the new invariant

**Avoid when:** the invariant exists only in request handling with no stored data, so no backfill surface exists

## Procedure

1. State the new invariant in data terms: which column or relation must satisfy what condition from now on.
2. Measure the volume of historical rows that violate the invariant, and record the count before any modification.
3. Decide per-category treatment: repair the data, quarantine it, delete it, or grant an explicit exception with a reason stamp.
4. Run the backfill in batches with a resumable checkpoint, never as a single destructive transaction.
5. Verify post-backfill with sampling and with the application's own read path, not only through a SQL statement.
6. Record the backfill script and its executed range, so later audit can prove which rows were touched and when.

## Signals

- The fix passes testing while a read path still surfaces pre-fix rows breaking the invariant anyway.
- Backfill proceeds without counting first, so the change is verified only by assumption counts that nobody measured.
- Deleted rows were also referenced by other entities, so the invariant breaks elsewhere invisibly while being repaired here.
- Sampling detects violations after backfill, meaning batch boundaries miscounted or the invariant was computed against expired state.

## Patterns

**1. Invariant**

```text
orders.user_id must reference an account owned by the same tenant -> N=4,182 violating rows
```

**2. Treatment**

```text
1,900: repair to correct tenant\n1,200: delete stale\n1,082: investigate separately
```

**3. Backfill**

```text
batch 1k, checkpoint each, re-run sample 100 -> 0 violations post-run
```

## Pitfalls

- Backfilling with a single transaction, which times out mid-migration and leaves the dataset in a partial state nobody revisits.
- Deleting violating rows without tracking their relationships, which repairs one invariant while breaking another silently.
- Fixing the fix and claim the count as complete when the checking reads the wrong density of rows.

## Done when

- The invariant is measured before and after the backfill, with verification through the application's real read path.
- Backfill is executed in resumable batches with an auditable range and treatment categories fully decided.

## References

- [NIST SP 800-53 data integrity control references](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
- [OWASP - database security cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Database_Security_Cheat_Sheet.html)
