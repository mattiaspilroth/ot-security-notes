# OT Identity Architecture: Federation, PAM, and Residual Risk

Stability in OT identity infrastructure reflects familiar OT constraints: long asset lifecycles, limited change capacity, and operational accountability that prioritizes continuity and diagnosability over policy compliance.

While modern identity architectures emphasize federation, centralized authentication, and dynamic authorization models, production OT environments impose structural limits on how far these models can be adopted without introducing new classes of risk. Identity controls in OT must remain viable over decades, operate under partial isolation, and degrade safely under failure conditions.

This note examines common OT identity patterns, the risks they redistribute, and why no single model fully resolves the underlying tensions.

## Identity as an Architectural Constraint in OT

In IT environments, identity systems are treated as continuously available, centrally governed services. They are assumed to be reachable, time-synchronized, and administratively coherent. Failures are disruptive but rarely safety-relevant.

In OT environments, identity infrastructure is subject to additional constraints:

* Connectivity may be intermittent or deliberately restricted
* Time synchronization cannot be assumed as a universal invariant
* Administrative authority is distributed across organizational boundaries
* Recovery from failure must be possible without external dependencies

These constraints shape which identity patterns can be sustained in practice.

## Common OT Identity Patterns

### Option A: Fully Isolated Local Identities

In this model, control systems authenticate users locally. Accounts are defined and managed per system or per site, often using local directories or embedded authentication mechanisms.

**Strengths:**

* No dependency on external identity providers
* Predictable failure modes
* High resilience under isolation

**Limitations:**

* Poor scalability across sites or systems
* Inconsistent access governance
* Difficult to audit centrally

**Observed degradation patterns:**

Without sustained investment in local identity management, this model predictably degrades:

* Weak or reused passwords due to lack of centralized policy enforcement
* Delayed deprovisioning after role changes or termination
* Credentials written down or shared to facilitate operational handovers
* Orphaned accounts with unclear ownership
* Break-glass credentials that become routine access methods

This model prioritizes availability and diagnosability but struggles to meet modern governance expectations. The operational burden falls entirely on OT teams, who are rarely resourced for sustained identity lifecycle management.

### Option B: Centralized Identity via Forest Trust or Shared Domain

Here, OT systems rely on a central directory, typically through domain trust or shared forest membership.

**Strengths:**

* Centralized policy enforcement
* Unified account lifecycle management
* Improved auditability

**Limitations:**

* Expands trust boundaries deeply into OT through infrastructure-level dependencies
* Couples OT availability to IT directory health
* Introduces systemic blast radius through Kerberos, NTLM, and directory replication

**Infrastructure-level trust implications:**

Forest trusts create dependencies beyond authentication:

* Kerberos tickets and delegation traverse trust boundaries
* NTLM relay attacks can propagate across forests
* Group Policy can be applied across trust relationships
* Lateral movement tooling designed for Active Directory works across the trust
* Directory replication and RPC traffic flows between IT and OT

If the IT forest is compromised, attackers gain implicit paths into OT through the trust infrastructure itself, not just through individual credentials.

This pattern improves administrative efficiency but concentrates failure risk in the identity plane and expands the attack surface from compromised IT infrastructure into OT.

### Option C: Federated Identity with Privileged Access Management

This model separates routine user identity from privileged operational access.

Federation is used for user-level functions such as read-only process monitoring, historian queries, reporting portals, analytics platforms, or user preference and personalization settings.

Privileged operations, including engineering changes, system administration, and high-consequence control actions, are mediated through PAM systems, jump servers, or locally authenticated mechanisms. Federation may assert identity, but elevation and execution of privileged actions require an independent control layer.

In high-risk environments, operator access may remain entirely non-federated.

**What federation means:**

Federation uses cryptographically signed tokens instead of directory trust or shared infrastructure.

When a user authenticates:

1. IT IdP (Identity Provider) validates credentials and issues a signed token
2. User presents token to OT system
3. OT system validates the signature and claims without contacting the IdP
4. No Kerberos, NTLM, or directory replication required

The OT environment trusts the signature, not the infrastructure.

**Strengths:**

* Centralized lifecycle management without infrastructure-level trust
* Protocol-level authentication instead of Kerberos or NTLM dependencies
* Improved credential hygiene through centralized MFA and policy enforcement
* Near-instant revocation capability for federated access

**Limitations:**

* Introduces new single points of failure
* Requires sustained operational investment in token management and key rotation
* Depends on downstream scope enforcement
* Limited applicability in brownfield environments with legacy applications

This is a hybrid model that attempts to balance governance with operational containment.

## What Federation Solves

Before examining what federation introduces, it is worth acknowledging what it addresses.

**Credential hygiene:**

Federation eliminates the need for users to remember and enter passwords for routine OT access. This reduces:

* Password reuse across IT and OT
* Weak passwords chosen for memorability
* Credentials written on notes or stored insecurely
* Shared accounts for operational convenience

**Lifecycle management:**

Centralized identity enables:

* Immediate revocation when employment ends
* Consistent deprovisioning across systems
* Centralized MFA enforcement
* Unified audit trails across IT and OT access

