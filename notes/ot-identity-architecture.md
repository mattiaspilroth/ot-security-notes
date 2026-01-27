# OT Identity Architecture: Federation, PAM, and Resilience

## Purpose and Scope

This note describes an identity architecture that reduces the operational burden of managing separate OT identity infrastructure while maintaining security boundaries and operational continuity.

The patterns described here address a common problem: organizations create separate Active Directory forests for OT environments as a security measure, but then struggle with the operational reality of managing isolated identity infrastructure. This typically results in:
- Credential sprawl and weak password practices
- Delayed or incomplete lifecycle management (joiners, leavers, role changes)
- Tension between security policy and operational usability

This is not a universal implementation guide. The technical patterns and access plane model are provided to support architectural decision-making in environments where:
- OT availability and safety requirements constrain authentication dependencies
- Regulatory frameworks require privilege segregation and audit logging
- Legacy systems coexist with modern federated identity capabilities
- Break-glass access must remain viable under degraded conditions

The approach is based on replacing infrastructure-level trust with protocol-level trust using OIDC and OAuth 2.0, combined with explicit privilege elevation controls and documented resilience mechanisms. The objective is to achieve strong security posture while maintaining manageable operational workload for OT organizations.

## The Problem: Operational Burden of Isolated Identity

Traditional guidance recommends separate Active Directory forests for IT and OT. This boundary is architecturally sound but creates operational challenges.

A separate OT forest requires:
- Dedicated expertise in Active Directory administration
- Consistent lifecycle management (provisioning, deprovisioning, role changes)
- Password policy enforcement and credential rotation
- Group Policy maintenance and security patching
- Break-glass procedure maintenance and testing

In practice, OT organizations rarely have dedicated identity management resources. The separate forest becomes passively managed, leading to:

- Weak or reused passwords due to lack of enforcement
- Delayed user deprovisioning after role changes or termination
- Orphaned accounts and unclear privilege assignments
- Emergency accounts that become permanent workarounds

Organizations facing this operational burden sometimes introduce forest trusts or other authentication mechanisms between IT and OT to regain usability and reduce management overhead. While this addresses the immediate operational problem, it reintroduces infrastructure-level trust where authentication and authorization depend on Kerberos, NTLM, RPC, and directory relationships.

If the IT forest is compromised, attackers may leverage:
- Kerberos delegation and ticket abuse
- NTLM relay and credential replay
- Group Policy manipulation
- Automated lateral movement tooling designed for Active Directory

The result is degraded security through either passive management or reintroduced trust dependencies, undermining the boundary the separate forest was intended to create.

---

## The Shift: Protocol and Trust Model Change (OIDC / OAuth 2.0)

OIDC and OAuth 2.0 replace infrastructure-level trust with protocol-level trust.

Instead of relying on Kerberos or NTLM, authentication and authorization are based on cryptographically signed JSON Web Tokens (JWTs).

Instead of asking:  
“Is this user or machine part of my directory?”

The OT environment asks:  
“Is this a valid, signed token issued by a trusted Identity Provider?”

### Security Implications

- **Protocol replacement**
  NTLM and Kerberos are replaced by JWTs signed using asymmetric cryptography.

- **Claim-based trust**  
  Trust is established through token signatures and claims, not shared infrastructure.

- **No implicit lateral trust**  
  There is no RPC, directory replication, or Kerberos dependency between IT and OT.

This protocol shift is the primary reason for the reduced attack surface.

---

## Access Plane Model

OT identity is best understood as three distinct access planes, each with a different risk profile.

(See conceptual diagram: [OT Identity Access Planes and Trust Boundaries](../diagrams/identity-access-planes.md))

---

## 1. Operational Access Plane (Default)

This plane covers the majority of OT access use cases.

Typical users and systems:
- Operators
- Engineers with read or limited write access
- Monitoring and visualization systems
- Historians and analytics platforms
- Vendor support in non-privileged roles

Characteristics:
- Federated authentication via OIDC / OAuth 2.0 where operationally feasible
- Claim-based authorization using scopes or roles
- No standing administrative privileges on OT operating systems
- No directory-level trust between IT and OT

### Offline Validation and Availability

To meet OT availability requirements:

- OT systems validate JWTs using the Identity Provider’s public keys
- Public signing keys are cached locally in the OT environment
- Short IT network interruptions do not immediately disrupt operator access

This ensures operational continuity without weakening security.

For continuously staffed operator environments, federated identity must govern session establishment and authorization, not the real-time continuity of an operator’s interaction with a running process. Once authenticated, operator sessions must remain valid across short-lived outages of identity infrastructure or network connectivity.

Re-authentication should occur only on session termination, privilege elevation, or explicit lock conditions, ensuring that identity service disruptions cannot inadvertently alter operator ability during normal operation.

### Time Dependency and Local Validation

JWT validation relies on accurate time to evaluate token validity. In isolated or intermittently connected OT environments, unmanaged clock drift can result in legitimate access being denied.

To preserve resilience:
- OT environments must maintain a reliable local time source
- Time synchronization must not depend on continuous connectivity to IT or external services
- Acceptable clock skew should be explicitly defined and tested

---

## 2. Privileged Access Plane (Controlled)

Administrative actions represent a higher risk profile and require explicit elevation.

Examples:
- OT server and workstation administration
- Active Directory, PKI, or firewall configuration
- System-level application changes
- Emergency maintenance activities

