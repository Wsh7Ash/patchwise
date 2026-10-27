# Usage

## Setup

```shell
git clone https://github.com/Wsh7Ash/patchwise.git
cd patchwise
python -m pip install -e ".[dev]"
```

## Day-to-day commands

```shell
python -m patchwise validate               # the 50/50 gate (exit 0 = all valid)
python -m patchwise validate --json        # machine-readable report for CI
python -m patchwise list                   # all 50 skills, one line each
python -m patchwise list --phase contain   # only containment skills
python -m patchwise list --tag rotation    # filter by tag
python -m patchwise search rotation rollback   # rank by keyword hits
python -m patchwise show credential-rotation-ordering  # full SKILL.md
python -m patchwise build                  # regenerate skills/, pack.json, results/
```

## Turning a finding into a fix plan

Write a small JSON finding report (or copy `examples/demo-finding.json`):

```json
{
  "title": "Cross-tenant order read via supplier-supplied identifier",
  "category": "broken-access-control",
  "cwe": "CWE-639",
  "tags": ["idor", "orders", "multi-tenant"]
}
```

Then:

```shell
python -m patchwise fixplan --finding examples/demo-finding.json
```

Output is a phase-ordered checklist: skills grouped under `## triage`,
`## contain`, `## fix`, `## verify`, `## harden`, `## document`, each with
matching steps and the reason it matched. Phases that matched nothing are
listed under *Thin coverage* so you can see what the plan does not yet address.
Add `--json` for a machine-readable payload and `--top N` to cap plan length.

## Loading skills into agents

Each generated file `skills/<slug>/SKILL.md` has YAML front-matter
(`name`, `description`, `pack`, `version`, `category`, `tags`), which
agent skill loaders consume directly. Point your agent at the `skills/`
directory, or pick specific skills with `pack.json`'s per-skill paths and
SHA-256 digests.

## Regeneration discipline

Never edit `skills/`, `pack.json` or `results/` by hand. Edit the corpus
dicts in `src/patchwise/corpus/part*.py`, run `python -m patchwise build`,
then run the tests — they compare committed files with a fresh render, so
drift fails CI instead of silently shipping.

## Scope

All commands work offline against the local corpus. `fixplan` reads a JSON
finding you supply; it performs no network operations and renders no exploit
guidance.
