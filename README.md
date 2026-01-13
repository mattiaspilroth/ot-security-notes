# OT Security Notes

This repository contains personal technical notes and reflections on
OT (Operational Technology) security in safety-critical and industrial
environments.

It serves as a conceptual and analytical layer for reasoning about how
OT security systems behave over time, particularly in environments where
availability, safety, and operational continuity dominate design decisions.

The focus is not on individual products or controls, but on recurring
patterns, failure modes, and architectural trade-offs observed in
real-world industrial systems.

## Scope and intent

These notes explore questions such as:

- How security controls behave under degraded or constrained conditions
- Why traditional IT security assumptions often fail in OT environments
- How risk accumulates through soft failures rather than explicit security incidents
- The gap between assessed security posture and observed operational reality
- How trust, identity, and connectivity assumptions decay over time
- How organizational resourcing and lifecycle economics dictate technical stasis

The emphasis is on *system behavior* rather than static configuration.

## Relationship to other repositories

This repository provides the overarching reasoning and mental models.
More focused deep-dives into specific problem areas may exist in
separate repositories.

For example:
- **[ot-trust-in-isolated-networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  explores one concrete manifestation
  of these patterns in the context of PKI, certificate validation, and
  trust management.

## Structure

* **`/notes`** — Analytical notes and architectural reflections regarding OT resilience
* **`/diagrams`** — Simple diagrams to support architectural reasoning

Content is intentionally lightweight and primarily text-based.
It evolves over time and is not expected to be exhaustive or polished.

## Disclaimer

All content represents personal technical observations.
It does not represent the views, guidance, or policies of any employer,
vendor, or organization.
