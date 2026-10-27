---
name: defect-record-archive-and-handbook
description: "Archive the complete defect record and fold its entries into the handbook index, so each arc is retrievable standalone and navigable as a whole. Use when a fix arc ends and the archive should be retrievable by both identifier and future query"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [archive, record, handbook]
---

# Defect Record Archive and Handbook

> Archive the complete defect record and fold its entries into the handbook index, so each arc is retrievable standalone and navigable as a whole

**Use when:** a fix arc ends and the archive should be retrievable by both identifier and future query

**Avoid when:** the fix arc produced no lessons beyond what the finding record already documents comprehensively

## Procedure

1. Confirm the record's evidence surfaces are complete: reproduction, verification, signoff, disclosure, and follow-up notes.
2. Archive the record in the channel it established, with the finding identifier as the canonical key rather than a personal note.
3. Add a handbook entry summarizing the defect class, patterns, and fix shape under the archive channel's existing tooling.
4. Cross-link the handbook entry with the finding record, so each arc's consumer can navigate both directions.
5. Run a search on the handbook index by the key terms an engineer would search for, since retrieval is quality control rather than storage.
6. Keep the archive immutable, and route changes to a new version entry rather than a mutation of an existing baseline.

## Signals

- A quick search reproduces the full arc, meaning future engineers will succeed where past records succeeded in queries as well.
- Handbook index does not surface a pattern vibe for this class, so the record exists but discovery within the handbook does not.
- Archived record has missing evidence entries, which pinches the baseline against future audits that may come from a colleague.
- Archive mutates in-place without versioning, which means future readers cannot resolve facts as recorded at archive time.

## Patterns

**1. Archive tree**

```text
findings/F-2026-0113/: reproduction, verification, signoff, disclosure, handbook entry
```

**2. Index row**

```text
handbook: defect classes -> ownership-missing (refs F-0113, F-0087)
```

**3. Retrieval**

```text
search by 'cross-tenant order' surfaces F-0113 first; F-0087 second
```

## Pitfalls

- Archiving without the evidence base, since that record becomes stale if verification trails are missing and cannot be re-added.
- Storing the handbook and finding records without mutual links, leaving each arc retrievable only through one tack.
- Testing retrieval quality only once at archive time, never rechecking it as class traffic grows and patterns shift.

## Done when

- The archive record is complete, immutable for its evidence layer, and confirmed retrievable under terms an engineer would search.
- The handbook entry cross-links in both directions so engineers can navigate from class to arc and arc to class without rebuilding.

## References

- [NIST SP 800-53 - record retention controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
- [OWASP - document retention guidance](https://cheatsheetseries.owasp.org/)
