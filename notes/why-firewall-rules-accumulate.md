# Why Firewall Rules Accumulate in OT Networks

Firewall rule sets in OT networks rarely reflect intentional design. 
They are the historical residue of operational problem-solving.

Over time, rules accumulate because they once resolved an urgent issue during installation, maintenance or recovery.

---

## 1. Creation: The Troubleshooting Path of Least Resistance

Most firewall rules are born during high-pressure lifecycle events such as commissioning, upgrades, or recovery from failure. In these moments, the operational priority is to restore functionality quickly rather than to understand underlying dependencies.

The firewall becomes a diagnostic tool of last resort for several reasons:

- **Binary feedback**  
  Success is immediate and observable. If the system works after a rule change, the intervention is deemed successful.

- **Lack of alternatives**  
  Deeper investigation, such as enabling logs or correlating behavior across layers, is too time-consuming for active outages.

- **Invisible dependencies**  
  Trust, certificate validation, and DNS are rarely visible at the application level, forcing troubleshooting down to the network layer.

Under pressure, architectural clarity gives way to pragmatic fixes.  
Rules created in crisis are designed to restore service, not to express long-term security intent.

---

## 2. Mandates: The Vendor Support Gravity Well

Beyond internal troubleshooting, many rules exist to satisfy external support requirements.

Vendors often refuse to support a system unless specific ports are opened, frequently in a broad or loosely defined manner. Challenging these requirements carries significant organizational risk.

Internal teams often lack the authority, time, or leverage to negotiate alternatives during commissioning or outages. As a result, such rules become effectively contractually mandated and are rarely revisited, even when their original justification no longer applies.

---

## 3. The Validation Gap: The Absence of Staging

In traditional IT environments, connectivity changes can be validated in staging or development environments before reaching production. In OT, such high-fidelity replicas rarely exist.

The production environment is often the only place where the full set of real-time interactions between PLCs, HMIs, application servers, and vendor components can be observed. This creates a significant barrier to firewall rule hygiene.

Several factors reinforce this gap:

- **The high cost of failure**  
  Every attempt to tighten or remove a rule becomes a live experiment on a revenue-generating or safety-critical system.

- **The inability to simulate**  
  Simulators and digital twins frequently fail to reproduce proprietary protocols, legacy behaviors, or undocumented dependencies that rely on the very rules under consideration.

- **The compressed maintenance window**  
  Limited outage time is prioritized for functional changes and recovery activities, not for validating whether existing security rules are still required.

Without a safe environment to test the impact of rule removal, the only definitive way to validate necessity is to break the system in production.

This technical reality reinforces a “never touch a running system” philosophy, even when the running system is known to carry accumulated risk.

---

## 4. Retention: The Asymmetry of Risk

Firewall rule decisions are governed by an imbalance of accountability.

- **The penalty for removal**  
  Closing a rule carries visible and immediate risk. If functionality breaks, the impact is attributed directly to the person who made the change.

- **The penalty for retention**  
  Leaving a rule open carries abstract, deferred, and often unmeasured risk.

Because operational teams are rewarded for uptime rather than for reducing latent exposure, the safest individual decision is to leave the rule in place. What begins as a temporary exception gradually becomes part of the accepted baseline.

---

## 5. Erosion: The Loss of Network Intent

As the rationale behind rules fades due to staff turnover, vendor dependency and opacity, and incomplete handovers, the firewall transitions from a security control into a risk archive.

This leads to the *shadow dependency* problem. A rule initially added for a specific database connection may now unknowingly support a legacy reporting tool or a maintenance script that no one remembers installing.

When network intent is lost, rule removal is avoided not because rules are known to be required, but because their absence is perceived as unsafe. The firewall becomes a record of past change events rather than a reflection of present needs.

---

## An Emergent Pattern, Not a Failure of Discipline

The accumulation of firewall rules in OT networks is rarely the result of negligence or poor practice.

It is an emergent property of opaque dependencies, vendor constraints, asymmetric risk incentives, and the prioritization of availability over architectural clarity.

Understanding this pattern is a prerequisite for addressing it.
