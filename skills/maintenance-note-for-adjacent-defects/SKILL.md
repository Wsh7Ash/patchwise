---
name: maintenance-note-for-adjacent-defects
description: "Write the maintenance note that connects this fix to adjacent defects either reported or suspected but concluded differently, so future arcs inherit the connective tissue. Use when the fix arc sent related but unmerged reports and the next engineer should read them together rather than as isolation"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [maintenance, documentation, linking]
---

# Maintenance Note for Adjacent Defects

> Write the maintenance note that connects this fix to adjacent defects either reported or suspected but concluded differently, so future arcs inherit the connective tissue

**Use when:** the fix arc sent related but unmerged reports and the next engineer should read them together rather than as isolation

**Avoid when:** the reports are wholly independent in surface, class and cause, and connecting them would create noise rather than signal

## Procedure

1. List the adjacent reports and records: the fix class's own finding entry, plus anything that shared a surface, symptom or vocabulary.
2. For each adjacent item, state the connection in one line: shared surface, shared cause, or a conclusion that differed and why.
3. Distinguish connection from causation, so the note never implies one defect caused another when they were parallel discoveries.
4. Insert the note with each one-line link and record it under the defect identifier the arc actually closed, not a broader label.
5. Point the note at the canonical records rather than describing them inline, since drift would otherwise split the truth.
6. Revise the note when an adjacent record concludes differently than expected, instead of leaving the first-read impression standing.

## Signals

- Two records claim incompatible outcomes, and reading either as truth requires the other's context that the note should supply.
- The same surface produced three reports in one quarter and future arcs will re-investigate the same fixtures instead of inheriting them.
- An adjacent record's terminology has evolved since this fix, leaving the current note disconnected from the class language.
- Cross-references point at records one paragraph describes as related and another as independent, which the note should clarify.

## Patterns

**1. Connect list**

```text
adjacent: F-0087 (same endpoint, different class), F-0071 (shared cause - missing owner filter)
```

**2. One-liner**

```text
F-0071: same root cause, distinct symptom (export, not API) -> handled by same fix commit
```

**3. Diff row**

```text
causation: none; both independent reports same quarter -> note reflects parallel discovery
```

## Pitfalls

- Writing the note once and reviving drift later, because records across time disagree about what the first paragraph claimed.
- Making every adjacency a causal claim, which escalates records into a relationships diagram nobody resolves during triage.
- Failing to re-check adjacent record state at rewrite, which lets the note point to outcomes that have since changed.

## Done when

- Each adjacent record has a one-line connection: shared surface, shared cause, or a distinct conclusion with its reason.
- The note distinguishes connection from causation and points to canonical records rather than restating them inline.

## References

- [OWASP - documentation and knowledge management guidance](https://cheatsheetseries.owasp.org/)
- [NIST SP 800-53 - audit controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
