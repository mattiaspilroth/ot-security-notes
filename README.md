# OT Security Notes

Architectural reasoning and lessons learned from implementing security in large-scale industrial operations.

This repository examines how security controls behave over long operational lifecycles in production OT environments, where availability, safety, and limited change capacity are structural constraints, not tunable priorities.

## Approach

These notes follow several guiding principles:

- **Architecture over compliance**  
  Design trade-offs, system behavior, and failure modes rather than control checklists.

- **Observed failure modes**  
  Long-term degradation patterns and operational reality in production environments.

- **Structural constraints**  
  Why common IT assumptions around patching, refresh cycles, and centralized management do not hold in OT.

## The Implementation Gap

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

### Core Framework
- [Why OT Infrastructure Appears Static: A Process Industry Perspective](./notes/why-ot-appears-static.md)  
  Explainer for IT and cybersecurity professionals on operational constraints in process industries.

### Technical Analysis
- [Silent Degradation under IT/OT Convergence](./notes/silent-degradation-under-convergence.md)  
  How isolation can conceal degradation and observability challenges in converged environments.

- [OT Identity Architecture: Federation, PAM, and Resilience](./notes/ot-identity-architecture.md)  
  Three access planes model and identity architecture adapted to industrial constraints.

## Related Repositories

- **[OT Trust in Isolated Networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  
  PKI, certificate validation, and trust continuity under constrained or intermittent connectivity.

## Discussion

These notes are intended to support architectural reasoning rather than prescribe solutions.

If you have observed similar or different failure modes, or have refined these patterns in your own practice, feel free to open an Issue or Discussion.

## About and Disclaimer

This repository contains architectural analysis and practitioner observations derived from hands-on work securing OT systems in high-consequence industrial production environments.

The material reflects the author's professional judgment and implementation experience across multiple industrial contexts. It is published in an independent capacity and does not represent the views, guidance, or policies of any current or former employer.

No site-identifying details, sensitive configurations, exploit instructions, or vendor-confidential information are included.

## Contact

- [LinkedIn](https://www.linkedin.com/in/mattiaspilroth)
- [Email](mailto:mattias@pilroth.com)
