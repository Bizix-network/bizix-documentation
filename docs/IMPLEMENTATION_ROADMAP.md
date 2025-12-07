# 🗺️ BiziX — Roadmap Implementare: De la Realitate la Whitepaper

> **Scop:** Alinierea completă a implementării tehnice cu promisiunile din Whitepaper  
> **Status curent:** 60% aliniat  
> **Target:** 95% aliniat  
> **Estimare totală:** 12-16 săptămâni (1 developer full-time)

---

## 📊 Gap Analysis: Whitepaper vs. Realitate

### Legendă Status
- ✅ **Implementat** — Funcțional, aliniat cu WP
- 🔄 **Parțial** — Există, dar incomplet
- ❌ **Lipsă** — Promis în WP, neimplementat
- ⚠️ **Problemă** — Implementat dar cu issues

---

### Sumar pe Capitole Whitepaper

| Capitol WP | Promisiuni | Implementat | Gap |
|------------|------------|-------------|-----|
| Cap 2: Arhitectura 4 Piloni | 4 | 3 | **n8n lipsește** |
| Cap 3: Arhitectura Încrederii | 8 | 3 | **Securitate, Descentralizare** |
| Cap 4: Ecosistemul | 12 | 6 | **Ambasadori, Dezvoltatori, BYOS** |
| Cap 5: Guvernanța | 5 | 2 | **Voting real, Quorum** |
| Cap 8: BiziX AI | 6 | 1 | **AI complet lipsește** |
| Cap 10: Tokenomics | 8 | 2 | **Burn, Tiers, Staking complet** |

---

## 🎯 FAZA 0: Urgențe (Săptămâna 1-2)

### 🔴 Fix Securitate Blockchain

**De ce urgent?** Nu poți lansa un blockchain cu vulnerabilități critice.

| Task | Fișier | Efort | Prioritate |
|------|--------|-------|------------|
| Fix `reject_proposal` fără validare origin | `pallets/bizix/src/lib.rs` | 2h | 🔴 P0 |
| Fix `update_company` fără verificare ownership | `pallets/company_registry/src/lib.rs` | 2h | 🔴 P0 |
| Configurare `TechnicalCommittee` corect | `runtime/src/lib.rs` | 4h | 🔴 P0 |
| Schimbare `approval_threshold` din hardcoded | `pallets/bizix/src/lib.rs` | 2h | 🟠 P1 |
| Adăugare validare origin pentru `change_proposal_status` | `pallets/bizix/src/lib.rs` | 2h | 🟠 P1 |

**Verificare completare:**
```bash
# Trebuie să eșueze când user normal încearcă:
- reject_proposal fără să fie în TechnicalCommittee
- update_company pentru o companie pe care nu o deține
- change_proposal_status fără permisiuni
```

**Livrabil:** Blockchain securizat, ready pentru audit

---

## 🎯 FAZA 1: Fundație (Săptămânile 3-6)

### 1.1 Event Bus Centralizat

**Referință WP:** Cap 2 (Orchestrare), Cap 4 (Automatizări)

| Task | Efort | Dependențe |
|------|-------|------------|
| Setup Redis Streams module în `shared/` | 4h | - |
| Definire event types și streams | 2h | - |
| Publisher în VM Service | 4h | Redis Streams |
| Publisher în Order Service | 3h | Redis Streams |
| Publisher în Auth Service | 2h | Redis Streams |
| Notification Worker (email) | 6h | Redis Streams |
| Audit Worker (logging centralizat) | 4h | Redis Streams |
| Integrare PM2 ecosystem | 2h | Workers |

**Livrabil:** Event Bus funcțional cu 3 workeri

---

### 1.2 Integration Engine (n8n)

**Referință WP:** Cap 2 (Pilonul 3), Cap 6 (BPaaS), Cap 8 (AI)

| Task | Efort | Dependențe |
|------|-------|------------|
| Setup n8n self-hosted (Docker) | 4h | - |
| Configurare Keycloak SSO pentru n8n | 4h | n8n running |
| Creare credențiale pentru servicii BiziX | 2h | n8n running |
| **Workflow 1:** Factură → OCR → Ciornă contabilitate | 8h | n8n + AI |
| **Workflow 2:** Email nou → Clasificare → CRM | 6h | n8n + AI |
| **Workflow 3:** Comandă finalizată → Notificare + Update stoc | 4h | n8n + Event Bus |
| Documentație workflows | 4h | Workflows |

**Livrabil:** n8n funcțional cu 3 workflow-uri demo

---

### 1.3 AI Gateway Extindere

**Referință WP:** Cap 8 (BiziX AI)

