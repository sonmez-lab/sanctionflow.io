# SanctionFlow -- AI-Powered Blockchain Sanctions Intelligence Platform
AI-powered blockchain sanctions intelligence platform – OFAC SDN screening, 520K+ threat addresses, multi-chain analysis, and real-time compliance tools for cryptocurrency transactions.

**Author:** Osman Sonmez | Sonmez Code Lab  
**Version:** 2.0 | April 2026  
**License:** MIT

---

## Abstract

SanctionFlow is an open-source intelligence platform for cryptocurrency sanctions compliance. It aggregates threat data from 12 independent sources into a unified database of 520,562+ flagged addresses, performs multi-chain transaction analysis across Bitcoin, Ethereum, and Tron networks, and generates AI-powered risk assessments through a multi-model architecture (Anthropic Claude, Qwen, DeepSeek with automatic failover). The platform implements three empirically derived evasion typologies as algorithmic scoring functions, provides interactive network graph visualization, and produces compliance-ready reports including SAR narrative drafts.

---

## 1. Problem Statement

OFAC maintains approximately 1,200 designated cryptocurrency addresses on the Specially Designated Nationals (SDN) list, spanning Bitcoin, Ethereum, and Tron networks. These designations cover programs including IRAN, RUSSIA, DPRK, and SDGT.

However, designation does not equal enforcement. The practical challenge is operationalization: enabling the thousands of virtual asset service providers (VASPs), compliance departments, and financial institutions that bear legal obligations under the Bank Secrecy Act (BSA) to screen transactions against these addresses -- and, critically, to detect the broader network of associated addresses that sanctioned actors use to avoid direct matches.

Commercial blockchain analytics platforms (Chainalysis Reactor, TRM Labs, Elliptic) offer comprehensive capabilities but command annual licensing fees of $100,000 to $500,000. These costs are prohibitive for smaller VASPs, fintech firms, money service businesses, compliance consultancies, and academic researchers.

The passage of the GENIUS Act (Guiding and Establishing National Innovation for U.S. Stablecoins Act) in July 2025 expanded BSA obligations to stablecoin issuers and intermediaries, creating hundreds of newly obligated entities that require sanctions screening capabilities. Meanwhile, cross-chain laundering reached an estimated $21.8 billion in 2025 (Chainalysis, 2026), and FinCEN has continued to expand reporting requirements for cryptocurrency transactions.

SanctionFlow addresses this gap by providing an open-source platform that combines SDN screening, multi-chain network analysis, behavioral pattern detection, and AI-powered risk assessment.

---

## 2. Platform Architecture

SanctionFlow implements a five-layer intelligence pipeline that transforms raw blockchain data and regulatory designations into structured risk assessments.

### 2.1 Data Aggregation Layer

Ingests and normalizes data from 12 authoritative sources (detailed in Section 5). Automated OFAC SDN synchronization runs every 30 minutes via GitHub Actions. Records are deduplicated across sources, and each address receives a composite confidence score using a probabilistic model that accounts for source reliability and cross-source corroboration.

### 2.2 Blockchain Analysis Layer

Chain-specific transaction analysis clients for three networks:

- **Bitcoin** -- Blockstream API (no key required)
- **Ethereum** -- Etherscan API (free tier: 5 calls/sec, 100K calls/day)
- **Tron** -- Tronscan API (free tier: ~3 calls/sec)

The layer performs 1-hop and 2-hop neighborhood mapping, constructing the transaction network around any queried address. For large-scale historical analysis, the platform integrates with Google BigQuery's public blockchain datasets, enabling queries across complete transaction histories without maintaining local chain data.

### 2.3 Pattern Detection Engine

Implements three evasion typologies (described in Section 4) as weighted scoring functions. Computes an overall risk score using a composite formula:

```
OVERALL_RISK = max(T1, T2, T3) * 0.50
             + sdn_proximity    * 0.30
             + volume_score     * 0.10
             + recency_score    * 0.10
```

SDN proximity scoring: direct SDN match = 100, 1-hop neighbor = 80, 2-hop neighbor = 50. Risk classification: Low (0-30), Medium (31-60), High (61-80), Critical (81-100).

