# Security Policy

## Authorized use only

`patchwise` is a knowledge corpus and planning tool for **authorized** security work: remediating, verifying and documenting defects in systems you own, or programs whose rules explicitly permit you to test them. The software itself performs no network requests — it renders, validates and plans locally.

The skills describe remediation and verification methodology. They contain no weaponized exploit material: the corpus is deliberately about what to do *after* a defect is confirmed — contain, fix, verify, harden, document, disclose.

## Scope discipline is built into the corpus

The opening skills exist to keep remediation work honest and bounded:

1. `faithful-reproduction-verification` — a fix is only real against a reproduction you can replay.
2. `blast-radius-containment-mapping` — containment scope is mapped before anything is shipped.
3. `credential-and-token-invalidation` — exposure containment includes everything the exposure touched.
4. `regression-watch-window` — a fix is not closed on the first green check.

A `fixplan` produced by the CLI always orders these before shipping steps.

## What this repository contains

- Methodology, checklists and diagnostic patterns for remediation and authorized verification.
- No exploit payloads, no credential harvesting techniques, no phishing material, no evasion guidance.
- No live target data, no credentials, no personal data. The example finding (`examples/demo-finding.json`) is synthetic.

## Reporting a vulnerability in this project

If you find a defect in `patchwise` itself (for example, a validation bypass that lets a malformed skill pass the gate), please open a private security advisory on the repository or contact the maintainer through GitHub. Do not publish exploit details before a fix is available.

## Scope of this project's own testing

All tests run locally against the checked-in corpus and synthetic example files. No test sends traffic anywhere; the `fixplan` command operates on a checked-in JSON finding. Continuous-integration runs the same local gates: validation, unit tests and an artifact-sync check.
