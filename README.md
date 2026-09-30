# 🛡️ CHAIN SENTINEL
**Automated Attribution of Unknown Cryptocurrency Wallets to Nearest VASPs**
*(Smart India Hackathon 2026 | Problem Statement: SIH26182 | Team: CODE GENIUS)*

---

### 🚀 Quick Links
- **Live Demo Environment:** `[Yahan Apna Live Demo Link Daal]`
- **90-Second Pitch Video:** `[Yahan Apna YouTube Link Daal]`

---

## 📌 Overview
CHAIN SENTINEL is a calibrated, human-approved wallet-to-VASP attribution engine built for SAHYOG. It traces suspect crypto wallets forward to the nearest exchange or custodian using deposit-sweep inference and on-chain heuristics. What normally takes manual investigators hours or days is compressed into minutes, providing a ranked, highly confident lead.

## ✨ Key Innovations
- **Speed & Precision:** Delivers a ranked VASP lead with calibrated confidence scores and counterfactual evidence.
- **Human-in-the-Loop:** Mandatory human reviewer approval; the system generates draft notices but never auto-sends them.
- **Sovereign by Design:** Built for self-hosted or air-gapped environments, ensuring zero address leakage to foreign APIs.
- **Label Flywheel:** Every VASP reply via SAHYOG upgrades the address label to a Level 5 Confirmed status.

## 🏗️ System Architecture
The platform is built on a 5-layer architecture ensuring speed, privacy, and accuracy.

```mermaid
flowchart TB
  subgraph USERS["Users"]
    INV["Investigator"]
    REV["Reviewer"]
  end

  subgraph SAH["SAHYOG Integration (mock adapter, not the live portal)"]
    SIN["Case and wallet intake"]
    SDR["Draft notice service"]
    SRS["VASP reply capture"]
  end

  subgraph UI["Presentation Layer"]
    DASH["React Dashboard"]
    GRAPH["Force Graph 2D/3D"]
    REP["Report Viewer"]
  end

  subgraph APP["Application Layer"]
    API["Attribution API - FastAPI"]
    CASE["Case and Workflow Service"]
    WORK["Celery Workers"]
    REDIS[("Redis queue and cache")]
  end

  subgraph CORE["Intelligence Core"]
    TRV["Traversal Engine - best-first"]
    HEU["Heuristics H1 to H7"]
    SWP["Deposit-sweep inference"]
    PEEL["Peel-chain and CoinJoin detector"]
    SCO["Scoring and calibration"]
    EXP["Explain and counterfactual"]
    RISK["Risk and typology"]
  end

  subgraph DATA["Data Layer"]
    PG[("PostgreSQL: labels, VASP directory, cases, audit")]
    NEO[("Neo4j: investigation graph")]
    EVI[("Evidence store: SHA-256 sealed")]
  end

  subgraph ADP["Chain Adapters"]
    BTC["Bitcoin"]
    EVM["EVM: ETH, BNB, Polygon"]
    TRX["Tron: USDT TRC-20"]
    SOL["Solana"]
    DEMO["Demo fixture provider"]
  end

  subgraph SRC["Chain Data Sources"]
    PUB["Public APIs: Esplora, Etherscan V2, TronGrid, Solana RPC"]
    SELF["Self-hosted nodes and indexers (sovereign mode)"]
  end

  INV --> DASH
  REV --> DASH
  DASH --> API
  GRAPH --- DASH
  REP --- DASH

  SIN --> CASE
  CASE --> SDR
  SDR -->|"reviewer-approved request"| SRS
  SRS -->|"VASP confirms: label upgraded to L5"| PG

  API --> CASE
  API --> REDIS
  REDIS --> WORK
  WORK --> TRV
  TRV --> HEU
  TRV --> SWP
  TRV --> PEEL
  HEU --> SCO
  SWP --> SCO
  PEEL --> RISK
  SCO --> EXP
  RISK --> EXP
  EXP --> EVI

  TRV --> BTC
  TRV --> EVM
  TRV --> TRX
  TRV --> SOL
  TRV --> DEMO
  BTC --> PUB
  EVM --> PUB
  TRX --> PUB
  SOL --> PUB
  BTC -.-> SELF
  TRX -.-> SELF

  HEU --> NEO
  TRV --> NEO
  API --> PG

  classDef suspect fill:#7f1d1d,stroke:#ef4444,color:#fff
  classDef core fill:#312e81,stroke:#a78bfa,color:#fff
  classDef data fill:#78350f,stroke:#f59e0b,color:#fff
  class TRV,HEU,SWP,PEEL,SCO,EXP,RISK core
  class PG,NEO,EVI data


⚙️ Attribution Decision Logic
flowchart TD
  A["Suspect wallet from SAHYOG case"] --> B["Detect chain(s) and fetch transfers"]
  B --> C{"Address labelled as VASP hot or deposit wallet?"}
  C -->|"Yes"| H["Terminal hit: candidate VASP"]
  C -->|"No"| D{"Single-use address that sweeps into a labelled hot wallet within window?"}
  D -->|"Yes"| E["Deposit-sweep inference: attribute to that VASP"]
  D -->|"No"| F{"Pass-through hub, mixer or bridge?"}
  F -->|"Pass-through hub or DEX router"| G["Continue trace through hub"]
  F -->|"CoinJoin or mixer"| M["Breakpoint: stop, low-confidence post-mix leads only"]
  F -->|"Bridge or swapper"| N["Continuation edge on other chain, confidence penalty"]
  F -->|"None"| I["Expand outflows: haircut flow share, drop dust"]
  G --> I
  N --> I
  I --> J{"Hop limit or min-share reached?"}
  J -->|"No"| C
  J -->|"Yes"| K["NO_ATTRIBUTION or UNKNOWN_CUSTODIAL"]
  H --> S["Score: label level, sweep, gas-funder, flow share, hop penalty, staleness"]
  E --> S
  M --> S
  K --> S
  S --> T["Calibrated posterior and band: High, Medium, Low"]
  T --> U["Explain and counterfactual"]
  U --> V["Hash-sealed report and draft notice"]
  V --> W{"Human reviewer approves?"}
  W -->|"Yes"| X["Send via SAHYOG channel"]
  W -->|"No"| Y["Back to investigator with reasons"]

💻 Tech Stack
Backend: Python 3.12, FastAPI, Celery, Redis
Data & Graph: PostgreSQL, Neo4j, NetworkX
Intelligence: Heuristics (H1-H7), Logistic Regression (Scikit-learn)
Frontend: React 18, Tailwind CSS, react-force-graph
Infrastructure: Docker Compose, GitHub Actions
(Note: SAHYOG integrations are handled via a mock adapter until official API specifications are public).

⚠️ Disclaimer
Attribution = investigative lead, not proof of ownership.
This engine provides high-confidence probabilities and ranked leads to empower state cyber police and investigators. It does not auto-execute legal actions or freeze funds without manual verification.
