# OT Security Notes

This repository documents architectural reasoning and technical lessons learned while designing and implementing OT security across large-scale industrial operations spanning multiple countries.

It serves as an analytical layer for understanding how OT security systems behave in real-world industrial environments, where availability, safety, and operational continuity routinely collide with enterprise IT expectations.

The observations documented here reflect common patterns encountered across multiple industrial organizations and are not specific to any single company.

## Why These Notes Exist

Much OT security guidance is written at a level of abstraction that assumes unlimited time, resources, and change capacity. In practice, security requirements are often interpreted as aspirational “best practice” targets rather than risk-informed, sustainable baselines. This typically occurs when the necessary analysis of operational feasibility is bypassed in favor of generic mandates.

These notes focus on the **implementation gap** that emerges when security expectations move from policy into production environments without a shared understanding of what is practically achievable. Rather than failing outright, many OT security programs become busy, fragmented, and difficult to sustain, while actual operational risk remains largely unchanged.

Across environments where this pattern repeats, a small set of structural conditions are consistently absent:

* **The Ownership Gap**
  OT security falls into a responsibility vacuum between enterprise IT governance and local operations, leaving no function fully accountable for long-term implementation outcomes.

* **Risk Prioritization Gaps**
  Security activity is driven by generic best-practice targets, audit expectations, or safe-harbor interpretations, rather than by a shared understanding of which operational risks are most material in a given context.

* **Misaligned Resourcing**
  Responsibility for implementation is placed at site level without corresponding authority, budget, or dedicated capacity, limiting execution to what can be absorbed opportunistically.

* **Lack of Sustained Operational Ownership**
  Controls are introduced as projects or initiatives but lack a clear long-term owner within the line organization, leading to gradual degradation once initial attention fades.

An additional consequence of these patterns is the **misallocation of limited operational change capacity**. Effort is often directed toward controls that are visible, defensible, or auditable rather than toward risks that are most significant for the operation. Over time, this leads to local fatigue, reduced trust in security initiatives, and repeated cycles of activity with little measurable improvement in resilience.

The focus of these notes is not on individual products or frameworks, but on recurring patterns in how security requirements are translated into operational reality. This translation ultimately determines whether an OT security program produces **measurable risk reduction** or devolves into **formal compliance activity**.

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
