## The Standard Validation Chain

In connected environments, certificate validation assumes that all external dependencies are reachable in near real-time.

In OT environments, these dependencies are often intentionally blocked. The validation logic must then decide how to handle failure, with outcomes that differ significantly between runtime operation and change events.

```mermaid
flowchart TD
    subgraph Isolated_Zone [Isolated OT Network]
        OT[OT System / Application] --> VAL[Certificate Validation Logic]

        subgraph Dependencies [Validation Attempts]
            DNS[DNS Resolution]
            CRL[Revocation Check / OCSP]
        end

        VAL --> DNS
        VAL --> CRL
    end

    subgraph Perimeter [Network Boundary]
        FW{Network Controls}
    end

    subgraph External [External Services]
        EXT[Public DNS & CA Services]
    end

    %% Links
    DNS --> FW
    CRL --> FW
    FW --x |"Blocked by Design"| EXT
    FW -.-> |"Timeout / Failure Signal"| VAL

    %% Outcome 1: Soft Fail
    VAL -.- |"Soft Fail (Runtime): Log & Proceed"| OT
    
    %% Outcome 2: Hard Fail
    VAL --x |"Hard Fail (Change): BLOCK Operation"| OT

    %% Styling
    style EXT fill:#fdd,stroke:#f66
    
    %% Red for the block at the firewall (Link index 5)
    linkStyle 5 stroke:red,stroke-width:3px
    
    %% Orange for the feedback loop (Link index 6)
    linkStyle 6 stroke:orange,stroke-width:2px,stroke-dasharray: 5 5

    %% Orange for Soft Fail outcome (Link index 7)
    linkStyle 7 stroke:orange,stroke-width:2px,stroke-dasharray: 5 5

    %% Red for Hard Fail outcome (Link index 8)
    linkStyle 8 stroke:red,stroke-width:3px
```
