# OT Security: Structural Constraints & Operational Reality

Working notes on long-lifecycle security in high-consequence industrial environments.

Based on architecture work across production sites where long lifecycles, limited change capacity, and operational accountability shape what can actually be sustained.

The central question:

**Which security controls survive operational reality, and which degrade predictably despite appearing sound on paper?**

These notes examine patterns observed in practice: what holds, what erodes, and the structural reasons why.

## Notes

Each note addresses a distinct pattern. They can be read independently.

### Operational Constraints

**[Why OT Infrastructure Appears Static](./notes/why-ot-appears-static.md)**
Stability in continuous process industries is an engineered response to asymmetric risk, not technical stagnation. Explains the constraints that make OT infrastructure resistant to change and why ignoring them leads to unimplementable security strategies.

**[Silent Degradation Under IT/OT Convergence](./notes/silent-degradation-under-convergence.md)**
Redundant IT infrastructure inside segmented OT zones degrades invisibly when health signals cannot reach anyone who can act. The gap is not telemetry—it is ownership.

### Identity and Trust

**[OT Identity Architecture: Federation, PAM, and Residual Risk](./notes/ot-identity-architecture.md)**
A framework for reasoning about identity patterns in OT. Explores trade-offs between isolation, federation, and hybrid models, and why authority for high-consequence actions must remain local.

**[Trust in Isolated OT Networks](./notes/trust-in-isolated-networks.md)**
Certificate validation assumes trust material is continuously obtainable. In segmented OT, architecture prevents this. Explains why trust degrades silently and why discovery is deferred until crisis.

**[Trust Flow in Constrained OT Environments](./notes/trust-flow-in-constrained-environments.md)**
Defines the properties that must hold for validation to occur predictably, repeatedly, and without improvisation inside constrained zones.

## Perspective

These notes favor:

* Architectural reasoning over compliance checklists
* Observed operational behavior over design intent
* Long-term durability over short-term elegance

The goal is useful analysis, not comprehensive coverage.

## Discussion

If you have observed different patterns, identified gaps, or have experience that challenges these conclusions, your perspective is welcome.

Open an issue or start a discussion.

## About

Written by **Mattias Pilroth**, working with OT security architecture across European chemical and process industry environments.

Independent work documenting patterns observed in practice. Does not represent employer positions and avoids sensitive or identifying details.

[LinkedIn](https://www.linkedin.com/in/mattiaspilroth)
