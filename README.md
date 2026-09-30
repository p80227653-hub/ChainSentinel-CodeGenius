# 🛡️ CHAIN SENTINEL
**Automated Attribution of Unknown Cryptocurrency Wallets to Nearest VASPs**
*(Smart India Hackathon 2026 | Problem Statement: SIH26182 | Team: CODE GENIUS)*

---

### 🚀 Quick Links
- **Live Demo Environment:** `[https://chain-sentinel-demo-five.vercel.app/]`
- **90-Second Pitch Video:** `[https://youtube.com/playlist?list=PLP7P7qwHHv4s&si=AqvL27T8mLIx6JHX]`

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

  subgraph SAH["SAHYOG Integration"]
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
    REDIS["Redis queue and cache"]
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
    PG["PostgreSQL: labels, VASP directory, cases"]
    NEO["Neo4j: investigation graph"]
    EVI["Evidence store: SHA-256 sealed"]
  end

  INV --> DASH
  REV --> DASH
  DASH --> API
  GRAPH --- DASH
  REP --- DASH

  SIN --> CASE
  CASE --> SDR
  SDR --> SRS
  SRS --> PG

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

  HEU --> NEO
  TRV --> NEO
  API --> PG
