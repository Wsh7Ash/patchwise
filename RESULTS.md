# Results

Reproducible evidence for the claims in `README.md`.

## Environment

- OS: Windows (MSYS2 shell), Python 3.14
- Runtime: Python 3.10+ standard library, `pytest` as dev dependency
- Dependency manager: `pip` / `python -m pip install -e ".[dev]"`
- Network: none required at any point

## Validation performed

| Gate | Result |
|---|---|
| install (`pip install -e ".[dev]"`) | PASS |
| build (`patchwise build`) | PASS (exit 0) |
| validation gate (`patchwise validate`) | PASS — **50/50 skills valid** |
| unit tests (`pytest -q`) | PASS — **16 passed** |
| demo run (`fixplan`, `search`) | PASS |
| secret scan | PASS (0 hits) |

## Commands executed

```shell
export PYTHONPATH=src
python -m patchwise build                  # regenerates skills/, pack.json, results/
python -m patchwise validate               # exit 0, prints 50/50
python -m pytest -q                        # 16 passed
python -m patchwise fixplan --finding examples/demo-finding.json > results/fixplan-demo.md
python -m patchwise search rotation rollback > results/search-example.txt
grep -rniE "(ghp_|github_pat_|AKIA|BEGIN.*PRIVATE KEY|xox[baprs]-|sk-)" .   # no matches
```

## Observed result

```text
built 50 skills -> .../patchwise/skills
patchwise: 50/50 skills valid
  ok  steps>=6 signals>=4 patterns>=3 pitfalls>=3 words>=180 refs canonical https
................                                                         [100%]
16 passed
```

## Fixes made during this build

While wiring the `fixplan` command and its tests, two real CLI bugs surfaced and
were fixed before the gates above were recorded:

1. `build_fixplan` built its keyword list from nested `list` objects
   (`category.split()` / `cwe.split()` inside the tuple), which raised
   `TypeError: cannot use 'list' as a set element`. Fixed by flattening with
   unpacked star-expressions and filtering the bare `cwe` token.
2. `build_fixplan` referenced `args.top` directly, so calling it from tests
   (or any non-CLI path) raised `NameError`. Fixed by adding a `top` parameter
   with a default of 12, which `cmd_fixplan` now passes through.

Both are regression-covered by `tests/test_corpus.py::test_cli_fixplan_orders_phases_and_flags_thin_coverage`
and `...::test_cli_fixplan_rejects_missing_finding_file`.
