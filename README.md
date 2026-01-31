# OT Security Notes

Architectural reasoning and lessons learned from implementing security in large-scale industrial operations.

This repository examines how security controls behave over long operational lifecycles in production OT environments, where availability, safety, and limited change capacity are hard design constraints rather than priorities.


## Principles and Scope

These notes follow several guiding principles for evaluating security in production OT environments:

- **Architecture over compliance**
Design trade-offs and system behaviors rather than control checklists.

- **Operational reality**
Failure modes and long-term degradation patterns observed in production environments.

- **Structural constraints**
Why common IT assumptions around patching, refresh cycles, and centralized management do not hold in OT.

- **Architectural reasoning**
Diagrams and written analysis intended to support durable decision-making and stakeholder communication.


## Operational Reality

Much OT security guidance is developed under assumptions that do not fully reflect production environments.

In practice, implementation outcomes are shaped by structural constraints and gaps such as:

- **Distributed implementation responsibility**
Security intent is often defined centrally, while implementation depends on site teams with operational accountability and limited resources.

- **Finite operational change capacity**
Production systems have limited tolerance for disruption. Controls that undermine repairability or stability tend to erode over time.

- **Long asset lifecycles**
OT systems commonly operate for decades, creating lifecycle mismatches with IT security planning assumptions.

- **Cross-domain governance boundaries**
High-impact controls frequently span IT and OT organizational models, funding structures, and decision authority.

- **Observability under operational constraints**
Visibility is only sustainable when monitoring, alerting, and response can be maintained by local teams over time.

These notes explore how such gaps influence real-world security outcomes.


## Notes

- [Why OT Infrastructure Appears Static](./notes/ot-infrastructure-appears-static.md)
- [Isolation Is Not Resilience](./notes/ot-isolation-vs-resilience.md)
- [OT Identity Architecture: Federation, PAM, and Resilience](./notes/ot-identity-architecture.md)


## Related Repositories

- **[OT Trust in Isolated Networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  
  PKI, certificate validation, and trust continuity under constrained or intermittent connectivity.


## Discussion

These notes are intended to support architectural reasoning and informed debate rather than prescribe fixed solutions.

If you have observed similar or different failure modes, or have refined these patterns in your own practice, feel free to open an Issue or Discussion.


## About and Disclaimer

This repository contains notes on architectural patterns and lessons learned from implementing OT security in high-consequence industrial production environments.

It reflects personal professional judgment informed by direct implementation experience and observations across multiple industrial environments. It does not represent the views, guidance, or policies of any current or former employer, and contains no site-specific, confidential, or proprietary information.

**Not included:** site-identifying details, sensitive configurations, exploit instructions, or vendor-specific confidential information.


## Contact

- [LinkedIn](https://www.linkedin.com/in/mattiaspilroth)
- [Email](mailto:mattias@pilroth.com)