A dedicated Corridor Flow Intelligence Engine monitors stablecoin flows between Turkey, Russia, Iran, and the UAE, using timezone-based geographic attribution (UTC+3 / UTC+3:30 business hours) to identify trade settlement patterns.

### 2.4 AI Intelligence Layer

Multi-model architecture with automatic failover:

- **Primary:** Anthropic Claude -- generates structured natural-language risk assessments
- **Fallback:** OpenRouter (Qwen, DeepSeek) -- ensures uninterrupted service

The layer includes an autonomous investigation agent using the ReAct (Reasoning and Acting) pattern for multi-step blockchain investigations, and an adversarial debate protocol (prosecutor/defender/judge roles) for stress-testing risk assessments.

### 2.5 Alert and Reporting System

Produces automated intelligence products:

- Real-time alerts on new OFAC designations (detected within minutes of publication)
- Daily intelligence briefs summarizing corridor activity and regulatory developments
- Downloadable PDF compliance reports
- SAR narrative drafts in FinCEN-compliant format
- Structured API responses for integration into existing compliance workflows

---

## 3. Modules

### 3.1 Address Screening

Screens any BTC, ETH, or TRX address against the OFAC SDN list (772 designated crypto addresses) and the full threat intelligence database (520,562+ records from 12 sources). Returns direct SDN matches, threat database hits, and counterparty attribution from known entity labels.

Auto-detects blockchain type from address format using regex patterns. Performs counterparty cross-referencing: when an address is screened, its transaction counterparties are checked against the entire threat database, expanding the detection surface beyond the queried address itself.

*Enforcement context:* When OFAC designated the Zedcex Exchange in January 2026, the designation included 7 Tron wallet addresses. However, Zedcex operated through hundreds of additional addresses not included in the designation. Simple SDN matching would catch only the 7 designated addresses. SanctionFlow's cross-reference against 520K+ threat records and behavioral scoring identifies network addresses that exhibit the same operational patterns.

### 3.2 Behavioral Fingerprinting

Evaluates addresses against six evasion typologies (three empirically derived, three pattern-based -- see Section 4). Each typology uses weighted behavioral indicators to generate a score from 0-100. This enables detection of sanctions evasion activity even when an address does not appear on any list and has no direct link to a designated address.

The three empirically derived typologies are based on analysis of 890,000 transactions across 52,000 wallets in the Turkey-Iran corridor:

- **Stablecoin Trade Settlement** -- USDT on Tron as a substitute for banking wires. Detection factors: stablecoin ratio, round-denomination amounts (consistent with invoice settlement), temporal regularity, business-hour concentration (UTC+3/+3:30), volume velocity, counterparty concentration.
- **Mining Revenue Monetization** -- Conversion of mining rewards (Iran accounts for ~4.5% of global Bitcoin hashrate) into stablecoins. Detection factors: mining pool input ratio, consolidation patterns, conversion speed, exchange output ratio.
- **Nested Exchange Services** -- Unlicensed exchanges nested within legitimate exchange infrastructure. Detection factors: distribution patterns, re-aggregation behavior, cross-chain bridge interactions, short holding periods, high turnover ratios.

### 3.3 Network Graph

Interactive D3.js force-directed graph visualization. Maps the 1-hop and 2-hop transaction neighborhood of any address. Dark theme with directed edges.

- Node colors: red (SDN), orange (high risk), yellow (medium), green (low), gray (unknown)
- Node size: proportional to transaction volume
- Edge thickness: proportional to transaction count
- Click any node for detail panel with AI-generated risk analysis

*Enforcement context:* The Turkoca network documented in FinCEN's MBaer NPRM (proposed rule for special measures) illustrates multi-jurisdiction complexity -- a company registered in Korea, operated from Turkey, banking through Switzerland, facilitating IRGC-QF oil smuggling. Network graph visualization reveals these cross-jurisdictional relationships that flat list-based screening cannot surface.

### 3.4 Transaction Flow Visualization

Sankey diagrams showing value flow through intermediary addresses from source to destination. Visualizes how funds move through layering chains, making it straightforward to identify the path between a source address and its ultimate destination. Supports BTC, ETH, and TRX transactions.

