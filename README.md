<p align="center">
  <img src="https://img.shields.io/badge/Flagged_Addresses-520K+-0d9488?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Intel_Sources-12-0d9488?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Blockchains-3-0d9488?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Evasion_Typologies-6-0d9488?style=for-the-badge" />
</p>

<h1 align="center">Sanction<span>Flow</span></h1>
<p align="center"><strong>AI-Powered Blockchain Sanctions Intelligence Platform</strong></p>
<p align="center">
  <img src="https://img.shields.io/badge/Version-2.0-14b8a6?style=flat-square" />
  <img src="https://img.shields.io/badge/License-MIT-14b8a6?style=flat-square" />
  <img src="https://img.shields.io/badge/AI_Engine-SanctionFlow_AI-14b8a6?style=flat-square" />
  <img src="https://img.shields.io/badge/Open_Source-Yes-14b8a6?style=flat-square" />
</p>

<p align="center">
  <a href="https://sanctionflow.io">sanctionflow.io</a> · 
  <a href="https://github.com/sonmez-lab">GitHub</a> · 
  <a href="https://doi.org/10.2139/ssrn.6340458">Research</a>
</p>

---

## The Problem

OFAC maintains **~1,200 designated cryptocurrency addresses** on the SDN list. Commercial analytics platforms (Chainalysis, TRM Labs, Elliptic) can screen against them — at **$100K–$500K/year**. The GENIUS Act (July 2025) created hundreds of newly obligated entities that need sanctions screening but cannot afford these tools.

**SanctionFlow closes this gap.** Open-source. AI-powered. Free.

---

