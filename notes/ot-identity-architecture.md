# OT Identity Architecture: Federation, PAM, and Residual Risk

Stability in OT identity infrastructure reflects familiar OT constraints: long asset lifecycles, limited change capacity, and operational accountability that prioritizes continuity and diagnosability over policy compliance.

While modern identity architectures emphasize federation, centralized authentication, and dynamic authorization models, production OT environments impose structural limits on how far these models can be adopted without introducing new classes of risk. Identity controls in OT must remain viable over decades, operate under partial isolation, and degrade safely under failure conditions.

This note examines common OT identity patterns, the risks they redistribute, and why no single model fully resolves the underlying tensions.
It first compares three identity models, then examines federation scope, third-party authority, and residual risk.

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

* Weak or reused passwords
* Delayed deprovisioning
* Credentials written down or shared
* Orphaned accounts
* Break-glass access becoming routine

This model prioritizes availability and diagnosability but struggles to meet modern governance expectations.

### Option B: Centralized Identity via Forest Trust or Shared Domain

Here, OT systems rely on a central directory, typically through domain trust or shared forest membership.
**In practice, this is the most common pattern in brownfield environments, often inherited from earlier infrastructure decisions rather than deliberately selected as an identity architecture.**

**Strengths:**

* Centralized policy enforcement
* Unified lifecycle management
* Improved auditability

**Limitations:**

* Expands trust boundaries deeply into OT
* Couples OT availability to IT directory health
* Introduces systemic blast radius

**Infrastructure-level implications:**

* Kerberos and delegation cross boundaries
* NTLM relay paths propagate
* Group Policy may traverse trusts
* AD-native lateral movement tooling remains effective
* Replication and RPC create standing connectivity

If IT is compromised, pathways into OT exist by design.

### Option C: Federated Identity with Privileged Access Management

This model separates routine identity from high-consequence authority.

Federation supports visibility, reporting, and low-risk interaction. Privilege is mediated through PAM, jump hosts, or local controls. In some environments, operator control remains entirely non-federated.

**What federation means:**

Authentication is based on cryptographically signed assertions rather than shared directory infrastructure. OT validates the signature, not the provider runtime.

**Strengths:**

* Central lifecycle control
* MFA enforcement
* No Kerberos or replication dependency
* Fast revocation

**Limitations:**

* New dependency on IdP integrity
* Requires durable token and key management
* Dependent on downstream enforcement
* Limited applicability in brownfield environments where legacy applications lack token support or cannot be modified within vendor validation constraints

This is a balancing model, not a perfect one.

## What Federation Solves

Federation addresses real operational and security problems that other models leave unresolved.

**Credential hygiene:**

* Reduces password reuse
* Reduces weak memorized credentials
* Limits note-based storage
* Reduces shared identities

**Lifecycle management:**

* Rapid revocation
* Consistent deprovisioning
* Central MFA
* Unified audit

**Reduced infrastructure coupling:**

Unlike forest trust, OT does not depend on Kerberos or replication.

These are significant operational improvements.

## Scoping Federation: Where the Boundary Goes

Federation can assert identity without granting process authority.

It can reasonably support:

* Read-only views
* Historians
* Analytics
* Documentation access
* Personalization

Actions that change plant state require additional layers reflecting accountability, context, and risk. Where consequence is asymmetric, authority remains local.

## Third-Party and Vendor Access: Where Authority Must Remain Local

External parties perform a large portion of privileged activity in OT.

Connectivity is often delivered by IT, but responsibility for outcome remains with the site. Federation can identify the individual, but it cannot determine whether the action is safe.

PAM becomes the authority boundary.

**PAM provides:**

* Local approval
* Context-aware authorization
* Session control
* Ability to terminate unsafe activity
* Emergency mechanisms with accountability

These capabilities require someone present and attentive. Active supervision is common during commissioning and exceptional during routine maintenance. The resulting gap follows the same ownership dynamics seen in infrastructure monitoring.

Without local authority, responsibility and control diverge. Risk accumulates.

## Residual Risk and Risk Redistribution

Each identity pattern redistributes risk rather than eliminating it.

| Risk dimension        | Option A (Local) | Option B (Forest Trust) | Option C (Federation + PAM)         |
| --------------------- | ---------------- | ----------------------- | ----------------------------------- |
| Primary concentration | Site discipline  | IT directory integrity  | IdP integrity and scope enforcement |
| Blast radius          | Local            | Cross-boundary systemic | Contained if scoped                 |
| Governance burden     | Entirely site    | Split                   | Split differently                   |
| Typical failure       | Credential decay | Lateral movement        | Claim misuse                        |
| Long-term drift       | Predictable      | Silent and wide         | Dependent on enforcement            |

### Identity Provider Compromise

A compromised IdP can mint tokens at scale. If federation is limited to visibility, exposure differs from infrastructure trust compromise.

### Downstream Enforcement Fragility

If one system mishandles claims, containment collapses.

### Governance Decay

All models degrade.

This follows the pattern explored in [Silent Degradation under IT/OT Convergence](./silent-degradation-under-convergence.md): controls that depend on cross-boundary coordination erode under operational pressure.

## What Shifts, and What Does Not

**Option A:** All burden remains local. Maximum autonomy, maximum workload, maximum governance fragility.

**Option B:** Identity management shifts to central IT. OT retains operational accountability but cedes identity authority. The blast radius of IT compromise expands into OT by design.

**Option C:** Routine identity governance shifts centrally. Privilege elevation and consequence management remain local. New operational dependencies (IdP availability, time synchronization, key rotation) are introduced.

In every model, OT must retain the ability to operate safely when external systems fail. This is not a preference—it is a safety requirement.

## This Is Not a Solvable Problem

No identity architecture eliminates the tension between centralized governance and operational resilience. Each model trades visibility for autonomy, efficiency for blast radius, or manageability for dependency.

The goal is not to find the correct model. It is to choose where residual risk resides, ensure it is visible, and verify that failure modes are survivable by the teams who will live with them.

## Implications for Identity Design in OT

* Plan for degradation over operational lifecycles
* Do not require federation for operator control or engineering changes to process state
* Keep authority for high-consequence actions under OT governance
* Understand infrastructure-level implications of directory trust
* Treat time synchronization and IdP availability as operational dependencies, not assumptions
* Design explicit fallback paths for identity system failure
* Expect governance erosion and design containment accordingly
* Prefer architectures that fail visibly rather than silently

## Closing

Identity in OT is not about elegance. It is about sustaining operational authority under stress for decades, and knowing where that authority will fail first.
If you want, I can also create a **visually scannable diagram** showing Options A/B/C with risks and governance boundaries—this makes it much easier for managers or engineers to digest quickly. Do you want me to do that next?
