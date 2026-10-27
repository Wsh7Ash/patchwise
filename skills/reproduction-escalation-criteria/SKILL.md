---
name: reproduction-escalation-criteria
description: "Decide when unreproducible reports escalate to a wider investigation rather than decline, using criteria fixed before the queue fills. Use when repros fail for several reports in a row and each one may be a real defect or an artifact of your environment"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [escalation, environment, triage]
---

# Reproduction Escalation Criteria

> Decide when unreproducible reports escalate to a wider investigation rather than decline, using criteria fixed before the queue fills

**Use when:** repros fail for several reports in a row and each one may be a real defect or an artifact of your environment

**Avoid when:** your environment exactly matches the reporter's and reproduction working is already the norm

## Procedure

1. Define standard vs escalated reproduction effort: what attempts, what environments, what time budget counts as standard.
2. Apply the standard effort to every unreproducible report and record which attempt each declined after.
3. Identify reproducible patterns in what fails: specific components, specific versions, specific conditions that differ.
4. Escalate only against agreed criteria: multiple independent reports, infrastructure mismatch, or automated anomalies.
5. On escalation, review the environment build steps themselves, since an environment breakage can underlie streaks of failure.
6. Either reproduce at the escalated level, or close the record with the full effort trail attached for auditability.

## Signals

- Reproduction fails across several unrelated reports at once, which itself correlates with a shared environmental change.
- The failing reports cluster on a component or version where your testing setup diverges from deployment.
- Reproduction succeeds only twice in ten runs, so flakiness is itself an environmental finding worth tracking separately.
- Escalated attempt succeeds in a colleague's environment, indicating a working assumption you could not see was wrong.

## Patterns

**1. Standard budget**

```text
3 attempts, 2 env, 30 minutes each, one teammate cross-check
```

**2. Escalate when**

```text
3+ independent reporters, prod-only behavior, config divergence detected
```

**3. Trace line**

```text
environment change: deploys since 2026-10-20 -> new TLS termination -> reproduces in prod-like env
```

## Pitfalls

- Escalating every non-reproduction, which exhausts effort while reports that clustered from a broken environment remain.
- Closing unreproducible reports silently without the trail needed to defend the closure decision.
- Reproducing in an environment that differs from the reporter's specification, which is sanitization rather than testing.

## Done when

- Non-reproduction escalates by declared criteria rather than by intuition.
- Every closure or escalation carries the available reproduction trail, so others inherit the reasoning not just a status.

## References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [Google BugHunter disclosure policy reference](https://bughunters.google.com/)