## Platform Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 1 — DATA AGGREGATION                                     │
│  12 sources · OFAC SDN sync every 30 min · 520K+ addresses      │
│  OFAC · EU · UK OFSI · Tether · Chainabuse · Etherscan · +6     │
└──────────────────────────────┬──────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 2 — BLOCKCHAIN ANALYSIS                                   │
│  Multi-chain · 1-hop & 2-hop neighborhood mapping                │
│  Bitcoin (Blockstream) · Ethereum (Etherscan) · Tron (Tronscan)  │
└──────────────────────────────┬──────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 3 — PATTERN DETECTION ENGINE                              │
│  6 evasion typologies · Corridor flow intelligence               │
│  Trade Settlement · Mining Monetization · Nested Exchange · +3   │
└──────────────────────────────┬──────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 4 — SANCTIONFLOW AI ENGINE                                │
│  Domain-trained compliance model · Multi-model orchestration     │
│  Adversarial debate protocol · ReAct autonomous agent            │
└──────────────────────────────┬──────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────┐
│  LAYER 5 — ALERT & REPORTING                                     │
│  Real-time alerts · Daily briefs · PDF reports · SAR drafts      │
│  REST API · Telegram · Email                                     │
└─────────────────────────────────────────────────────────────────┘
```

---

## SanctionFlow AI Engine

The intelligence layer is powered by a proprietary multi-model AI architecture purpose-built for sanctions compliance analysis.

| Component | Description |
|-----------|-------------|
| **Domain-Trained Compliance Model** | Fine-tuned on OFAC enforcement actions, corridor intelligence data, SAR narratives, and FinCEN advisories |
| **Multi-Model Orchestration** | Three independent AI models with automatic failover for uninterrupted analysis |
| **Adversarial Debate Protocol** | Prosecutor / Defender / Judge framework — three models stress-test each risk assessment from opposing perspectives |
| **ReAct Investigation Agent** | Autonomous multi-step blockchain investigation using Reasoning-and-Acting pattern |
| **Corridor-Specific Training Data** | Model trained on 890,000 analyzed transactions and 52,000 attributed wallets from the Turkey-Russia-Iran corridor |
| **SAR Narrative Generation** | Generates FinCEN-compliant suspicious activity report narratives from structured risk data |

**Why a domain-specific model matters:** General-purpose AI models lack the specialized knowledge to distinguish between legitimate high-volume stablecoin transfers and sanctions evasion patterns. SanctionFlow AI is trained on real enforcement data — OFAC designations, DOJ seizure documents, corridor transaction patterns — enabling detection that generic models cannot achieve.

---

## Threat Intelligence Sources

| # | Source | Type | Records | Confidence |
|---|--------|------|---------|------------|
| 1 | **OFAC SDN List** | 🏛️ U.S. Government | ~772 crypto addresses | 🟢 1.00 |
| 2 | **EU Consolidated Sanctions** | 🏛️ Government | Variable | 🟢 0.95 |
| 3 | **UK OFSI List** | 🏛️ Government | Variable | 🟢 0.95 |
| 4 | **Tether (USDT) Blacklist** | 🔒 Issuer Enforcement | ~1,200 frozen | 🔵 0.90 |
| 5 | **Ransomwhere Database** | 🔬 Security Research | ~11,000 | 🔵 0.85 |
| 6 | **GraphSense TagPacks** | 🎓 Academic | 76 tagpack files | 🔵 0.80 |
| 7 | **Exchange Hot/Cold Wallets** | 📋 Curated | 500+ wallets | 🔵 0.75 |
| 8 | **Etherscan Labels** | 🌐 Open Source | ~45,000 | 🟡 0.70 |
| 9 | **EtherScamDB** | 📋 Curated | Variable | 🟡 0.65 |
| 10 | **Chainabuse Reports** | 👥 Community | ~50,000+ | 🟠 0.40 |
| 11 | **Tornado Cash Contracts** | 🏛️ OFAC Designated | 28 contracts | 🟢 1.00 |
| 12 | **Bridge Contracts** | 📡 Monitored | 27 protocols | 🔵 0.75 |

**Composite Confidence:** `1 - ((1-c₁) × (1-c₂) × ... × (1-cₙ))` — Two sources at 0.70 each → composite **0.91**

---

## Evasion Typologies

Three core typologies empirically derived from **890,000 transactions** across **52,000 wallets** in the Turkey-Iran corridor:

### 🔴 Typology 1: Stablecoin Trade Settlement

USDT on Tron as a substitute for banking wire transfers in sanctioned-jurisdiction trade.

| Factor | Weight | Description |
|--------|--------|-------------|
| Stablecoin Ratio | `0.20` | % of transactions in USDT/TRC20 |
| Round Amounts | `0.20` | % with round denominations ($10K, $50K, $100K ±2%) |
| Temporal Regularity | `0.20` | Coefficient of variation of inter-tx intervals |
| Business Hours | `0.20` | % during UTC+3/+3:30 (09:00–18:00) |
| Volume Velocity | `0.10` | Total volume / address age in days |
| Counterparty Conc. | `0.10` | % volume to top 3 counterparties |

### 🟠 Typology 2: Mining Revenue Monetization

Iranian mining operations (~4.5% global BTC hashrate) converting rewards to stablecoins.

| Factor | Weight | Description |
|--------|--------|-------------|
| Mining Pool Inputs | `0.30` | % inputs from known mining pools |
| Consolidation | `0.25` | Many small inputs → single large output |
| Rapid Conversion | `0.25` | Receipt to exchange deposit < 24h |
| Exchange Output | `0.20` | % outputs to known exchange addresses |

### 🟡 Typology 3: Nested Exchange Services

Unlicensed exchanges nested within legitimate infrastructure.

| Factor | Weight | Description |
|--------|--------|-------------|
| Distribution | `0.25` | Single input → 10+ unique outputs |
| Re-aggregation | `0.25` | Funds return to similar address < 48h |
| Cross-chain | `0.20` | Bridge interactions (BTC→ETH→TRON) |
| Short Holding | `0.15` | Median holding time < 4 hours |
| High Turnover | `0.15` | Total volume > 20× current balance |

---

## Risk Scoring

```
RISK = max(T1, T2, T3) × 0.50
     + sdn_proximity    × 0.30
     + volume_score     × 0.10
     + recency_score    × 0.10
