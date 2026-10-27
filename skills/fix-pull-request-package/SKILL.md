---
name: fix-pull-request-package
description: "Package the fix so reviewers get one coherent unit: failing test, fix, regenerated assets, updated docs and an explicit scope note. Use when a reviewed production fix is moving from branch toward merge, and reviewer effort is worth batching deliberately"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [pull-request, review, process]
---

# Fix Pull Request Package

> Package the fix so reviewers get one coherent unit: failing test, fix, regenerated assets, updated docs and an explicit scope note

**Use when:** a reviewed production fix is moving from branch toward merge, and reviewer effort is worth batching deliberately

**Avoid when:** the organization's release policy requires unit-level commits that decompose the same package anew

## Procedure

1. Open the PR with the defect identifier, reproduction summary and the one-sentence fix hypothesis.
2. Include the failing test first, then the fix commits so reviewers see the failure by work order rather than by reading   code.
3. Regenerate any impacted artifacts on the same PR, with a build artifact diff so reviewers see what the regeneration proposes.
4. Update user-facing documentation the fix changes, including release notes with the defect identifier.
5. Attach the fix-scope note via an explicit 'Not in scope' list, so the boundary remains auditable after merge.
6. Track the PR as it traverses review, capturing review phases until merge, deployment and post-verification.

## Signals

- Reviewers ask for a failing test because the fix arrived without one, which costs reviewer attention that packaging should have saved.
- Artifact regeneration appears in the PR as an unexplained blob diff, hiding regeneration hazards that later fail releases.
- Release notes and PR description disagree, which usually means a real fix detail was lost in translation one direction or the other.
- Scope notes written for review vanish from the merged artifact, so the next refactor encounters the boundary unexplained.

## Patterns

**1. PR body text**

```text
defect: F-0113\nhypothesis: one sentence\nrepro: attached steps + failing test
```

**2. Ordering plan**

```text
commit 1: failing test\ncommit 2: fix\ncommit 3: regenerate policy files\ncommit 4: docs
```

**3. Range row**

```text
changed: orders.py:97 + policy regen\nout-of-scope: refactor idea filed separately
```

## Pitfalls

- Keeping the failing test out of the PR, which removes the beginning of the reviewer's reasoning and delays approval.
- Excluding regenerated artifacts, so the fix ships with a stale policy file that quietly permits the vulnerable flow.
- Omitting scope notes, which guarantees the boundary that kept the fix small survives nowhere accessible to history.

## Done when

- The PR packages test-first ordering, hypothesis, regenerated artifacts, documentation updates and an explicit scope note.
- The scope list matches the actual diff exactly, and the docs reflect the fix behavior verified by the tests.

## References

- [Google engineering practices code review](https://google.github.io/eng-practices/review/)
- [GitHub - PR guidance reference](https://docs.github.com/get-started/quick-start/git-and-github-learning-resources)
