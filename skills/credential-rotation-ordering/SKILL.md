---
name: credential-rotation-ordering
description: "Rotate exposed credentials in dependency order, generating new secrets first so services never double-fail during the swap. Use when exposure requires rotating keys, passwords or tokens across several services that depend on each other"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [rotation, credentials, ordering]
---

# Credential Rotation Ordering

> Rotate exposed credentials in dependency order, generating new secrets first so services never double-fail during the swap

**Use when:** exposure requires rotating keys, passwords or tokens across several services that depend on each other

**Avoid when:** only one isolated credential needs replacing and nothing depends on the rotation order

## Procedure

1. Draw the dependency graph among affected credentials: which services read which secrets, and which secrets are shared.
2. Generate new credentials before revoking old ones, so every consumer can be migrated from a working state.
3. Migrate leaf consumers first, then mid-tier services, and finally shared or root credentials that everything hangs from.
4. Verify each migrated consumer makes at least one authenticated call before revoking the credential it just left.
5. Revoke old credentials only when the migration metric shows no consumer still reaching for them.
6. Record the rotation ledger: credential, new version, consumers migrated and revocation timestamps, for audit.

## Signals

- A service fails after rotation because a shared secret was swapped while its consumer cached the previous value.
- Two services share one credential, so rotation must be sequenced or one of them always ends up broken.
- Consumers read credentials from several stores, and one store was missed, so the old value reappears after revocation.
- A rotation completes but a scheduled job awakens days later still carrying the retired credential in its own config.

## Patterns

**1. Rotation order**

```text
shared secret S used by api+worker -> 1) issue S' 2) migrate api 3) migrate worker 4) revoke S
```

**2. Verify tactic**

```text
from each consumer: 200 authenticated call -> migrate next
401 -> rollback the consumer only
```

**3. Rotation ledger**

```text
S: issued 15:10, migrated api 15:14, worker 15:20, revoked 15:31
```

## Pitfalls

- Revoking before migrating, which turns one security incident into concurrent availability incidents across consumers.
- Missing secondary stores or cached copies, so a revoked credential keeps authenticating through a forgotten consumer.
- Rotating shared credentials without sequencing consumers, which makes each rotation a coin flip on whether every service survives.

## Done when

- Every affected credential has been replaced with consumers migrated in dependency order, verified by an authenticated call each.
- The rotation ledger records issuance, migration and revocation timestamps so later audits can see the true exposure closure.

## References

- [OWASP - secrets management cheat sheet](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
- [NIST SP 800-63B - credential guidance](https://pages.nist.gov/800-63-3/sp800-63b.html)
