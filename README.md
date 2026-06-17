# HydraNexus

**AI-powered threat detection and intelligence platform built for SOC analysts who need answers, not more dashboards.**

HydraNexus ingests security events, enriches indicators in real time, and runs them through a six-head AI consensus engine to produce structured threat assessments — with full transparency into how every score was reached.

🔗 **Live platform:** [hydranexus.io](https://hydranexus.io)

---

## The problem

Most SIEM platforms tell you *that* something is suspicious. They rarely tell you *why*, *how confident* they are, or *what to do next*. Analysts are left correlating tabs, chasing false positives, and writing remediation steps from scratch.

HydraNexus is built around a different premise: the platform should reason about threats the way a senior analyst does — from multiple angles simultaneously, with explicit logic, and with actionable output at the end.

---

## How it works

### Six-head consensus engine

Every event is evaluated by six specialized AI analysis heads, each focused on a distinct threat dimension:

| Head | Focus |
|------|-------|
| **PROT** | Protocol and network behavior anomalies |
| **BEHV** | Behavioral patterns and attacker methodology |
| **TACT** | MITRE ATT&CK tactic and technique mapping |
| **FORN** | Foreign infrastructure and geopolitical context |
| **BIZL** | Business logic and insider threat indicators |
| **INFR** | Infrastructure exposure and asset risk |

A seventh layer — the **Arbiter** — weighs the six heads and produces a final consensus threat assessment with a unified confidence score.

### IP enrichment with full transparency

HydraNexus enriches every IP address against VirusTotal, AbuseIPDB, Shodan, and Censys — then shows analysts exactly what data was injected into each head's prompt. No black boxes.

- Live vs. cached data is surfaced per result (24h TTL for VT/AbuseIPDB, 72h for Shodan/Censys)
- Each enriched IP shows its contribution to the final threat score
- Rate-limit protection prevents API quota exhaustion under load

### Built on real honeypot data

HydraNexus is trained and tested against 2,700+ real honeypot events captured via Cowrie SSH honeypots — not synthetic datasets. The platform has processed real SSH brute-force campaigns, credential stuffing attacks, and lateral movement attempts.

---

## Platform overview

| Tab | What it does |
|-----|--------------|
| **PRIMITIVES** | Raw event ingestion — paste JSON or upload batch files |
| **RELATED INCIDENTS** | Cross-event correlation and clustering |
| **ANALYSIS** | Six-head consensus output with Arbiter verdict |
| **ENRICH IP** | Real-time IP enrichment with scoring transparency |
| **ACTIONS** | Analyst response actions and recommendations |
| **REMEDIATION** | Auto-generated remediation steps per threat type |
| **MITRE** | ATT&CK tactic and technique mapping |
| **AUDIT** | Full scoring formula transparency — every weight visible |
| **EVENT LOG** | Platform activity, rate warnings, enrichment history |

---

## Architecture

```
Honeypot / SIEM events
        │
        ▼
   FastAPI backend  ←──────────────────────────────────┐
        │                                               │
        ├── Enrichment layer                            │
        │   ├── L1: In-memory cache (15 min)            │
        │   ├── L2: PostgreSQL cache (24–72h TTL)       │
        │   └── APIs: VirusTotal · AbuseIPDB · Shodan · Censys
        │                                               │
        ├── Six-head consensus engine                   │
        │   └── Ollama · hydranexus-detector:7B         │
        │       running locally on RTX 2060             │
        │                                               │
        └── Arbiter (final verdict + confidence score)  │
                │                                       │
                ▼                                       │
   Cloudflare Pages frontend  ───────────────────────────
   (hydranexus.io)
```

**Stack:**
- Frontend: React · Cloudflare Pages
- Backend: FastAPI · PostgreSQL · Render
- AI inference: Ollama · hydranexus-detector:7B · SimplyNuc (64GB RAM, RTX 2060)
- Enrichment: VirusTotal · AbuseIPDB · Shodan · Censys

---

## Current status

- ✅ v0.6 production-ready
- ✅ 2,700+ real honeypot events processed
- ✅ Six-head consensus engine operational
- ✅ Enrichment caching + rate-limit protection live
- ✅ MITRE ATT&CK mapping integrated
- ✅ Patent pending (filed June 2026)
- 🔄 University SOC pilot — in progress
- 🔄 Paid tier — roadmap

---

## Roadmap

**Phase 1 — SOC-ready**
- Alert lifecycle: triage → assign → escalate → resolve → close
- SLA enforcement (P1/15min, P2/1hr, P3/4hr)
- RBAC and user roles
- Multi-tenant isolation for university/enterprise deployments

**Phase 2 — Threat intelligence platform**
- IP-to-IP correlation and relationship analysis
- Historical intelligence (first seen / last seen / recurrence)
- Attribution engine — botnet, malware family, campaign mapping
- IOC → TTP mapping

**Phase 3 — AI-SOC tier**
- AI-generated cross-event investigation summaries
- SOAR playbook automation
- Threat graph visualization
- STIX/TAXII feed ingestion

---

## About

HydraNexus is built and operated by **Omar Al-Fayyadh** through [Telecom Design Solutions LLC](https://tdsllc.info).

Omar brings 15+ years of infrastructure engineering experience (OSP fiber, ISP operations, NOC), an M.S. in Information Assurance & Cybersecurity, and hands-on SOC analyst training to the platform's design. HydraNexus is not a research project — it is a working tool built to solve real detection and triage problems.

📧 Contact: [tdsllc.info](https://tdsllc.info)
🔗 Platform: [hydranexus.io](https://hydranexus.io)
💼 LinkedIn: [linkedin.com/in/omaralfayyadh](https://linkedin.com/in/omaralfayyadh)

---

*This repository contains the public product overview for HydraNexus. Source code is proprietary.*
