# patchwise

**50 validated vulnerability-remediation skills, plus a CLI that turns a finding report into an ordered, phase-grouped fix checklist.**

## The problem

Fixing a confirmed defect well is a whole sequence: reproduce honestly, contain the exposure, ship the minimal fix, verify beyond the first green check, harden against the class, then document and disclose without creating a second incident. The methodology exists in scattered postmortems and runbooks, but almost never as checkable, ordered skill files — so the first rollout to face a defect re-decides the sequence from scratch.

## What this repo does

`patchwise` is a corpus of **50 unique skills** organised into six phases of a remediation arc — `triage → contain → fix → verify → harden → document` — with a validator that enforces structural and quality floors on every skill, and a CLI whose domain command (`fixplan`) turns a structured finding report into an ordered remediation checklist drawn from the corpus.

Every skill is a self-contained procedure with a summary, use/avoid guidance, a numbered procedure, observable signals, concrete patterns, pitfalls and completion criteria.

### Key features

- **50 skills, all validated**: `validate` enforces 6+ steps, 4+ signals, 3+ patterns, 3+ pitfalls, 180+ words, canonical `https` references, no placeholder tokens, no machine-local paths, unique slugs and titles, and byte-identical agreement with the files on disk.
- **`fixplan`**: turns a JSON finding report (title, category, CWE, tags) into a phase-ordered checklist, scoring the corpus against the finding's vocabulary and flagging phases with thin coverage.
- **`validate`**: the "50/50 valid" gate. Exits non-zero on any violation, with `--json` for CI.
- **`build`**: deterministically regenerates `skills/`, `pack.json` (SHA-256 per skill) and `results/`.
- **`list` / `show` / `search`**: browse and search the corpus locally, no network required.
- **Zero runtime dependencies**: Python standard library only; `pytest` is the single dev dependency.

### Terminal example

```console
$ python -m patchwise validate
patchwise: 50/50 skills valid
  ok  steps>=6 signals>=4 patterns>=3 pitfalls>=3 words>=180 refs canonical https

$ python -m patchwise fixplan --finding examples/demo-finding.json
# Fix plan - Cross-tenant order read via supplier-supplied identifier
Skills matched: 21 of 50   Plan steps: 12

## contain
  ...
$ python -m patchwise search rotation rollback
  9  credential-rotation-ordering                Credential Rotation Ordering
  ...
```

## Architecture

```text
corpus (Python dicts)  --skill_from_dict-->  Skill dataclasses
        |                                          |
        |                    +---------------------+
        v                    v
   render.py  --SKILL.md-->  skills/<slug>/SKILL.md  + pack.json (sha256)
        |                    results/catalog.md, results/validation.json
        v
   validate.py  --rules-->  Issue list  -->  CLI exit code (0 = 50/50 valid)
   cli.py --fixplan-->      phase-ordered remediation checklist from a finding
```

- **Corpus** (`src/patchwise/corpus/`): the source of truth, three readable files of authored skill dicts.
- **Model** (`model.py`): typed `Skill` shape plus strict dict validation.
- **Render** (`render.py`): pure, deterministic renderer and pack builder — same input, byte-identical output.
- **Validate** (`validate.py`): all structural, strength and hygiene rules in one place.
- **CLI** (`cli.py`): shared subcommands plus patchwise's domain command `fixplan`.

Generated files are committed; tests assert committed files match a fresh render, so drift is a test failure rather than a silent surprise.

## Requirements

- Python 3.10+ (tested on 3.14)
- `pytest` for the test suite

## Installation

```shell
python -m pip install -e ".[dev]"
```

## Usage

```shell
python -m patchwise validate            # 50/50 gate
python -m patchwise list                # all skills
python -m patchwise list --phase contain
python -m patchwise show blast-radius-containment-mapping
python -m patchwise search rotation rollback
python -m patchwise build               # regenerate skills/, pack.json, results/
python -m patchwise fixplan --finding examples/demo-finding.json
```

## Corpus map

| Phase | Skills | Arc stage |
|---|---|---|
| triage | 9 | report intake, repro verification, ownership routing |
| contain | 8 | blast-radius mapping, credential and data containment |
| fix | 9 | root-cause framing, patch packaging, rollout ordering |
| verify | 8 | deterministic re-run, fleet sweep, regression watch, signoff |
| harden | 9 | guard rails, canaries, sweeps, adjacent-class trend analysis |
| document | 7 | postmortems, disclosure, release notes, record archival |

## License

MIT — see [LICENSE](LICENSE).

## Scope and authorized use

`patchwise` documents remediation and verification methodology for defects in **systems you own or are authorized to modify**. The corpus performs no network operations; it renders, validates and plans locally.
