# Data Processing Flow Diagram

```mermaid
graph TD
    subgraph "ImageRPS / TMS (CDS 2.9/TAP 3.1)"
        A[Generate Batch & Intraday Files<br/>M–Sa, Su]
        A --> B{Extensions}
        B -->|Completed| C["Completed"]
        B -->|Failed| D["Failed"]
        B -->|Parallel Completed| E["PTC31"]
        B -->|Parallel Failed| F["PTF31"]
    end

    subgraph "TAP 3.1 (Servers 34–38)"
        G[Receives files from ImageRPS/TMS]
        G --> H{Route based on extension}
        H -->|Completed / Failed| I[CDS 2.9<br/>Current State Path]
        H -->|PTC31 / PTF31| J[CDS 3.5<br/>Parallel Path]
    end

    subgraph "CDS 2.9"
        K[Existing production path<br/>No changes]
    end

    subgraph "CDS 3.5"
        L[Polls for PTC31 / PTF31]
        L --> M{Process}
        M -->|Completed| N["_CC35"]
        M -->|Failed| O["_FC35"]
        N --> P[TAP 3.5]
        O --> P
    end

    subgraph "TAP 3.5"
        Q[Polls for _CC35 / _FC35]
        Q --> R{Process}
        R -->|Completed| S["_Completed"]
        R -->|Failed| T["_Failed"]
        S --> U[End of Flow]
        T --> U
    end

    C --> I
    D --> I
    E --> J
    F --> J
    J --> L

    classDef imageRPS fill:#e1f5fe,stroke:#0277bd,stroke-width:2px,color:#000;
    classDef tap31 fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px,color:#000;
    classDef cds29 fill:#e8f5e8,stroke:#2e7d32,stroke-width:2px,color:#000;
    classDef cds35 fill:#fff3e0,stroke:#ef6c00,stroke-width:2px,color:#000;
    classDef tap35 fill:#fce4ec,stroke:#c2185b,stroke-width:2px,color:#000;

    class A,B,C,D,E,F imageRPS;
    class G,H,I,J tap31;
    class K cds29;
    class L,M,N,O,P cds35;
    class Q,R,S,T,U tap35;
```# lockbox
