# Changelog

All notable changes to `patchwise` are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/); versions follow [SemVer](https://semver.org/).

## [1.0.0] - 2026-10-27

### Added

- Initial release: 50 unique remediation skills across six phases
  (`triage`, `contain`, `fix`, `verify`, `harden`, `document`).
- Validator with structural, strength and hygiene rules; `50/50 skills valid`
  gate with `--json` output for CI.
- Deterministic renderer and `build` command writing `skills/`,
  `pack.json` (SHA-256 per skill) and `results/`.
- CLI: `validate`, `list`, `show`, `search`, `build`, `fixplan`.
- `fixplan` — structured finding report to phase-ordered remediation checklist.
- Test suite (16 tests) covering corpus shape, rules, regeneration, drift and
  every CLI command.
- CI workflow, docs (`ARCHITECTURE`, `USAGE`, `PROJECT_SPEC`), `SECURITY.md`.

### Fixed

- `build_fixplan`: keyword extraction no longer feeds unflattened lists into
  the term filter (`TypeError: cannot use 'list' as a set element`).
- `build_fixplan`: `args.top` is no longer referenced outside the command
  layer; the function takes a `top` parameter defaulting to 12.
