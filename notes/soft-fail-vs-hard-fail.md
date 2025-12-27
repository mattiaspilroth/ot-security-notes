# Soft Fail vs Hard Fail in OT Security Systems

## Overview

In OT environments, the most critical security failures are often not
explicit connection denials or blocked communications.

Instead, risk accumulates through *soft failures*:
trust-related failures that degrade silently over time without triggering
alerts, alarms, or immediate operational impact.

This distinction between **hard fail** and **soft fail** behavior is central
to understanding why many security controls appear compliant on paper,
yet fail operationally in real OT systems.

## Hard Fail

A hard fail is a failure mode where the system:

- Explicitly refuses to operate
- Fails fast and visibly
- Produces immediate and observable impact

### Typical examples

- Application refuses to start due to invalid certificate
- TLS handshake fails and connection is rejected
- Driver installation aborts due to missing signature
- Service crashes when dependency validation fails

### Characteristics

- Easy to detect
- Often discovered during installation, upgrade, or commissioning
- Highly disruptive, but visible
- Forces immediate remediation

In OT, hard fails are often *avoided by design* due to availability and safety
concerns. Systems are expected to continue operating even under degraded
conditions.

## Soft Fail

A soft fail is a failure mode where the system:

- Continues operating
- Degrades security posture silently
- Accumulates technical and trust debt over time

### Typical examples

- Certificate validation skipped due to unreachable CRL or OCSP
- Expired certificates accepted with warnings suppressed
- Fallback to cached or static trust anchors without verification
- Security features disabled to avoid operational disruption
- Time drift causing validation logic to behave unpredictably

### Characteristics

- No immediate operational impact
- Rarely visible in logs or alarms
- Often unnoticed by operators and engineers
- Can persist for years in isolated environments

Soft failures are particularly dangerous in OT because they create
a *false sense of security*.
The system appears functional, compliant, and stable, while trust assumptions
are slowly eroding.

## Why Soft Fail Dominates in OT

Several structural factors in OT environments favor soft fail behavior:

- Limited or no external connectivity
- Long system lifecycles and infrequent maintenance windows
- Strong availability and safety priorities
- Legacy systems with incomplete validation logic
- Manual or absent certificate lifecycle management

As a result, many OT systems are designed to:
> "Fail open rather than fail closed"

This design choice is rarely documented, tested, or reassessed over time.

## The Invisible Risk Accumulation

Soft fail mechanisms often stack:

- Cached trust stores never refreshed
- CRL distribution points unreachable for years
- Certificates renewed manually without full validation
- Local overrides introduced during incidents and never removed

Each individual workaround appears harmless.
Collectively, they erode the integrity of trust relationships.

This creates systems that are:
- Operationally stable
- Formally compliant
- Cryptographically untrustworthy

## Assessment Blind Spots

Traditional assessments often miss soft failures because they focus on:

- Configuration state at a point in time
- Presence of controls, not their runtime behavior
- Worst-case impact rather than likelihood and degradation
- Zone and conduit abstractions without trust-flow analysis

Soft failures live between these abstractions.

They emerge from:
- Time
- Isolation
- Operational pragmatism
- Human intervention

## Implications for OT Security Architecture

Addressing soft fail behavior requires:

- Treating trust as a *runtime dependency*, not a static configuration
- Designing for degraded connectivity explicitly
- Making trust failures observable without causing hard fails
- Separating safety-driven availability decisions from silent security bypass

In OT, resilience is not achieved by eliminating failure,
but by making failure *visible, bounded, and recoverable*.

## Closing Reflection

Hard fails stop systems.
Soft fails hollow them out.

In many OT environments, the most serious security incidents will not be
sudden or dramatic.
They will be the result of trust assumptions that quietly stopped being true
years earlier.
