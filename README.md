# OT Security Notes

Architectural reasoning and technical lessons learned while implementing OT security in large-scale industrial operations.

The focus is how security controls behave in real production environments where availability, safety, and change capacity are primary design constraints.

These notes describe patterns observed across multiple industrial organizations. They are not specific to any single company.

## What You'll Find Here

- Architectural notes and design trade-offs (not maturity scores)
- Failure modes and "soft degradation" behaviors
- Implementation constraints that make OT different from IT in practice
- Diagrams that support reasoning and communication

**Not included:** site-identifying details, sensitive configurations, exploit instructions, or vendor-specific confidential information.

## Why These Notes Exist

OT security guidance is often written at a level of abstraction that assumes unlimited time, resources, and operational change capacity.

In production environments, implementation must account for structural constraints:

- **Distributed implementation responsibility**  
  Security requirements are often set centrally, while implementation depends on site teams with operational priorities and resource limitations.

- **Finite operational change capacity**  
  Industrial environments have limited tolerance for disruption. Controls must be implemented in ways that preserve production stability.

- **Cross-domain coordination requirements**  
  High-impact security controls often span IT and OT organizational boundaries, requiring coordination across different governance models and funding structures.

- **Trade-offs between visibility and enforcement**  
  Security architectures must balance the need for threat detection with the operational constraints of maintaining production systems.

These notes document how these constraints shape security implementations and what they imply for architectures that must function under real operational conditions.

## Notes

- [Why OT Infrastructure Appears Static](./notes/00-why-ot-infrastructure-appears-static.md)
- [Isolation Is Not Resilience](./notes/ot-isolation-vs-resilience.md)
- [OT Identity Architecture: Federation, PAM, and Resilience](./notes/ot-identity-architecture.md)

## Related Repositories

- **[ot-trust-in-isolated-networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  
  PKI, certificate validation, and trust continuity under constrained or intermittent connectivity.

## About This Work

This repository documents patterns observed across multiple industrial organizations while implementing OT security.

The analysis reflects professional experience in production environments across industrial sectors, but does not represent the views, guidance, or policies of any employer, vendor, or organization.

The intent is to document architectural patterns, not to prescribe universal solutions.