**Reduced infrastructure-level dependencies:**

Compared to forest trusts, federation does not create dependencies on Kerberos, NTLM, or directory replication. The OT environment validates cryptographically signed tokens without relying on shared infrastructure with IT.

These are real operational and security improvements.

## Scoping Federation: Where the Boundary Goes

Option C raises an immediate question: where does federation end and local authentication begin?

### Federation Does Not Have to Mean Operator Control

A common assumption is that identity federation must extend all the way to operator access. In high-consequence OT environments, this is neither necessary nor desirable.

Federated identity can be deliberately constrained to:

* Read-only process monitoring and supervisory views
* Historian queries and reporting systems
* Analytics platforms and dashboards
* Engineering workstations accessing documentation or read-only views
* User preference management and personalization settings

Federation can confirm who a person is. It does not, by itself, determine whether that person should be able to place the process into a different state. High-consequence actions, including operator control, engineering changes, and system administration, require controls that account for situational awareness, approval paths, and operational responsibility.

Where federated operator control is unacceptable, local or system-bound authentication can be retained for control actions, while federation supports visibility and governance elsewhere.

This is an intentional boundary reflecting asymmetric consequence.

## Third-Party and Vendor Access: Where Authority Must Remain Local

Production OT relies heavily on external parties. Equipment vendors, integrators, and specialist maintenance providers frequently perform work that site personnel cannot substitute.

Their access is not exceptional. It is routine and often urgent.

In many organizations, the volume of privileged sessions initiated by third parties exceeds that of internal users. This makes contractor and vendor activity one of the most significant operational attack surfaces in OT.

External connectivity, remote transport, and perimeter security are commonly delivered by IT. This is appropriate. However, authentication to the network is not equivalent to authorization to interact with the process.

The responsibility for consequences remains with the site.

Federation can identify the individual and IT can provide the connection, but neither can determine whether the requested action is safe, timely, or permissible within current plant conditions.

For this reason, Privileged Access Management under OT authority is the critical control layer.

PAM provides:

* Local approval and supervision
* Context-aware authorization
* Session control and monitoring
* The ability to interrupt or terminate unsafe activity
* A mechanism for emergency access that still preserves accountability

Most importantly, it ensures that the organization operating the process retains decision authority over who performs high-consequence actions.

### Asymmetric Dependency

Vendors may authenticate through systems they control. They may arrive under contractual obligations and time pressure. During outages, there is strong incentive to accelerate access.

Under these conditions, governance fails unless the authority to permit or deny action resides with OT.

This is not distrust of vendors. It is recognition that accountability for outcome cannot be outsourced.

### What This Means Architecturally

Even in highly federated environments:

* Third-party access typically terminates in PAM or controlled jump environments
* Elevation is granted locally
* Execution authority remains with OT
* Emergency bypass mechanisms are designed and owned by the site

Without this boundary, responsibility and authority separate. Over time, that separation produces unmanaged risk.

## Residual Risk and Risk Redistribution

Each identity pattern redistributes risk rather than eliminating it.

**Option A concentrates risk in:**

* Credential management discipline at the site level
* Manual lifecycle management processes
* Individual account compromise

**Option B concentrates risk in:**

* IT directory infrastructure availability and integrity
* Infrastructure-level attack paths
* Systemic lateral movement

**Option C concentrates risk in:**

* IdP availability and integrity
* Downstream scope enforcement
* Token lifecycle and key rotation
* Time synchronization in isolated zones

### Identity Provider Compromise

A compromised IdP can issue valid tokens at scale. If federation is restricted to visibility functions, compromise grants observation but not control authority.

This is a different exposure than forest trust, where compromise may create deep infrastructure paths into OT.

### Downstream Scope Enforcement Fragility

Federation assumes applications correctly enforce scopes and claims.

If one critical system fails to do so, containment assumptions can collapse. This fragility is systemic.

### Governance Decay

All models rely on sustained discipline. Over time, exceptions accumulate and temporary measures become permanent.

The difference is where the burden falls:

* Option A on the site
* Option B on central IT
* Option C split between central identity and OT-controlled privilege

This is an emergent property of long-lived environments.

## What Shifts, and What Does Not

**Option A:**

* All burden on OT
* Maximum control, maximum workload

**Option B:**

* Lifecycle shifts to IT
* OT retains responsibility but loses identity authority

**Option C:**

* Routine identity central
* Privilege and consequence local
* Additional dependencies introduced

Regardless of model, OT must be able to operate safely when external systems fail.

## This Is Not a Solvable Problem

No identity architecture removes the tension between governance and resilience.

The goal is to choose where risk resides, make it visible, and ensure failure modes are survivable.

## Implications for Identity Design in OT

* Assume degradation over time
* Do not require federation for operator or engineering control
* Keep authority for process-changing actions local
* Understand infrastructure implications of forest trust
* Treat time and IdP availability as dependencies
* Design explicit fallback paths
* Expect governance erosion
* Prefer architectures that fail noisy rather than silent

Identity in OT is not about elegance. It is about sustaining operational authority under stress for decades.
