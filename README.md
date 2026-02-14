# OT Security: Structural Constraints & Operational Reality

This repository contains working notes on securing operational technology in high-consequence industrial environments.

The ideas come from architecture work across production sites where long lifecycles, limited change capacity, and operational accountability shape what can be sustained over time.

The central question:

**Which security controls remain effective over decades in production OT, and which degrade predictably under operational pressure?**

These notes examine patterns observed in practice: what holds, what erodes, and the structural reasons why.

## What you will find here

**[Why OT Infrastructure Appears Static](./notes/why-ot-appears-static.md)**  
Stability in continuous process industries is an engineered response to asymmetric risk, not technical stagnation. This note examines the constraints that make OT infrastructure resistant to change.

**[Silent Degradation Under IT/OT Convergence](./notes/silent-degradation-under-convergence.md)**  
Redundant IT infrastructure introduced into segmented OT zones can degrade invisibly when health signals cannot reach operations teams. The gap is not telemetry; it is ownership.

**[OT Identity Architecture: Federation, PAM, and Residual Risk](./notes/ot-identity-architecture.md)**  
A framework for reasoning about identity patterns in OT. Examines the trade-offs between isolation, federation, and hybrid models, and why authority for high-consequence actions must remain local.

Each note stands independently. Read in any order.

## Perspective

These notes favor:

- Architectural reasoning over compliance checklists
- Observed operational behavior over design intent
- Long-term durability over short-term elegance

The goal is useful analysis, not comprehensive coverage.

## Related Repositories

- **[OT Trust in Isolated Networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  
  PKI, certificate validation, and trust continuity under constrained or intermittent connectivity.

## Discussion

If you have observed different patterns, identified gaps in the reasoning, or have experience that challenges these conclusions, I would value hearing it. Open an issue or start a discussion.

## About

Written by **Mattias Pilroth**, working with OT security architecture across European chemical and process industry environments.

This is independent work based on professional experience. It does not represent employer positions and avoids sensitive or identifying details.

[LinkedIn](https://www.linkedin.com/in/mattiaspilroth)
