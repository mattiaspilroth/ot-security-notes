# OT Security Notes

This repository documents architectural reasoning and technical lessons learned while designing and implementing OT security across large-scale industrial operations spanning multiple countries.

It serves as an analytical layer for understanding how OT security systems behave in real-world industrial environments, where availability, safety, and operational continuity routinely collide with enterprise IT expectations.

The observations documented here reflect common patterns encountered across multiple industrial organizations and are not specific to any single company.

## Why These Notes Exist

Much OT security guidance is written at a level of abstraction that assumes unlimited time, resources, and change capacity. In practice, security requirements are often interpreted as aspirational “best practice” targets rather than risk-informed, sustainable baselines. This typically occurs when the necessary analysis of operational feasibility is bypassed in favor of generic mandates.

These notes focus on the **implementation gap** that emerges when security expectations move from policy into production environments without a shared understanding of what is practically achievable:

* **The Ownership Gap**
  How OT security can fall into a responsibility vacuum between enterprise IT governance and local operations, leaving no function fully accountable for actual implementation outcomes.

* **Policy Interpretation vs. Operational Reality**
  How enterprise security policies, when treated as generic best practice rather than context-specific risk controls, often fail to achieve their intended effect at the site level.

* **Framework Misalignment**
  Where frameworks such as IEC 62443 are applied as static compliance checklists or “safe-harbor” mechanisms to bypass the time-consuming work of site-specific investigation. This often results in frameworks being experienced as externally imposed constraints rather than used as tools for cooperative risk prioritization.

* **Sustainable Resilience**
  Identifying which security controls a local maintenance organization can realistically operate and sustain over time, given lifecycle constraints, vendor dependencies, and limited change windows.

* **Organizational Readiness**
  How funding models, incentives, and internal governance structures determine whether security is implemented collaboratively or experienced as an administrative obligation.

An additional consequence of these patterns is the **misallocation of limited operational change capacity**. When organizations default to generic best-practice targets as safe-harbor strategies, effort is often directed toward controls that are visible and auditable rather than toward risks that are most material in a given operational context. Over time, this leads to local fatigue, reduced trust in security initiatives, and, paradoxically, little or no reduction in actual operational risk.

The focus is not on individual products, but on recurring patterns in how security requirements are translated into operational reality. This translation ultimately determines whether an OT security program produces **measurable risk reduction** or devolves into **formal compliance activity**.

## What Enables Sustainable OT Security

Across environments where OT security produced lasting risk reduction rather than temporary compliance, several common conditions were present:

* **Resources allocated where implementation happens**
  Sites had dedicated capacity, authority, and budget to implement and maintain controls.

* **Collaborative risk prioritization**
  Enterprise and site stakeholders jointly assessed material risks rather than relying solely on predefined control lists.

* **Realistic implementation timelines**
  Security work accounted for maintenance windows, vendor constraints, and operational dependencies.

* **Sustained operational ownership**
  Controls could be operated by the line organization without continuous central intervention.

* **Outcome-focused metrics**
  Progress was measured in risk reduction and resilience, not documentation volume or maturity scores.

These patterns are uncommon, but repeatable when organizational structure and incentives support them.

## Scope and Intent

These notes explore the friction between governance and engineering, including:

* How security controls behave under degraded or constrained conditions
* Why common IT security assumptions often fail when applied directly to OT
* How risk accumulates through incremental and soft failures rather than discrete incidents
* The gap between assessed security posture and observed operational reality
* How lifecycle economics and resourcing models drive long-term technical stasis

The emphasis is on **system behavior** and **implementation constraints** rather than static configurations or maturity scoring.

## Relationship to Other Repositories

This repository provides the overarching architectural reasoning. Focused deep-dives into specific problem areas are maintained separately:

* **[ot-trust-in-isolated-networks](https://github.com/mattiaspilroth/ot-trust-in-isolated-networks)**  
  Analysis of PKI, certificate validation, and trust continuity under constrained or intermittent connectivity.

Additional repositories will be added as specific architectural patterns are developed and documented.

## Structure

* **`/notes`**
  Analytical notes and architectural reflections on OT resilience, governance, and organizational friction.

* **`/diagrams`**
  Simple diagrams supporting architectural reasoning and risk discussions.

## About This Work

This repository documents patterns observed across multiple industrial organizations during enterprise OT security architecture and implementation work.

The analysis reflects professional experience implementing OT security at scale, but does not represent the views, guidance, or policies of any employer, vendor, or organization.

The intent is to document architectural patterns, not to prescribe universal solutions.
