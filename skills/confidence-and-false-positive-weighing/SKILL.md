---
name: confidence-and-false-positive-weighing
description: "Weigh every judgment against how confident you actually are, and pre-register what would change your mind so false positives surface earlier. Use when reproduction is partial, uncertain or environment-dependent and further effort is competing with other work"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [confidence, false-positive, triage]
---

# Confidence and False-Positive Weighing

> Weigh every judgment against how confident you actually are, and pre-register what would change your mind so false positives surface earlier

**Use when:** reproduction is partial, uncertain or environment-dependent and further effort is competing with other work

**Avoid when:** the reproduction is complete and the defect is unambiguous, where confidence adds only process overhead

## Procedure

1. State what is observed directly versus what is being assumed about behavior outside the tested environment.
2. Name the specific test or check that would convert an assumption into an observation.
3. Estimate, in plain language, what fraction of such reports historically turn out to be genuine.
4. Pre-register disconfirming evidence for this record: which observation, if seen, would invalidate it.
5. Decide whether further effort is justified now, later by a larger fix, or never without reporter input.
6. Record the confidence level with the exact disconfirming condition, so future revisits start from the same state of knowledge.

## Signals

- Reproduction depends on a scenario that also happens under a wide range of ordinary behavior, which causes false positives to recur.
- The defect appears only on your environment, and the alternatives you would need remain unreachable.
- Two confident claims conflict, meaning one record's assumptions must be wrong somewhere and both need re-examination.
- The reporter's supplementary data, when added, contradicts the earlier severity reasoning rather than strengthening it.

## Patterns

**1. Assumption list**

```text
observed: 200 for peer object
assumed: deterministic across deployments
pre-register: 403 in original sandbox would falsify
```

**2. Confidence row**

```text
confidence=medium observes=1/2 preconditions disconfirm-when=`403 on staging`
```

**3. Action row**

```text
hold effort: ask reporter one question, not a re-implementation of their report
```

## Pitfalls

- Treating failure to reproduce once as proof of invalidity, when conditions differ between test and production.
- Recording a confidence label with nothing that could change it, which makes later review pointless.
- Escalating uncertainty upward instead of naming the one test that resolves it, which displaces but never removes doubt.

## Done when

- Every uncertain record has an explicit assumption list and one pre-registered disconfirming observation.
- Effort decisions (continue, hold, escalate, drop) are recorded against the assumption the decision rests on.

## References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [CWE guidance for ambiguous defect classes](https://cwe.mitre.org/)
