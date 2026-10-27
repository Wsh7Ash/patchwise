---
name: reproduction-planning
description: "Plan the exact reproduction before touching the system: what precondition, what request or input, and what observation counts as success. Use when a report claims a behavior you have not yet seen and you need an honest attempt to see it or fail to see it"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [reproduction, verification, triage]
---

# Reproduction Planning

> Plan the exact reproduction before touching the system: what precondition, what request or input, and what observation counts as success

**Use when:** a report claims a behavior you have not yet seen and you need an honest attempt to see it or fail to see it

**Avoid when:** the evidence is already a complete, monitorable reproduction supplied by the reporter

## Procedure

1. Extract the reporter's exact preconditions: version, configuration, account state, feature flags and data shape.
2. Build the reproduction environment you are entitled to use, matching preconditions one by one rather than approximately.
3. Convert the narrative steps into a scripted sequence whose inputs and outputs are both recorded.
4. Define success criteria in advance: which observation, on which response or log, verifies the reported behavior exists.
5. Run the script once unchanged; if it fails, vary one precondition at a time and log which variation mattered.
6. Record the final reproducible sequence, its environment and the fraction of the reporter's conditions that were needed.

## Signals

- Reproduction succeeds only under a configuration the reporter never mentioned, which the report should now record.
- Success requires a race or ordering, making the reproducible script flaky and the severity judgment harder.
- The reporter's environment cannot be replicated exactly, so the reproduction result is partial and the record must say so.
- Success happens at once but no observation satisfies the scripted success criterion, indicating an unmodeled side channel.

## Patterns

**1. Repro script**

```text
env: v2.3.1, flag=x700-off
GET /api/reset (no origin) -> 200 + token valid -> reported behavior confirmed
```

**2. Vary-one**

```text
flag on: refuse; flag off: accepted -> precondition=flag x700-off not in report
```

**3. Record row**

```text
F-0113 reproduced=yes complexity=single flaky=no preconditions-missing=[flag]
```

## Pitfalls

- Reproducing on a system version the report never claimed to affect, then calling the report invalid.
- Declaring non-reproduction after a single attempt with one precondition set, when the report named several.
- Recording reproduction success without the success criterion, which lets strong conclusions ride on weak observations.

## Done when

- A scripted, repeatable reproduction exists with preconditions, success criteria and environment recorded.
- The mismatch between reported and required preconditions, if any, is noted for follow-up with the reporter.

## References

- [OWASP Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [CWE - description reference](https://cwe.mitre.org/)
