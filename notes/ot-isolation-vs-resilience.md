# Isolation Is Not Resilience

## Purpose and Scope

This note examines a structural failure mode in long-lived OT environments where isolation conceals internal degradation until failure becomes imminent or unavoidable.

Isolation and segmentation are foundational OT security controls. They reduce external attack surface, limit lateral movement, and enforce trust boundaries. But isolation also constrains health monitoring, alarm propagation, and visibility into system degradation over time.

This note is not an argument against isolation as a security control. It documents a specific operational risk: in environments with 10-15 year lifecycles, systems that cannot signal their own health will eventually degrade silently, and isolation boundaries can prevent early detection.

The problem is particularly acute when:
* Redundant components share firmware versions, lifecycles, and failure modes
* Health indicators exist but cannot propagate beyond isolated zones
* Verification relies on commissioning artifacts or static inventory rather than live system behavior
* Common-cause dependencies are invisible in network topology

This note describes one such failure and its architectural implications for environments that must balance segmentation with selective observability.

---

## The Problem: Silent Degradation Under Isolation

In one OT virtualized environment designed for high availability:

* Compute hosts were redundant
* Core services were virtualized
* Basic health alarms were present
* No direct external connectivity existed

On paper, the environment appeared resilient.

In practice, the entire virtual stack depended on a single shared disk array with dual controllers. Both controllers ran identical firmware, had been in long-term continuous operation, and represented a common-mode dependency.

### The Incident

At some point, one controller failed. The remaining controller entered a degraded state, and cooling fans ran at maximum due to missing temperature feedback. No alarm reached operations or engineering teams because health signaling was confined within the segmented and isolated zone.

The issue was discovered only by chance when a technician noticed abnormal fan noise. Subsequent investigation, including escalation to the storage vendor under active support, revealed a known firmware fault affecting the controller model and a high probability that the remaining controller would have failed within weeks, taking the entire virtual OT stack offline.

From a reliability engineering perspective, this represents a classic *Common-Cause Failure (CCF)*, where redundant components fail due to shared firmware, lifecycle, and operating conditions rather than independent faults.

---

## Key Characteristics of the Failure

This failure mode had several notable properties:

* **Dynamic state vs. static inventory**  
  Verification of as-built designs or asset inventories does not reveal active hardware decay. Resilience depends on observing the live behavior of a system over time, not relying solely on static assumptions that disconnection implies safety.

* **Common-Cause Failure (CCF)**  
  Redundant components were exposed to the same firmware defects, operational timelines, and environmental conditions. Redundancy at the component level did not eliminate systemic risk.

* **Alarm containment**  
  Health indicators existed but did not propagate beyond the isolated zone.

* **Detection by chance, not design**  
  Discovery relied on incidental human proximity rather than engineered observability.

---

## Architectural Implications

This observation highlights an important distinction:

> Isolation protects against some risks, but it can also conceal others.

In OT environments, resilience requires more than redundancy and separation. It requires the ability to detect, signal, and reason about degradation over time.

This risk is particularly pronounced in OT environments with lifecycles spanning 10 to 15 years. In such environments, firmware defects related to prolonged uptime, resource exhaustion, or time-dependent behavior frequently surface late in the lifecycle, well after commissioning and initial validation.

Key implications include:

* Redundancy must be evaluated for common-cause failure, not just component count
* Segmented and isolated environments still require reliable, authenticated alarm paths
* Monitoring must consider aging, firmware defects, and correlated lifetimes
* Absence of alarms is not evidence of health

---

## Observability Under Segmentation and Isolation Constraints

Segmentation, isolation, and observability are often treated as opposing goals. In practice, resilient OT architectures must balance all three. While segmentation and isolation reduce exposure to certain external threats, they also limit visibility into system health and degradation over time.

Any mechanism introduced to provide observability across a segmentation or isolation boundary (for example gateways, data diodes, or overlay services) introduces its own logical footprint and potential failure modes. This introduces a **complexity tax** that must be explicitly considered in architectural design.

In practice, observability mechanisms often fail in predictable ways. Alert flooding from false positives causes operators to disable notifications or ignore the platform entirely, monitoring stacks become more complex than the systems they observe, and silent failures of health signaling paths create false confidence. These failure modes are not hypothetical, they reflect common patterns in environments where observability was added incrementally without sufficient attention to operational sustainability.

Across OT environments that remain diagnosable over long lifecycles, certain architectural characteristics recur:

* **Segmentation and isolation remain the default state**  
  Control and safety systems are typically not opened for convenience or visibility alone.

* **Health signaling is selective and minimal**  
  Only the information required to detect degradation and trigger response is allowed to propagate outward.

* **Observability sits outside the isolated zone**  
  Additional logic, agents, or dependencies are avoided inside legacy or safety-critical systems wherever possible.

* **Alarm paths are resilient to partial failure**  
  Loss of a single component or communication path must not suppress critical health signals.

* **Operational diagnosability is preserved**  
  On-site personnel must be able to understand and act on faults without reliance on external platforms or continuous central support.

Visibility does not require full connectivity. Carefully scoped telemetry paths can enable meaningful observability without introducing inbound functional paths for threats.

From a risk perspective, this balance is not symmetrical. Segmentation and isolation tend to reduce exposure to low-probability, high-impact cyber events, while simultaneously increasing exposure to high-probability, high-impact operational failures if degradation remains unseen. Resilient architectures must reason about both risks explicitly rather than optimizing for only one.

---

## When This Pattern Applies

While this example involved virtualized OT infrastructure with shared storage, the underlying pattern appears across many OT environments:

* Redundant field devices running identical firmware and lifecycles
* Backup or failover systems tested infrequently and aging in parallel
* Environmental dependencies such as cooling, power, or HVAC that are themselves isolated and lack external health signaling, preventing them from escalating degradation or failure conditions
* Architectures where common-mode dependencies are not visible in network topology or asset inventories

The fundamental risk is the same: systems that cannot signal their own degradation will eventually fail silently, and redundancy does not eliminate risk when components share common failure causes.

---

## Summary

Isolation is a common and often necessary design constraint in OT security, but resilience is determined by how systems behave over time, not by how disconnected they are.

Highly segmented or isolated environments can degrade quietly if they lack mechanisms to signal emerging failures beyond their boundaries. The objective is not comprehensive observability, but sufficient health signaling to detect degradation before it becomes catastrophic, while respecting the segmentation boundaries that isolation provides.

Systems must possess sufficient self-awareness to signal their approaching failure over time, especially when deliberate disconnection limits direct interaction with the wider network.