### 3.5 Cross-Chain Bridge Trace

Monitors 27 bridge protocol contracts including Wormhole, Multichain, Polygon Bridge, Hop Protocol, Synapse, Stargate, Arbitrum Bridge, and Optimism Gateway. Detects chain-hopping evasion patterns (the BTC-to-ETH-to-TRON laundering sequence commonly used by sanctioned actors). Assigns elevated severity scores to interactions with compromised or deprecated bridge protocols.

*Enforcement context:* The Lazarus Group's exploitation of the Ronin Bridge ($625 million, March 2022) demonstrated how bridge protocols serve as laundering infrastructure. Cross-chain movement is the primary technique by which sanctioned actors exploit the fragmented nature of blockchain analytics.

### 3.6 Corridor Intelligence

Stablecoin flow analysis between Turkey, Russia, Iran, and the UAE. The corridor engine attributes transactions to likely jurisdictions using temporal analysis of business-hour patterns (UTC+3 for Turkey, UTC+3:30 for Iran). Detects trade settlement signatures, computes corridor-specific risk scores, and tracks flow volumes over time.

*Enforcement context:* The Aifory Pro case involved a cash-to-crypto service operating simultaneously from Moscow, Dubai, and Istanbul, transferring funds to an Iranian exchange. This illustrates how corridor actors exploit multiple jurisdictions -- and why jurisdiction-level flow monitoring is needed alongside address-level screening.

### 3.7 Risk Heatmap

D3.js geographic visualization displaying sanctions evasion risk concentration across 32 countries, 5 risk levels, 8 monitored corridors, and 6 evasion typologies. Provides a geographic overview of where high-risk cryptocurrency activity concentrates, updated with live corridor flow data.

### 3.8 AI Risk Analysis

Generates structured natural-language risk assessments using the multi-model AI architecture. Each assessment includes:

1. Overall risk level with key reasoning
2. Typology analysis (which behavioral patterns match and why)
3. SDN connection analysis (distance to designated addresses)
4. Recommended actions (specific, actionable compliance steps)
5. Confidence level (data sufficiency assessment)

For high-stakes determinations, the adversarial debate protocol deploys multiple AI models in prosecutor, defender, and judge roles to evaluate the risk assessment from opposing perspectives. This produces more robust conclusions than single-model analysis.

### 3.9 Compliance AI Assistant

Natural language query interface for sanctions compliance questions. Queries are answered against OFAC guidance, SDN data, and the threat intelligence database. Example queries:

- "Is this address connected to any sanctioned entity?"
- "What are the red flags for Tron-based transfers from Turkey?"
- "Summarize recent OFAC designations involving cryptocurrency."

Responses include source citations and confidence levels.

### 3.10 SAR Generator

Generates FinCEN-ready Suspicious Activity Report narrative drafts. Translates detected patterns, risk scores, and network topology data into the structured narrative format required by BSA reporting obligations. Exports to PDF.

SAR narrative preparation typically takes a compliance officer 30-60 minutes per filing. The generator automates the initial draft, documenting the analytical basis for each filing.

### 3.11 Threat Intelligence Database

520,562+ addresses aggregated from 12 independent sources. Each record includes source attribution, category classification, confidence score, and last-seen timestamp. Composite confidence scoring combines signals from multiple sources: when two independent sources with confidence 0.70 each flag the same address, the composite confidence rises to 0.91. Full source details in Section 5.

### 3.12 Scam Explorer and Community Reporting

Search interface across the 520K+ threat intelligence database with filters by source, category, blockchain, confidence score, and date range. Paginated results with address detail views.

Community reporting enables users to submit scam addresses with evidence. This creates a bidirectional intelligence model -- the platform both provides and receives threat data -- forming a feedback loop that strengthens the database over time.

### 3.13 Developer API

REST API with tier-based access control (public, verified, researcher, internal). Key endpoints:

