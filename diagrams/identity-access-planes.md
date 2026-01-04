```mermaid
graph TD
    subgraph IT_Domain [IT Identity Domain]
        IDP[Identity Provider - OIDC]
        JWKS[Signing Keys - JWKS]
    end

    subgraph OT_Domain [OT Security Boundary]
        subgraph OP_Plane [Operational Access Plane]
            Client[User or Client Application]
            KeyCache[Local JWKS Cache - Public Keys]
            TokenValidate[JWT Validation - Offline Capable]
            Apps[OT Apps and Historians]
        end

        subgraph PRIV_Plane [Privileged Access Plane]
            PAM[PAM Solution]
            MFA[Phishing-resistant MFA - FIDO2]
            Servers[OT Infrastructure and Workstations]
        end

        subgraph RES_Plane [Resilience and Break-glass Plane]
            Trigger[Dark IT Trigger - Defined Conditions]
            LocalAD[Isolated Local AD and Local Accounts]
        end
    end

    %% Operational Flow (Claim Trust)
    JWKS --> KeyCache
    IDP -- "Issues signed JWT" --> Client
    Client -- "Presents JWT (claim trust, no Kerberos or RPC)" --> TokenValidate
    KeyCache --> TokenValidate
    TokenValidate -- "Authorize via scopes and roles" --> Apps

    %% Privileged Flow (Mediated Admin)
    IDP -- "Federated login" --> PAM
    MFA --> PAM
    PAM -- "Brokered session via OT technical account (no password disclosure)" --> Servers

    %% Resilience Flow (Exceptional Only)
    Trigger --> LocalAD
    LocalAD -. "Emergency access only (reviewed and controlled)" .-> Servers

    %% Styling
    style RES_Plane fill:#fff3e0,stroke:#ff9800,stroke-dasharray: 5 5
    style PRIV_Plane fill:#e8f5e9,stroke:#4caf50
    style OP_Plane fill:#e3f2fd,stroke:#2196f3

```
