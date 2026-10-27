---
name: fix-verification-signoff
description: "Close the defect record with a signed-off verification statement that names who verified what, against which evidence, so the record can stand alone. Use when a fix has gathered enough verification evidence and the record finally needs a durable closure declaration"
pack: patchwise
version: 1.0.0
category: defensive-security
tags: [closure, signoff, evidence]
---

# Fix Verification Signoff

> Close the defect record with a signed-off verification statement that names who verified what, against which evidence, so the record can stand alone

**Use when:** a fix has gathered enough verification evidence and the record finally needs a durable closure declaration

**Avoid when:** the fix has no verification evidence yet, in which case signoff is a ritual rather than a statement

## Procedure

1. Collect the evidence a closure needs: original reproduction, verification rerun, fleet sweep results and watch-window summary.
2. Name the verifier explicitly, and separate author and verifier roles where policy requires it.
3. State the environment and build that carried the verification, since the same bugfix cannot verify itself across all builds.
4. State the residual risk: any case the fix does not cover, related sibling defects which remain open, or channel delays that matter.
5. Sign off with a durable record: text stored with the finding, not merely an email or a channel message.
6. Keep the closure record retrievable by the original report identifier so future astronomers can find the whole arc.

## Signals

- A closure record cannot be found later, so disputes over earlier closure rely on memory alone.
- Closure statements never name the environment, so a later fix lands in a different one and distorts the trajectory.
- Residual risk is absent, since closure means zero risk, making the fix record itself untruthful when compared to reality.
- One engineer both authored the fix and verified it, satisfying no rigor requirement the organization had declared.

## Patterns

**1. Signoff block**

```text
F-0113 closed 2026-11-01 by verifier-b, evidence set: reproduction, rerun, fleet sweep, watch
```

**2. Environment**

```text
verified on v2.3.2-oct31 deploy + images rebuilt
```

**3. Residual**

```text
unresolved: LTS channel 2 days behind; F-0118 sibling defect open
```

## Pitfalls

- Verifying from your own location step without independent evidence, so the signoff describes your belief rather than observed state.
- Disregarding residual risk, which future incidents cite when asking why the fix missed a neighboring flow.
- Closing without retaining the receipts, which makes the record impossible to audit later.

## Done when

- The closure record contains named verifier, environment, evidence set and residual risk, and lives with the finding permanently.
- Linked follow-up items are explicitly tracked rather than carried in the closure's prose, so they cannot be lost in the summary.

## References

- [ISO 27001 - evidence handling guidance](https://www.iso.org/standard/27001)
- [NIST SP 800-53 - record keeping controls](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final)