```

| SDN Proximity | Score |
|---------------|-------|
| Direct SDN match | 100 |
| 1-hop neighbor | 80 |
| 2-hop neighbor | 50 |

| 🟢 Low | 🟡 Medium | 🟠 High | 🔴 Critical |
|---------|-----------|---------|-------------|
| 0 — 30 | 31 — 60 | 61 — 80 | 81 — 100 |

---

## Corridor Intelligence

```
🇮🇷 Iran          →     🇹🇷 Turkey        →     🇦🇪 UAE           →     🇷🇺 Russia
IRGC-QF / CBI          Transit / Nested         OTC / Cash-out         A7A5 / Garantex
Mining Operations       Exchanges                                       Grinex
```

| Metric | Value |
|--------|-------|
| Attributed Wallets | **52,000** |
| Analyzed Transactions | **890,000** |
| Est. Annual Corridor Volume | **$5.8B** |
| Evasion Typologies Identified | **3** |

---

## Platform Modules

| Module | Description |
|--------|-------------|
| 🔍 **Address Screening** | OFAC SDN + 520K threat DB cross-reference |
| 🧬 **Behavioral Fingerprint** | 6 typology scoring — detection beyond list matching |
| 🕸️ **Network Graph** | Interactive D3.js force-directed visualization |
| 🌊 **Transaction Flow** | Sankey diagrams for fund flow analysis |
| 🔗 **Cross-Chain Bridge Trace** | 27 bridge protocols monitored |
| 🌍 **Corridor Intelligence** | Turkey-Russia-Iran-UAE flow analysis |
| 🗺️ **Risk Heatmap** | 32 countries, 5 risk levels, 8 corridors |
| 🤖 **AI Risk Analysis** | SanctionFlow AI Engine with adversarial debate |
| 💬 **Compliance AI Assistant** | Natural language queries with source citations |
| 📋 **SAR Generator** | FinCEN-ready suspicious activity report drafts |
| 🛡️ **Threat Intel Database** | 520K+ addresses, 12 sources, composite scoring |
| 🚨 **Scam Explorer** | Search + community reporting |
| 📡 **Designation Radar** | Predictive OFAC designation scoring |
| ⚡ **Developer API** | REST API with tier-based access |
| 📊 **Daily Brief** | AI-generated intelligence summary |
| 📄 **PDF Reports** | Compliance-ready downloadable reports |

---

## Technology Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Python 3.11 · FastAPI · SQLAlchemy async · asyncpg |
| **Database** | PostgreSQL (Neon serverless) |
| **Cache** | Redis (24h TTL) |
| **AI Engine** | SanctionFlow AI — domain-trained multi-model architecture with adversarial debate and autonomous investigation agent |
| **Frontend** | React 18 · Vite · Tailwind CSS · D3.js |
| **Deployment** | Render (backend) · Vercel (frontend) · GitHub Actions (CI/CD) |

---

## Quick Start

```bash
git clone https://github.com/sonmez-lab/sanctionflow-ci.git
cd sanctionflow-ci
pip install -r requirements.txt
cp .env.example .env
uvicorn backend.main:app --reload --port 8000
```

```bash
# Screen an address
curl http://localhost:8000/api/screen/TJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# Threat intel stats
curl http://localhost:8000/api/threat-intel/stats
```

---

## References

- Chainalysis. (2026). *The 2026 Crypto Crime Report*
- GENIUS Act, Pub. L. No. 119-XX (2025)
- OFAC. (2022). Tornado Cash Designation
- OFAC. (2026). SDN List — Sanctions List Service
- Sonmez, O. (2025–2026). Eight studies on SSRN (see table above)

---

<p align="center">
  <strong>Developed by Osman Sonmez — Irvine, California</strong><br>
  <a href="https://sanctionflow.io">sanctionflow.io</a> · 
  <a href="https://github.com/sonmez-lab">github.com/sonmez-lab</a> · 
  <a href="https://scholar.google.com">Google Scholar</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/MIT-License-0d9488?style=flat-square" />
</p>
