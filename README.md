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

In production environments, a predictable gap often emerges between **what can be demonstrated** (artifacts, metrics, assessments) and **what deterministically reduces operational risk**.

This is rarely about incompetence. More often it results from structural conditions that shape how security work is funded, sequenced, and sustained:

- **Distributed authority without centralized resources**  
  Requirements are set centrally, but implementation relies on site teams with competing operational priorities and limited dedicated capacity.

- **Evidence-driven investment**  
  Work gravitates toward controls that produce visible artifacts (dashboards, reports, compliance metrics) rather than controls that enforce boundaries or constrain access.

- **Organizational boundary friction**  
  High-impact controls often sit between IT governance and OT operations, where ownership, accountability, and funding can be unclear.

- **Limited operational change capacity**  
  Industrial environments have finite tolerance for disruption. This favors controls that can be overlaid with minimal coordination—even when foundational enforcement would reduce more risk.

The result is not necessarily failure, but resource allocation patterns that optimize for **legibility and defensibility** more than **risk reduction**.

These notes document how that gap manifests in implementations, and what it implies for security architectures that must survive real operational constraints.

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