| Endpoint Group | Prefix | Purpose |
|---|---|---|
| Address Screening | `/api/screen` | SDN + threat DB screening |
| AI Analysis | `/api/analyze` | AI-powered risk assessment |
| Network Graph | `/api/graph` | Transaction neighborhood data |
| Compliance Chat | `/api/query` | Natural language queries |
| Intelligence Briefs | `/api/brief` | Daily brief generation |
| Reports | `/api/report` | PDF report generation |
| Threat Intel | `/api/threat-intel` | Threat database lookups |
| Corridor Intelligence | `/api/corridor` | Corridor flow analysis |
| Bridge Trace | `/api/bridge` | Cross-chain bridge monitoring |
| SAR Generation | `/api/sar` | SAR narrative drafts |

Rate limiting via Redis. API key management with registration and email verification. Full API documentation available at the `/developer` portal.

---

## 4. Evasion Typologies

SanctionFlow implements six evasion typologies as detection algorithms. The first three are empirically derived from research analyzing 890,000 transactions across 52,000 wallets in the Turkey-Iran corridor. The remaining three are pattern-based, targeting common laundering techniques documented in enforcement actions.

### 4.1 Stablecoin Trade Settlement (Empirical)

Sanctioned-jurisdiction actors using USDT on the Tron network as a substitute for traditional banking wire transfers in trade settlement. This typology accounts for the largest volume of detected corridor activity.

Detection uses six weighted factors:

| Factor | Weight | Description |
|---|---|---|
| Stablecoin ratio | 0.20 | Percentage of transactions that are USDT/TRC20 |
| Round amounts | 0.20 | Percentage of transactions with round denominations ($10K, $50K, $100K +/-2%) |
| Temporal regularity | 0.20 | Coefficient of variation of inter-transaction intervals (lower = more regular) |
| Business hours | 0.20 | Percentage of transactions during UTC+3/+3:30 business hours (09:00-18:00) |
| Volume velocity | 0.10 | Total volume / address age in days |
| Counterparty concentration | 0.10 | Percentage of volume going to top 3 counterparties |

### 4.2 Mining Revenue Monetization (Empirical)

Conversion of cryptocurrency mining rewards -- particularly from Iranian mining operations (~4.5% of global Bitcoin hashrate) -- into stablecoins through rapid exchange interactions.

| Factor | Weight | Description |
|---|---|---|
| Mining pool inputs | 0.30 | Percentage of inputs from known mining pool addresses |
| Consolidation pattern | 0.25 | Many small inputs aggregated into single large output |
| Rapid conversion | 0.25 | Time from receipt to exchange deposit (<24h) |
| Exchange output ratio | 0.20 | Percentage of outputs directed to known exchange addresses |

### 4.3 Nested Exchange Services (Empirical)

Unlicensed exchange operations nested within legitimate exchange infrastructure, operating as intermediaries that facilitate sanctions evasion.

| Factor | Weight | Description |
|---|---|---|
| Distribution pattern | 0.25 | Single input dispersed to >10 unique output addresses |
| Re-aggregation | 0.25 | Distributed funds return to similar address within 48h |
| Cross-chain activity | 0.20 | Bridge interactions consistent with BTC-to-ETH-to-TRON pattern |
| Short holding period | 0.15 | Median holding time <4 hours |
| High turnover | 0.15 | Total volume >> current balance (>20x ratio) |

### 4.4 Offshore Shell Structuring (Pattern-Based)

Value routed through corporate wallet structures associated with shell companies in multiple jurisdictions. Detection identifies: high-volume wallets with minimal on-chain history, rapid fund pass-through with negligible retention, and counterparty concentration in known offshore jurisdictions.

### 4.5 Cross-Chain Bridge Hopping (Pattern-Based)

Sequential movement of funds across multiple blockchain networks using bridge protocols to obscure the transaction trail. Detection monitors interactions with 27 tracked bridge contracts, identifies multi-hop chain sequences (e.g., BTC to ETH to TRON), and flags rapid bridge usage within short time windows.

### 4.6 Mixer and Tumbler Services (Pattern-Based)

Use of mixing protocols and tumbler services to break the on-chain link between source and destination. Detection identifies interactions with known mixing contracts (including the 28 OFAC-designated Tornado Cash addresses), equal-output transaction patterns characteristic of CoinJoin, and post-mix distribution behaviors.

