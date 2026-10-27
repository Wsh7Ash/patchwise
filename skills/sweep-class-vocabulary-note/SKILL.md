---
name: sweep-class-vocabulary-note
description: "Record the vocabulary the arc produced, so future reviewers, sweepers and documentation inherit identical language for the same evidence. Use when the fix arc surfaced concepts that the codebase currently calls by different names in different places"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [vocabulary, glossary, documentation]
---

# Sweep Class Vocabulary Note

> Record the vocabulary the arc produced, so future reviewers, sweepers and documentation inherit identical language for the same evidence

**Use when:** the fix arc surfaced concepts that the codebase currently calls by different names in different places

**Avoid when:** the arc produced no new concepts and the existing vocabulary is the vocabulary future work should inherit

## Procedure

1. List the terms the arc coined or gave new weight to, alongside the terms the codebase currently uses for them.
2. Pick a canonical term for each concept, prefer one already in use over a new coin, and record the aliases rather than hiding them.
3. Write the glossary with each entry carrying a definition, one example, and cross-references to the arc's records.
4. Route the glossary to the place the next engineering query hits, so the vocabulary appears where the question is asked.
5. Update the glossary on later arcs that sharpen or retire a term, recording the change with its date rather than silently rewriting it.
6. Keep the glossary shorter than the number of terms in the arc, since a glossary beyond readability is a shelf nobody opens.

## Signals

- The class has a term with four synonyms growing in different repositories, which the glossary should consolidate.
- A new engineer's first report uses the old term and its triage loses time until the vocabulary catches up.
- The glossary has not been updated in months, and the arc's sharpest defined term does not appear there.
- Sweeps across codebases disagree because one uses 'cross-tenant read' while another uses 'peer read'.

## Patterns

**1. Term row**

```text
canonical: peer-read
aliases: cross-tenant-read, lateral-read
def: read of another tenant's record, regardless of auth state
example: F-0113
```

**2. Canonical pick**

```text
peer-read existed in code comments -> chosen over new coin 'lateral-read'
```

**3. Revision row**

```text
2026-11-20: new term bulk-sibling-read added from F-0102 arc
```

## Pitfalls

- Coining a new term when an existing one fits, which increases the vocabulary's surface faster than its usefulness.
- Recording aliases without stating which is canonical, leaving future writers to pick per-arc and lose consistency.
- Omitting the arc's record cross-references, so glossary readers cannot tell what the term was derived from or where it applies.

## Done when

- Each coined or weighted term has an entry carrying the canonical pick, the aliases, and a cross-reference to the arc's record.
- The glossary lives in the channel the next question queries and is revised on later arcs rather than rewritten silently.

## References

- [OWASP - documentation style for security writing](https://cheatsheetseries.owasp.org/)
- [Google developer documentation style guide](https://developers.google.com/style)
