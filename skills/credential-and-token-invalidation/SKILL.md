---
name: credential-and-token-invalidation
description: "When a defect leaks credentials or tokens, invalidate every affected session and credential atomically, then prove what was invalidated. Use when the confirmed defect exposed tokens, keys, passwords, cookies or long-lived grants to actors who should not have them"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [sessions, tokens, invalidation]
---

# Credential and Token Invalidation

> When a defect leaks credentials or tokens, invalidate every affected session and credential atomically, then prove what was invalidated

**Use when:** the confirmed defect exposed tokens, keys, passwords, cookies or long-lived grants to actors who should not have them

**Avoid when:** the leak is still hypothetical and no credential has left a protected boundary

## Procedure

1. Enumerate the credential surfaces the defect could have exposed: session cookies, bearer tokens, API keys, refresh tokens, service accounts and signed URLs.
2. Compute the exposure window: when the defect first existed and when it was contained, so invalidation targets a bounded interval.
3. Invalidate sessions and tokens for the exposed surface, allowing an emergency batch call rather than one-by-one manual work.
4. Force re-authentication where invalidation cannot retroactively revoke what has already been consumed.
5. Notify the affected accounts honestly, in plain language, while avoiding instructions that assume the reporter's account.
6. Record every invalidation action with timestamp, surface and scope, so the timeline can be audited and cross-checked against the defect.

## Signals

- Some exposed credentials continue authorizing calls even after invalidation, because the revocation applies to a different token surface.
- The exposure window extends backwards beyond the visible defect, because logging shows the defect has been reachable for longer.
- Long-lived service credentials sit out of rotation and remain functional even after the session invalidation.
- User-facing notification reveals confusion because the report's timeline and the actual exposure window diverge.

## Patterns

**1. Surface map**

```text
leaked surfaces: session cookies + personal access tokens; exposure window: 2026-10-20..10-28
```

**2. Invalidation**

```text
revoke all session rows for user set A (4k sessions) revoked=at 15:02
rotate API keys for service-tokens after window
```

**3. Audit row**

```text
post-invalidation check: refresh continues -> 401 revoked=yes
```

## Pitfalls

- Invalidating a narrow surface and assuming every exposed credential was the same type of credential.
- Setting the exposure window to the moment the report arrived, which understates how long leaked credentials had been usable.
- Forgetting service accounts and long-lived keys, which commonly outlive interactive session invalidation and reappear.

## Done when

- Every leaked credential surface within the computed window has been tested as revoked and the revocation timeline recorded.
- Affected users receive honest notification within a defined timeframe, and account owners are not left to reconstruct their own exposure.

## References

- [OWASP - session management cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html)
- [NIST SP 800-63B - digital identity guidance](https://pages.nist.gov/800-63-3/sp800-63b.html)