---

## 5. Threat Intelligence Sources

| # | Source | Type | Records | Confidence | Description |
|---|---|---|---|---|---|
| 1 | OFAC SDN List | Regulatory | ~772 crypto addresses | 1.00 | U.S. Treasury designated addresses with program and entity metadata |
| 2 | EU Consolidated Sanctions | Regulatory | Variable | 0.95 | European Union sanctions list, parsed from XML |
| 3 | UK OFSI List | Regulatory | Variable | 0.95 | UK Office of Financial Sanctions Implementation |
| 4 | Tether (USDT) Blacklist | Issuer enforcement | ~1,200 | 0.90 | Addresses frozen via AddedBlackList smart contract event |
| 5 | Circle (USDC) Freezes | Issuer enforcement | Variable | 0.90 | Addresses frozen by USDC issuer |
| 6 | Ransomwhere Database | Security research | ~11,000 | 0.85 | Bitcoin addresses attributed to ransomware operations |
| 7 | Chainabuse Reports | Community | ~50,000+ | 0.40 | Community-submitted scam and fraud reports |
| 8 | Etherscan Labels | Open-source | ~45,000 | 0.70 | Labeled Ethereum addresses (exchanges, contracts, flagged entities) |
| 9 | GraphSense TagPacks | Academic | 76 tagpack files | 0.80 | Address attribution from Austrian Institute of Technology |
| 10 | EtherScamDB / CryptoScamDB | Curated | Variable | 0.65 | Phishing, Ponzi, and investment fraud addresses |
| 11 | Tornado Cash Contracts | OFAC-designated | 28 contracts | 1.00 | OFAC-designated Tornado Cash mixing contracts (Aug 2022) |
| 12 | Exchange Hot/Cold Wallets | Curated | ~500+ | 0.75 | Known wallets for Binance, Coinbase, Kraken, Bitfinex, OKX, Bybit, Huobi, Gate.io, KuCoin, Gemini |

**Bridge Contract Monitoring:** 27 contracts tracked across Wormhole, Multichain, Polygon Bridge, Hop Protocol, Synapse, Stargate, Arbitrum Bridge, Optimism Gateway, and others. Elevated severity for compromised or deprecated protocols.

**Composite Confidence Formula:** When multiple independent sources flag the same address, confidence is calculated as:

```
composite = 1 - ((1 - c1) * (1 - c2) * ... * (1 - cn))
```

Two sources at 0.70 each yield a composite of 0.91, reflecting the low probability of independent false positives converging on the same address.

---

## 6. Technology Stack

### Backend

- **Language:** Python 3.11
- **Framework:** FastAPI with async/await throughout
- **ORM:** SQLAlchemy async with asyncpg driver
- **Database:** PostgreSQL (Neon serverless, NullPool for compatibility)
- **Cache:** Redis (24-hour TTL on all external API responses)
- **HTTP Client:** httpx (async)
- **Retry Logic:** tenacity library on all external calls
- **PDF Generation:** ReportLab
- **Config:** pydantic-settings (all secrets via environment variables)

### Frontend

- **Framework:** React 18 with Vite bundler
- **Styling:** Tailwind CSS
- **Visualization:** D3.js (network graphs, heatmaps, Sankey diagrams)
- **Routing:** React Router
- **HTTP:** Axios via custom `useApi` hook
- **Proxy:** Vite dev server proxies `/api` to backend at localhost:8000

### AI Providers

- **Primary:** Anthropic Claude (via `anthropic` SDK)
- **Fallback:** OpenRouter API (access to Qwen, DeepSeek, and other models)
- **Architecture:** Abstract `AIProvider` base class, `AIManager` singleton with automatic failover between providers

### Deployment

- **Backend:** Render (also supports Railway). Start command: `uvicorn backend.main:app --host 0.0.0.0 --port $PORT`
- **Frontend:** Vercel with API rewrites to backend
- **CI/CD:** GitHub Actions
  - SDN sync: every 30 minutes
  - Daily intelligence briefs: 12:00 UTC
  - Neighbor crawling: scheduled