Characteristics:
- Access mediated through a PAM solution or equivalent control
- Privileges are:
  - Just-in-time
  - Time-limited
  - Session-recorded
  - Explicitly approved where applicable
- No standing administrator accounts tied directly to personal IT identities

Administrative access is primarily remote, mediated through PAM from IT networks. Direct physical access to OT systems for maintenance or emergency scenarios falls under the Resilience Plane, not this access model.

### Administrative Footprint Control

In PAM-based models:

- Users authenticate to PAM using federated identity and strong MFA
- PAM maps users to privileged technical accounts in OT
- Users never know or handle the passwords of OT administrative accounts
- Privileged credentials never leave the PAM boundary

This prevents credential harvesting even if a user’s IT workstation is compromised.

---

## OIDC and PAM Integration Patterns

OIDC remains the primary identity source, while PAM or MFA enforces privilege elevation discipline.

### Pattern A: OIDC + PAM with Technical Accounts

- User authenticates to PAM using OIDC with hardware-backed MFA
- PAM brokers access using dedicated OT technical accounts
- Credentials are isolated, rotated, and hidden from users
- Suitable for legacy OT platforms and Windows-based environments

---

### Pattern B: OIDC with Strong MFA for Privileged Scopes

- OIDC tokens are accepted directly by OT systems that support modern authentication
- Privileged scopes require:
  - Explicit re-authentication
  - Hardware-backed, phishing-resistant MFA such as FIDO2
- Suitable for modern OT applications, APIs, and engineering tools

Push notifications or SMS-based MFA are insufficient for this risk profile.

---

### Pattern Selection and Hybrid Approaches

Pattern selection depends on:
- OT system authentication capabilities
- Organizational PAM maturity
- Legacy system constraints
- Vendor support boundaries

Many environments implement hybrid approaches where:
- Modern OT applications use Pattern B (direct OIDC with strong MFA)
- Legacy Windows systems use Pattern A (PAM with technical accounts)
- Isolated safety systems may require neither pattern

The architectural objective is to minimize infrastructure-level trust and 
credential exposure, not to achieve uniform implementation across all systems.

---

## 3. Resilience and Break-Glass Plane (Exceptional)

OT environments require a fallback mechanism when federated identity or PAM dependencies are unavailable.

Characteristics:
- Local OT Active Directory accounts or local system accounts
- Fully isolated from IT and external identity providers
- Access restricted to predefined emergency scenarios
- Credentials stored securely and tested regularly
- Mandatory post-event review and audit

### Formal Trigger Conditions

The transition to the Resilience Plane must be governed by clearly defined triggers, such as:
- Loss of federated identity services beyond defined thresholds
- Cyber incident response scenarios
- Safety-critical operational continuity events

This prevents resilience mechanisms from being used for convenience or performance issues.

---

### Resilience-Dominant Environments

In highly isolated OT environments or where safety requirements prohibit dependency on external identity services, local OT identity may play a dominant role, with federation used selectively where feasible.

This represents an explicit architectural trade-off where availability and safety outweigh the benefits of centralized identity services.

---

## Identity Lifecycle and Termination Risk

Federated identity enables near-instant revocation of access in the Operational and Privileged Access Planes.

The Resilience Plane introduces a controlled risk:
- Local OT accounts may remain active after employment termination in IT

This risk should be addressed through context-appropriate measures such as:
- Regular review of local OT accounts
- Named, non-shared break-glass identities
- Sealed credential storage and access logging
- Mandatory reconciliation after incident use

This residual risk is an explicit design trade-off: limited, auditable identity persistence is accepted to preserve safety and operational continuity under degraded conditions.

---

## Privileged Access and MFA Requirements

Privileged actions in OT environments should enforce strong authentication 
as the default control.

Requirements:
- Hardware-backed, phishing-resistant MFA
- Local validation within the OT environment
- No dependency on cloud-only MFA services for safety-critical access

This prevents credential replay, phishing-based escalation, and trust abuse.

Where technical or safety constraints prevent implementation of hardware-backed MFA, compensating controls and explicit risk acceptance are required.

---

## Trust Anchor and Root of Trust

This architecture distinguishes between:

- **Trust Anchor for daily operations**  
  The IT Identity Provider acts as the trust anchor for federated identity and authorization.

- **Root of Trust for safety and availability**  
  The local OT environment remains the ultimate root of trust through isolated identity, access, and control mechanisms.

This distinction is critical for regulatory, safety, and resilience assessments.

---

## Scope and Limitations

This model addresses:
- User and service identity
- Authorization and privilege elevation
- Trust boundaries between IT and OT

It does not replace:
- Endpoint hardening
- Network segmentation
- Application-level authorization logic
- Secure engineering of real-time control systems

---

## Summary

This reference architecture demonstrates how separating operational access, 
privileged access, and resilience mechanisms enables OT environments to:

- Reduce or eliminate infrastructure-level trust between IT and OT
- Replace legacy authentication protocols with cryptographically verifiable 
  tokens where technically feasible
- Minimize lateral movement and credential exposure
- Preserve operational continuity under degraded conditions  
- Improve auditability and governance alignment

Implementation will vary based on technical constraints, safety requirements, 
and organizational maturity. The architectural principles provide a framework 
for reasoning about identity and access trade-offs rather than a universal 
implementation checklist.
