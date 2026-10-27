# Project Spec — patchwise

## Name

`patchwise`

## Category

`defensive-security`

## Target user

Engineering teams, fixers and security agents who have a confirmed finding and need to contain it, fix it, verify the fix actually landed and close the record honestly.

## Problem

Remediation methodology is scattered across postmortems and tribal knowledge. Each new arc re-decides the same things: whether the reproduction was faithful, what containment should cover, in which order multiple secrets must be rotated, when a fix is verified beyond the first green check, and how to close the record without a second incident. Nothing is machine-checkable, and agents have no structured skill files to load.

## Solution

A corpus of 50 unique, validated remediation skills organised into the six phases of a fix arc, with a validator that enforces quality floors and a CLI that turns a structured finding report into a phase-ordered checklist with thin-coverage warnings.

## Core workflow

```text
author/edit corpus  ->  build  ->  skills/ + pack.json + results/
                                    |
                                    v
                          validate (50/50 gate)  ->  pytest  ->  CI
                                    |
                                    v
                       fixplan --finding report.json -> ordered checklist
```

## Must-have features

- 50 unique skills with unique slugs and titles, covering triage, contain, fix, verify, harden, document.
- Validator with structural, strength and hygiene rules; exit `0` only at 50/50.
- Deterministic renderer; committed files proven in sync by tests.
- `fixplan` producing a phase-ordered checklist from a JSON finding report, with phase coverage gaps surfaced.

## Nice-to-have (explicitly out of scope for v1)

- Auto-detection of affected entry paths from source code.
- Integration with incident tooling APIs.
- A scoring model beyond vocabulary matching.

## Acceptance tests

1. `validate` exits 0 and reports 50/50.
2. `pytest -q` passes 16 tests, including regeneration equality and drift detection.
3. `fixplan` — round-trips the checked-in example finding.
4. A generated build lands byte-identically in a clean temp directory.
