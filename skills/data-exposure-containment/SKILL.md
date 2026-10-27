---
name: data-exposure-containment
description: "Stop an active data-exposure leak by closing the path, then measure what actually left, without inventing conclusions beyond the logs. Use when a confirmed exposed endpoint or public artifact was serving data it should not have, and exposure may span hours or days"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [exposure, containment, logs]
---

# Data Exposure Containment

> Stop an active data-exposure leak by closing the path, then measure what actually left, without inventing conclusions beyond the logs

**Use when:** a confirmed exposed endpoint or public artifact was serving data it should not have, and exposure may span hours or days

**Avoid when:** nothing has been exposed yet, and containment is preventive rather than measured

## Procedure

1. Close the exposure path itself first: fix the endpoint, protect the artifact, or break the route that serves the data.
2. Compute what the exposure covered: which objects, fields and tenants, from the moment it opened to the moment it closed.
3. Query access logs to determine which identities requested the exposed data and how often, staying within your own entitlements.
4. Separate suspicious access from ordinary traffic, since legitimate users may have been reading data they were meant to see.
5. Contact affected data owners if the exposure crossed their boundaries, before they discover it external to your process.
6. Record what closed the leak, when, and what the measurement showed, keeping the honest lower bound explicit where logs are incomplete.

## Signals

- Access logs show reads from identities that should never have held the data, during the exposure window.
- The exposure span is wider than any visible change log, so the defect may predate release tracking.
- A cache or CDN edge copies persist after the origin closes, which means the leak continues at another layer.
- Affected tenants are anonymous within your logs, since your chosen correlation fields shift with identity sources.

## Patterns

**1. Close path**

```text
disable /api/documents/public (rounding only tenant admins)
read close 2026-10-28T14:31Z
```

**2. Access read**

```text
log range 2026-10-20..10-28: 91 callers, 14 unknown-token, tenant-B-requested 8 DOCs
```

**3. Cache sweep**

```text
CDN stale copy sweep -> 3 edge locations invalidated after origin closed
```

## Pitfalls

- Closing the path and stopping there, never measuring what left, so the response underestimates real exposure.
- Incorrectly treating all access as suspicious, when legitimate users were in the same windows being counted.
- Copying caches stay valid after origin closure, which means the leak continues at another layer.

## Done when

- The exposure path is closed at every layer including caches and edges, verified with a fresh request.
- Access measurements are recorded with explicit lower bounds where the logs are incomplete.

## References

- [OWASP - logging cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html)
- [NIST SP 800-61 - incident handling guidance](https://csrc.nist.gov/pubs/sp/800/61/r2/final)