### Middleware Stack (applied in order)

1. CORS
2. Abuse Protection -- IP reputation and abuse detection
3. Rate Limiting -- tier-based via Redis
4. API Authentication -- API key validation, user and tier attached to request state

---

## 7. Research Foundation

The detection algorithms in SanctionFlow are derived from empirical research rather than theoretical models.

**Primary research:** "Turkey-Iran Cryptocurrency Corridor: A Legal and Empirical Analysis" (Sonmez, 2025, SSRN). This study documented 847 seed addresses drawn from OFAC designations, FinCEN advisories, and court records. Through systematic blockchain network traversal, it attributed 52,000 wallets and analyzed 890,000 transactions, producing the first comprehensive empirical map of the corridor's financial topology. The three core evasion typologies (Sections 4.1-4.3) were identified and formally specified in this research.

**Companion study:** "Turkey's FATF Grey-List Exit: An Empirical Assessment of AML/CFT Reforms" (Sonmez, 2025, SSRN). This paper examined Turkey's AML/CFT reform trajectory through the FATF grey-listing and delisting process, providing contextual intelligence about the regulatory environment in which corridor-based evasion operates.

**Policy contribution:** Formal public comment submitted to FinCEN (Docket FINCEN-2026-0001) with data-driven recommendations on detecting cryptocurrency-based sanctions evasion in OFAC-designated corridors, drawing on the empirical findings and operational experience from the platform's development.

---

## 8. Getting Started

### Prerequisites

- Python 3.11+
- Node.js 18+
- PostgreSQL (or Neon serverless)
- Redis (optional, for caching)

### Quick Start

```bash
# Clone the repository
git clone https://github.com/sonmez-lab/sanctionflow-ci.git
cd sanctionflow-ci

# Backend setup
python -m venv venv
source venv/bin/activate        # Unix
# venv\Scripts\activate         # Windows
pip install -r requirements.txt

# Configure environment
cp .env.example .env
# Edit .env with your API keys (Etherscan, Tronscan, Anthropic/OpenRouter)

# Start backend
uvicorn backend.main:app --reload --port 8000

# Frontend setup (new terminal)
cd frontend
npm install
npm run dev
```

The app boots in degraded mode without optional services (database, Redis, AI keys), so you can explore the architecture even with minimal configuration.

### API Quick Start

```bash
# Health check
curl http://localhost:8000/health

# Screen an address
curl http://localhost:8000/api/screen/TJxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# Get threat intelligence stats
curl http://localhost:8000/api/threat-intel/stats
```

### Running Tests

```bash
pytest backend/tests/ -v

# Single test file
pytest backend/tests/test_sdn_parser.py -v

# Linting
ruff check backend/
```

---

## References

Chainalysis. (2026). *The 2026 Crypto Crime Report*. Chainalysis Inc.

Financial Crimes Enforcement Network. (2026). Proposed Rule: Enhanced Due Diligence for Convertible Virtual Currency Mixing. Docket FINCEN-2026-0001.

Guiding and Establishing National Innovation for U.S. Stablecoins Act (GENIUS Act), Pub. L. No. 119-XX (2025).

Office of Foreign Assets Control. (2022). "U.S. Treasury Sanctions Notorious Virtual Currency Mixer Tornado Cash." Press Release, August 8, 2022.

Office of Foreign Assets Control. (2021). *Sanctions Compliance Guidance for the Virtual Currency Industry*. U.S. Department of the Treasury.

Office of Foreign Assets Control. (2026). *Specially Designated Nationals and Blocked Persons List (SDN List)*. U.S. Department of the Treasury.

Sonmez, O. (2025). "Turkey-Iran Cryptocurrency Corridor: A Legal and Empirical Analysis." Social Science Research Network (SSRN).

Sonmez, O. (2025). "Turkey's FATF Grey-List Exit: An Empirical Assessment of AML/CFT Reforms." Social Science Research Network (SSRN).

Weber, M., et al. (2019). "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics." *KDD Workshop on Anomaly Detection in Finance*.

---

*Developed by Sonmez Code Lab -- California, United States*
