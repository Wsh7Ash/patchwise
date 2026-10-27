# Architecture

## Goals

1. **One source of truth.** Skills are authored as Python data; Markdown files are generated. A test asserts committed files match a fresh render, so the two can never disagree.
2. **Validation is a first-class gate.** Every quality floor lives in `validate.py` and is enforced by both the CLI and the test suite.
3. **No runtime dependencies.** Standard library only, so the pack runs anywhere Python 3.10+ exists.

## Components

```text
src/patchwise/
├─ corpus/           authored skill data (source of truth)
│  ├─ __init__.py    PACK metadata + loads part1..part3
│  ├─ part1.py       skills 1-18   (triage, contain)
│  ├─ part2.py       skills 19-33  (fix, verify)
│  └─ part3.py       skills 34-50  (harden, document)
├─ model.py          Skill dataclass + strict dict loading
├─ render.py         deterministic SKILL.md + pack.json + results writer
├─ validate.py       every rule, one place
└─ cli.py            validate / list / show / search / build / fixplan
```

### Data flow

```text
part1..part3 (dicts)
      │  skill_from_dict()  -- shape check, rejects missing/empty fields
      ▼
   Skill (frozen dataclass)
      │
      ├─ validate_pack()  ─▶ Issue list ─▶ CLI exit code (0 only at 50/50)
      └─ render_skill()   ─▶ skills/<slug>/SKILL.md  (byte-deterministic)

fixplan (finding.json)
      │  score corpus against finding vocabulary (tags, category, CWE)
      ▼
   phase-ordered checklist  (triage → contain → fix → verify → harden → document)
      with thin-coverage warnings for phases that matched nothing
```

### The fixplan command

`fixplan` is patchwise's domain command and separates this pack from a document library. It consumes a small finding report:

```json
{ "title": "...", "category": "broken-access-control", "cwe": "CWE-639", "tags": ["idor", "orders"] }
```

and scores every skill against the finding's vocabulary (tag hits and word
occurrences in slug/title/summary/tags), then orders the best matches by
engagement phase. Phases with no matched skill are reported as thin coverage,
so an unchecked expectation is visible rather than silent.

### Design decisions

- **`fixplan` is intentionally small.** It encodes vocabulary matching and
  phase ordering, not a remediation policy engine. Product-specific judgment
  belongs to the reader, and thin-coverage warnings make gaps explicit rather
  than guessing.
- **Whitespace stability.** The renderer normalizes to LF endings and the
  build writes with `newline="\n"`, so regeneration is byte-identical across
  platforms; drift tests would otherwise fail spuriously on Windows checkouts.
- **Layer ownership.** Command-line concerns only in `cli.py`; data rules only
  in `validate.py`; file writing only in `render.py`.
