---
name: report-intake-normalization
description: "Convert every incoming defect report, regardless of template, into one internal schema so downstream triage never re-parses free text twice. Use when reports arrive from several channels: vendor forms, email, coordinated disclosure, internal QA and automated scanners"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [intake, triage, normalization]
---

# Report Intake Normalization

> Convert every incoming defect report, regardless of template, into one internal schema so downstream triage never re-parses free text twice

**Use when:** reports arrive from several channels: vendor forms, email, coordinated disclosure, internal QA and automated scanners

**Avoid when:** a single trusted intake format already exists and every field arrives machine-readable

## Procedure

1. Define the internal finding schema: identifier, title, affected component, observed behavior, expected behavior, evidence, reporter, timestamp and stated conditions.
2. Map each intake channel's fields onto that schema, and record which channel fields have no schema home.
3. Normalize every free-text description into the structured fields, keeping the original text attached as the reporter's own account.
4. Mark each report with its channel and reliability tier, so later weightings can be explicit rather than intuitive.
5. Attach evidence exactly as provided, then separately note what the reporter says versus what the evidence shows.
6. Store the normalized record with a stable internal identifier that every later phase references.

## Signals

- Two channels carry equivalent information shaped so differently that no shared field name fits both.
- The reporter's narrative and the attached evidence disagree, which marks the record for verification rather than acceptance.
- A critical field such as affected version is missing from most reports coming through one channel.
- Automated scanner output arrives with deduplication-hostile field values that produce repeated near-identical records.

## Patterns

**1. Schema row**

```text
id=F-2026-0113 channel=vendor-form affected=auth v=2.3.1 observed=token-accepted expected=refuse
evidence=attached-har
```

**2. Channel map**

```text
vendor-form: env->affected, steps->observed
cve-proxy: desc->title_only (needs manual extraction)
```

**3. Record line**

```text
F-2026-0113 normalized=yes channel-gap=affected-version confidence=medium
```

## Pitfalls

- Editing the reporter's words while normalizing, which destroys the audit trail a later dispute requires.
- Assigning severity during intake, before any reproduction, so early guesses fossilize into labels nobody revisits.
- Normalizing only the structured channels, leaving email reports permanently second-class with weaker records.

## Done when

- Every open report exists in the shared schema with source channel, evidence links and an internal identifier.
- Field-level channel gaps are recorded, and no severity or validity judgment has been attached yet.

## References

- [ISO 29147 - vulnerability disclosure](https://www.iso.org/standard/72311.html)
- [CWE - categorization reference](https://cwe.mitre.org/)
