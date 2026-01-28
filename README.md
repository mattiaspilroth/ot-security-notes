# OT Security Notes

Architectural reasoning and technical lessons learned while implementing OT security in large-scale industrial operations.

The focus is on how security controls behave over long operational lifecycles under real OT constraints, where availability, safety, and limited change capacity are primary design requirements.

---

## What You'll Find Here

- Architectural notes and design trade-offs (not maturity scores)
- Failure modes and long-lifecycle degradation behaviors
- Implementation constraints that make OT different from IT in practice
- Diagrams that support architectural reasoning and communication

**Not included:** site-identifying details, sensitive configurations, exploit instructions, or vendor-specific confidential information.

---

## Why These Notes Exist

OT security guidance is often written at a level of abstraction that assumes unlimited time, resources, and short technology lifecycles.

In production environments, implementation must account for structural constraints such as:

- **Distributed implementation responsibility**  
  Security requirements are often set centrally, while implementation depends on site teams with operational priorities and resource limitations.

- **Finite operational change capacity**  
  Industrial environments have limited tolerance for disruption. Controls must be implemented in ways that preserve production stability and repairability.

- **Long asset lifecycles**  
  Many OT systems operate for 15–30 years, creating constraints that do not align with IT refresh, patching, or replacement assumptions.

- **Cross-domain coordination requirements**  
  High-impact security controls often span IT and OT organizational boundaries, requiring coordination across different governance models and funding structures.
  
- **Observability under operational constraints**  
  Security architectures must provide sufficient visibility for detection and response while avoiding monitoring complexity that exceeds operational capacity to maintain.

These notes document how such constraints shape security implementations and what they imply for architectures that must function reliably over time.

---

## Notes

- [Why OT Infrastructure Appears Static](./notes/ot-infrastructure-appears-static.md)
- [Isolation Is Not Resilience](./notes/ot-isolation-vs-resilience.md)
- [OT Identity Architecture: Federation, PAM, and Resilience](./notes/ot-identity-architecture.md)

---

## Related Repositories

- **[ot-trust-in-isolated-networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  
  PKI, certificate validation, and trust continuity under constrained or intermittent connectivity.

---

## Discussion

These notes are intended to support architectural reasoning and informed debate rather than prescribe fixed solutions.

If you have observed similar or different failure modes, or have refined these patterns in your own practice, feel free to open an Issue or Discussion to contribute to the dialogue.

---

## About This Work & Disclaimer

This repository documents architectural patterns and lessons learned from implementing OT security in high-consequence industrial production environments.

This work documents patterns observed across multiple industrial environments and reflects personal professional judgment. It does not represent the views, guidance, or policies of any current or former employer, and contains no site-specific, confidential, or proprietary information.

---

## Contact

- [LinkedIn](https://www.linkedin.com/in/mattiaspilroth)
- [Email](mailto:mattias@pilroth.com)