| Task | Efort | Dependențe |
|------|-------|------------|
| Setup Ollama/vLLM pentru modele locale | 6h | - |
| API Gateway pentru AI requests | 4h | LLM running |
| OCR facturi (Tesseract + preprocessing) | 8h | - |
| Clasificare documente (fine-tuned model) | 12h | Training data |
| Integrare RAG existent cu n8n | 4h | n8n |
| Human-in-the-Loop queue | 6h | Event Bus |
| AI Trust Layer (hash-uri on-chain) | 8h | Blockchain |

**Livrabil:** AI Gateway cu OCR, clasificare, RAG

---

## 🎯 FAZA 2: Ecosistem (Săptămânile 7-10)

### 2.1 Ambassador Portal

**Referință WP:** Cap 4 (Programul Ambasadori), Cap 11

| Task | Efort | Dependențe |
|------|-------|------------|
| Setup Next.js app (clone din Client Portal) | 4h | - |
| Pagină înregistrare ambasador | 6h | Keycloak |
| Dashboard comisioane | 8h | Backend API |
| Link generator personalizat | 4h | - |
| Tracking referrals | 6h | Event Bus |
| Pagină publică ambasador | 6h | - |
| Rapoarte și export | 4h | - |
| Smart contract comisioane (basic) | 12h | Blockchain |

**API Backend necesare:**
```
POST /api/ambassadors/register
GET  /api/ambassadors/me
GET  /api/ambassadors/referrals
GET  /api/ambassadors/commissions
GET  /api/ambassadors/:slug (public profile)
```

**Livrabil:** Ambassador Portal MVP

---

### 2.2 Developer Portal

**Referință WP:** Cap 4 (Mediu Dezvoltatori)

| Task | Efort | Dependențe |
|------|-------|------------|
| Setup Next.js app | 4h | - |
| Documentație API interactivă (Swagger embed) | 4h | OpenAPI specs |
| SDK-uri (npm package BiziX) | 12h | API stabil |
| Ghid "Prima ta aplicație" | 6h | SDK |
| Dashboard aplicații publicate | 6h | Marketplace API |
| Revenue tracking pentru devs | 6h | Event Bus |
| Sandbox/Testing environment | 8h | Proxmox |

**Livrabil:** Developer Portal cu SDK și docs

---

### 2.3 Marketplace Îmbunătățiri

**Referință WP:** Cap 4 (BiziX Marketplace)

| Task | Efort | Dependențe |
|------|-------|------------|
| Listing aplicații terțe (nu doar interne) | 6h | Dev Portal |
| Review/aprobare aplicații | 8h | Governance |
| Rating și reviews | 6h | - |
| Revenue share automat (smart contract) | 12h | Blockchain |
| Concierge Personalizări (formular + matching) | 10h | - |

**Livrabil:** Marketplace deschis pentru dezvoltatori

---

## 🎯 FAZA 3: Blockchain Complet (Săptămânile 11-14)

### 3.1 Tokenomics Complet

**Referință WP:** Cap 10

| Task | Efort | Dependențe |
|------|-------|------------|
| **Burn mechanism** (20% pe tranzacție) | 8h | - |
| **Tier system** în runtime | 12h | - |
| - Tier 1 (Holder): discount 15% | 4h | Tiers |
| - Tier 2 (Staker 1K): beta access | 4h | Tiers |
| - Tier 3 (Delegator 5K): revenue share | 6h | Tiers |
| - Tier 4 (Validator 10K): full benefits | 4h | Tiers |
| **Staking complet** | 16h | - |
| - Lock-up periods (flex/3/6/12 luni) | 6h | - |
| - Multiplicatori recompense | 4h | - |
| - Unstaking delays | 4h | - |
| **Treasury buyback** (10% profit trimestrial) | 8h | Governance |
| RPC-uri pentru tier checking | 4h | Tiers |
| Frontend: tier display în Portal | 4h | RPC |

**Livrabil:** Tokenomics complet conform WP

---

### 3.2 Governance Complet

**Referință WP:** Cap 5

| Task | Efort | Dependențe |
|------|-------|------------|
| Tipuri propuneri (Minor/Standard/Major/Critic) | 8h | - |
| Quorum diferențiat per tip | 6h | Tipuri |
| Perioada de vot configurabilă (7-14 zile) | 4h | - |
| Multiplicator vot (Holder/Staker/Validator) | 6h | Tiers |
| Cost propunere (100 BIZ, returnat dacă trece) | 4h | - |
| UI Governance în Portal | 12h | Backend |
| Delegare vot | 8h | - |

**Livrabil:** Governance on-chain conform WP

---

### 3.3 Descentralizare Faza 1

**Referință WP:** Cap 3 (Descentralizare 3 Faze)

