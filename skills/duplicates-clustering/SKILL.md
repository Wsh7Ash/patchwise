---
name: duplicates-clustering
description: "Cluster newly arriving reports against the existing backlog so duplicates attach to the first record instead of spawning duplicate workstreams. Use when the queue holds many similar reports for the same component and manual comparing is already failing"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [deduplication, triage, queue]
---

# Duplicate Clustering

> Cluster newly arriving reports against the existing backlog so duplicates attach to the first record instead of spawning duplicate workstreams

**Use when:** the queue holds many similar reports for the same component and manual comparing is already failing

**Avoid when:** the queue is small enough that reading every report is still cheap and reliable

## Procedure

1. Choose matching keys for a finding: affected component, defect class, endpoint or file, and precondition set.
2. Compute keys for the arriving report and for open backlog records, comparing the same fields against the same fields.
3. Group candidates by key-prefix and rank them by shared keys before manual reading begins.
4. Read only the ranked candidates closely and decide: duplicate, related-but-distinct, or new.
5. When a duplicate is confirmed, merge it into the first record as an additional reporter with a timestamp.
6. When related reports turn out distinct, record why the cluster was split, so future reports key correctly.

## Signals

- Multiple reports describe the same endpoint and behavior yet were filed as unrelated because wordings differ.
- Deduplication by title alone merges genuinely distinct defects that a precondition comparison would separate.
- A reopened defect arrives as a fresh report because nobody threads earlier duplicates into the same record.
- The affected-version field differs while the defect behavior is identical, indicating a range rather than a point fix.

## Patterns

**1. Key match**

```text
arriving: comp=auth file=login.py class=weak-hash
existing F-2026-0098: same + same -> duplicate filed-as-reporter-2
```

**2. Split rule**

```text
same endpoint + same class but different precondition -> related, not duplicate
```

**3. Rank row**

```text
candidates=4 read=2 duplicate=1 distinct=1
```

## Pitfalls

- Clustering on surface text such as titles, which both over-merges aliases and under-merges rewordings.
- Marking related reports as duplicates, losing a second angle on the same component that the fix should also cover.
- Dropping the duplicate's reporter from the record, which later breaks notification obligations when the fix ships.

## Done when

- Every arriving report has an explicit duplicate decision with the key comparison that produced it.
- Confirmed duplicates are merged with reporter attribution, and clusters are split where preconditions genuinely differ.

## References

- [CWE lookup](https://cwe.mitre.org/)
- [MITRE CVE program - backlog handling reference](https://cve.org/)