| Task | Efort | Dependențe |
|------|-------|------------|
| Setup 3 noduri validator în locații diferite | 8h | Infrastructure |
| Publicare cod pe GitHub (open-source) | 4h | Cleanup |
| Anchoring zilnic pe Polygon PoS | 12h | Polygon account |
| Script verificare anchoring | 4h | Anchoring |
| Documentație pentru validatori externi | 6h | - |

**Livrabil:** Faza 1 descentralizare completă

---

### 3.4 Explorer și Wallet Finalizare

**Referință WP:** Cap 4 (Casele Platformei)

| Task | Efort | Dependențe |
|------|-------|------------|
| Explorer: Verificare tranzacții | 4h | - |
| Explorer: Verificare anchoring Polygon | 4h | Anchoring |
| Explorer: Governance history | 4h | Governance |
| Wallet: Staking UI | 6h | Staking |
| Wallet: Delegare UI | 4h | Delegare |
| Wallet: Tier display | 2h | Tiers |
| Wallet: Conversie RON ↔ BIZ (integrare PSP) | 12h | Payment gateway |

**Livrabil:** Explorer și Wallet production-ready

---

## 🎯 FAZA 4: Polish & Launch (Săptămânile 15-16)

### 4.1 Teste și Securitate

| Task | Efort | Dependențe |
|------|-------|------------|
| Unit tests blockchain (target 60%) | 16h | - |
| Unit tests backend (target 50%) | 12h | - |
| Integration tests | 12h | - |
| Security audit intern | 8h | - |
| Penetration testing basic | 8h | - |
| Bug bounty setup | 4h | - |

**Livrabil:** Test coverage 50%+, audit report

---

### 4.2 Documentație și i18n

| Task | Efort | Dependențe |
|------|-------|------------|
| Externalizare strings Website (50-60) | 6h | - |
| Externalizare strings Portal | 4h | - |
| README-uri actualizate toate componentele | 6h | - |
| User guides (Getting Started) | 8h | - |
| Video tutorials (3-5 video-uri) | 12h | - |

**Livrabil:** Documentație completă, i18n complet

---

### 4.3 Infrastructure & Monitoring

| Task | Efort | Dependențe |
|------|-------|------------|
| Grafana dashboards pentru VM metrics | 6h | - |
| Alerting (PagerDuty/Slack) | 4h | - |
| Backup verification | 4h | - |
| Disaster recovery plan | 4h | - |
| Load testing | 6h | - |

**Livrabil:** Production-ready infrastructure

---

## 📅 Timeline Vizuală

```
Săpt 1-2   ████ FAZA 0: Fix Securitate (URGENT)
           └── Blockchain vulnerabilities

Săpt 3-4   ████████ FAZA 1a: Event Bus
           └── Redis Streams + Workers

Săpt 5-6   ████████ FAZA 1b: n8n + AI
           └── Integration Engine + AI Gateway

Săpt 7-8   ████████ FAZA 2a: Ambassador Portal
           └── Comisioane, Referrals

Săpt 9-10  ████████ FAZA 2b: Developer Portal + Marketplace
           └── SDK, Docs, Revenue Share

Săpt 11-12 ████████ FAZA 3a: Tokenomics
           └── Burn, Tiers, Staking

Săpt 13-14 ████████ FAZA 3b: Governance + Descentralizare
           └── Voting, Anchoring Polygon

Săpt 15-16 ████████ FAZA 4: Polish & Launch
           └── Teste, Docs, Monitoring
```

---

## 📊 Estimări Totale

### Per Fază

| Fază | Săptămâni | Ore | Focus Principal |
|------|-----------|-----|-----------------|
| **FAZA 0** | 2 | 14h | Securitate blockchain |
| **FAZA 1** | 4 | 90h | Event Bus, n8n, AI |
| **FAZA 2** | 4 | 100h | Ambassador, Developer, Marketplace |
| **FAZA 3** | 4 | 130h | Tokenomics, Governance, Descentralizare |
| **FAZA 4** | 2 | 80h | Teste, Docs, Monitoring |
| **TOTAL** | **16** | **~414h** | |

### Per Componentă Nouă

| Componentă | Ore | Status WP |
|------------|-----|-----------|
| Event Bus | 27h | ❌→✅ |
| n8n Integration | 32h | ❌→✅ |
| AI Gateway | 48h | 🔄→✅ |
| Ambassador Portal | 56h | ❌→✅ |
| Developer Portal | 46h | ❌→✅ |
| Tokenomics Complet | 64h | 🔄→✅ |
| Governance Complet | 48h | 🔄→✅ |
| Descentralizare F1 | 34h | ❌→✅ |
| Wallet/Explorer | 36h | 🔄→✅ |
| Teste & Docs | 88h | 🔄→✅ |

---

## ✅ Checklist Final: Aliniere Whitepaper

### Capitolul 2: Arhitectura pe 4 Piloni
- [ ] Pilonul 1: Aplicații (pachete) — ✅ DONE
- [ ] Pilonul 2: Cloud Privat (Proxmox) — ✅ DONE
- [ ] Pilonul 3: Integration Engine (n8n) — FAZA 1
- [ ] Pilonul 4: Blockchain — ✅ DONE (cu fix-uri FAZA 0)

### Capitolul 3: Arhitectura Încrederii
- [ ] Registru Imutabil — ✅ DONE
- [ ] Glass Box — ✅ DONE
- [ ] Descentralizare Faza 1 — FAZA 3
- [ ] Anchoring Polygon — FAZA 3
- [ ] Smart Contracts Comisioane — FAZA 2
- [ ] AI Trust Layer — FAZA 1
- [ ] Specificații DPoS — FAZA 3

### Capitolul 4: Ecosistemul
- [ ] Marketplace — ✅ DONE (extindere FAZA 2)
- [ ] Developer Mode (SSH) — ✅ DONE
- [ ] Concierge Personalizări — FAZA 2
- [ ] Opțiuni Deployment — ✅ DONE
- [ ] Portabilitate — 🔄 (export OK, Merkle proofs FAZA 3)
- [ ] Programul Ambasadori — FAZA 2
- [ ] BYOS — POST-MVP
- [ ] Validatori — FAZA 3

### Capitolul 5: Guvernanța
- [ ] Tipuri propuneri — FAZA 3
- [ ] Quorum diferențiat — FAZA 3
- [ ] Proces vot on-chain — FAZA 3
- [ ] Drept vot per tier — FAZA 3

### Capitolul 8: BiziX AI
- [ ] Local LLMs — FAZA 1
- [ ] Human-in-the-Loop — ✅ UI DONE, backend FAZA 1
- [ ] OCR Facturi — FAZA 1
- [ ] Clasificare Documente — FAZA 1
- [ ] Asistent Conversațional — FAZA 1
- [ ] AI Trust Layer — FAZA 1

### Capitolul 10: Tokenomics
- [ ] Token BIZ — ✅ DONE
- [ ] Burn 20% — FAZA 3
- [ ] Tier System — FAZA 3
- [ ] Staking Complet — FAZA 3
- [ ] Governance Voting — FAZA 3
- [ ] Treasury Buyback — FAZA 3

---

## 🚀 Criterii de Succes

### La Sfârșitul FAZEI 0
- [ ] Zero vulnerabilități critice în blockchain
- [ ] Teste pentru fix-urile de securitate

### La Sfârșitul FAZEI 1
- [ ] Event Bus procesează 100+ evenimente/minut
- [ ] n8n rulează 3 workflow-uri în producție
- [ ] AI poate procesa facturi cu 90%+ acuratețe

### La Sfârșitul FAZEI 2
- [ ] 10+ ambasadori înregistrați
- [ ] 5+ dezvoltatori au accesat SDK
- [ ] Primul dezvoltator extern a publicat o aplicație

### La Sfârșitul FAZEI 3
- [ ] Tokenomics complet funcțional
- [ ] Governance a procesat 5+ propuneri
- [ ] Anchoring Polygon verificabil public

### La Sfârșitul FAZEI 4
- [ ] Test coverage 50%+
- [ ] Zero erori critice în 7 zile de testare
- [ ] Documentație pentru toate funcționalitățile
- [ ] **WHITEPAPER ALIGNMENT: 95%+**

---

## 📝 Note și Decizii de Luat

### Decizii Necesare Înainte de Start

| Decizie | Opțiuni | Impact |
|---------|---------|--------|
| **n8n hosting** | Self-hosted vs. n8n Cloud | Cost vs. mentenanță |
| **AI models** | Local (Ollama) vs. API (Claude/OpenAI) | Latență vs. cost |
| **Polygon network** | Mainnet vs. Amoy testnet | Cost real vs. testare |
| **Payment gateway** | Stripe vs. EuPlătesc extended | Features vs. localizare |

### Riscuri și Mitigări

| Risc | Probabilitate | Impact | Mitigare |
|------|---------------|--------|----------|
| Delay la fix-uri blockchain | Medie | 🔴 Critic | Prioritate absolută săpt 1-2 |
| n8n learning curve | Scăzută | 🟡 Mediu | Documentație bună există |
| AI accuracy sub așteptări | Medie | 🟡 Mediu | Fallback la manual + training |
| Lipsa dezvoltatori externi | Ridicată | 🟡 Mediu | Focus pe ambasadori întâi |

---

*Document generat: 2025-12-07*  
*Următoarea actualizare: După completarea FAZEI 0*


