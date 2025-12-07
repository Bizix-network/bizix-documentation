# BiziX — Documentație Tehnică Centralizată

> **Ultima actualizare:** 2025-12-07  
> **Versiune:** 2.0  
> **Status general:** MVP Funcțional / Early Beta

---

## 📊 Dashboard Stadiu Proiect

### Sumar Executiv

| Metric | Valoare |
|--------|---------|
| **Componente totale** | 9 documentate + 2 în lucru + 2 neînceput |
| **Componente complete (>80%)** | 4 (Blockchain, Backend, Bridge, Portal) |
| **Componente în lucru (50-80%)** | 3 (Website, Wallet, Explorer) |
| **Componente incipiente (<50%)** | 2 (n8n, AI Gateway) |
| **Aliniere cu Whitepaper** | ~60% |
| **Stadiu mediu** | **~75%** |

### 🚦 Status Vizual

```
CORE INFRASTRUCTURE
├── BiziX Chain (Blockchain)     ████████░░ 85%  ⚠️ Securitate
├── Backend Core (Microservices) ███████░░░ 75%  ✅ Funcțional
├── Bridge (Nginx/OIDC)          ████████░░ 85%  ✅ Producție
├── Infrastructure (Proxmox)     ████████░░ 80%  ✅ Funcțional
├── Integration Engine (n8n)     ░░░░░░░░░░  0%  ❌ Neînceput
└── AI Gateway (RAG)             ██░░░░░░░░ 20%  🔄 MVP basic

PORTALE & APLICAȚII
├── Client Portal (Next.js)      ███████░░░ 75%  ✅ Beta
├── Landing Website (Vue)        ██████░░░░ 67%  ✅ Funcțional
├── Wallet                       ██████░░░░ 60%  🔄 De slefuit
├── Explorer                     ██████░░░░ 60%  🔄 De slefuit
├── Ambassador Portal            ░░░░░░░░░░  0%  ❌ Neînceput
└── Developer Portal             ░░░░░░░░░░  0%  ❌ Neînceput
```

---

## 🏗️ Inventar Componente

### Tier 1: Core Infrastructure

| ID | Componentă | Repo/Locație | Stadiu | Probleme Critice | Doc |
|----|------------|--------------|--------|------------------|-----|
| C01 | **BiziX Chain** (Substrate) | bizix-blockchain | ✅ 85% | ⚠️ Securitate: validare origin, ownership | [→](./components/bizix_blockchain.md) |
| C02 | **Backend Core** (Microservices) | bizix-backend-core | ✅ 75% | Teste 35%, MemoryStore | [→](./components/BIZIX_BACKEND_CORE_COMPONENT.md) |
| C03 | **Bridge** (Nginx/OIDC) | bizix-bridge-backend | ✅ 85% | Teste 5% | [→](./components/bizix_bridge.md) |
| C04 | **Integration Engine** (n8n) | - | ❌ 0% | **NEÎNCEPUT** | - |
| C05 | **AI Gateway** (RAG) | - | 🔄 20% | MVP basic, de adaptat | - |
| C06 | **Infrastructure** (Proxmox) | - | ✅ 80% | Documentație parțială | - |

### Tier 2: Portale & Aplicații

| ID | Componentă | Repo/Locație | Stadiu | Probleme | Doc |
|----|------------|--------------|--------|----------|-----|
| P01 | **Client Portal** | bizix-user-portal-new | ✅ 75% | Metrics placeholder, Profile hardcoded | [→](./components/bizix_client_portal.md) |
| P02 | **Landing Website** | bizix-frontend | ✅ 67% | 50-60 strings de externalizat | [→](./components/bizix_website.md) |
| P03 | **Wallet** | - | 🔄 60% | De slefuit | - |
| P04 | **Explorer** | - | 🔄 60% | De slefuit | - |
| P05 | **Ambassador Portal** | - | ❌ 0% | NEÎNCEPUT | - |
| P06 | **Developer Portal** | - | ❌ 0% | NEÎNCEPUT | - |

### Tier 3: Pachete Aplicații

| ID | Pachet | Aplicații | Status |
|----|--------|-----------|--------|
| A01 | Start (Prezență) | WordPress, Matomo, Contact Form | ✅ Template ready |
| A02 | Vinde Online | WordPress + WooCommerce | ✅ Template ready |
| A03 | Servicii & Programări | EasyAppointments, Chatwoot | ✅ Template ready |
| A04 | B2B Simplu | EspoCRM, osTicket | ✅ Template ready |
| A05 | Birou Digital | Nextcloud, Outline | ✅ Template ready |

---

## 🔴 Probleme Critice de Rezolvat

### Securitate (URGENT)

| Componentă | Problema | Severitate | Impact |
|------------|----------|------------|--------|
| **Blockchain** | `reject_proposal` fără validare origin — oricine poate respinge | 🔴 CRITIC | Governance compromis |
| **Blockchain** | `update_company` fără verificare ownership — oricine poate modifica | 🔴 CRITIC | Date corupte |
| **Blockchain** | `TechnicalCommittee = EnsureRoot` — doar sudo controlează | 🟠 HIGH | Centralizare excesivă |
| **Blockchain** | `approval_threshold = 1` hardcodat | 🟡 MEDIUM | O persoană aprobă tot |

### Teste (Risc la Refactoring)

| Componentă | Coverage Actual | Target Recomandat |
|------------|-----------------|-------------------|
| Blockchain | 10% (placeholder) | 60%+ |
| Backend Core | 35% | 60%+ |
| Bridge | 5% | 40%+ |
| Client Portal | 40% | 60%+ |

### Funcționalități Lipsă vs. Whitepaper

| Feature din Whitepaper | Componentă Așteptată | Status |
|------------------------|----------------------|--------|
| Integration Engine (n8n) | C04 | ❌ 0% |
| AI Human-in-the-Loop complet | C05 | 🔄 20% |
| Ambassador Portal | P05 | ❌ 0% |
| Developer Portal | P06 | ❌ 0% |
| Blockchain anchoring public | C01 | ❌ Neimplementat |
| Smart contracts comisioane | C01 | 🔄 Parțial |

---

## 📈 Statistici per Componentă

### BiziX Blockchain (C01)
```
Funcționalități Core:     ████████████████████ 95%
Governance (Treasury):    ████████████████████ 100%
RPC APIs:                 ██████████████████░░ 90%
Teste:                    ██░░░░░░░░░░░░░░░░░░ 10% ⚠️
Benchmarking:             ████████████████░░░░ 80%
Documentație:             ██████████░░░░░░░░░░ 50%
Securitate:               ██████░░░░░░░░░░░░░░ 30% 🔴
```

### Backend Core (C02)
```
Arhitectură Microservicii: ████████████████████ 100%
API Gateway:               ████████████████████ 100%
Auth Keycloak:             ████████████████████ 100%
VM Provisioning:           ████████████████████ 100%
Queue Processing:          ████████████████████ 100%
Teste:                     ███████░░░░░░░░░░░░░ 35%
Email Notifications:       ░░░░░░░░░░░░░░░░░░░░ 0%
```

### Client Portal (P01)
```
Dashboard:                 ████████████████████ 100%
Templates/Marketplace:     ████████████████████ 100%
Apps (VMs) Management:     ████████████████████ 100%
Checkout Flow:             ████████████████████ 100%
Action Center (HITL):      ████████████████████ 100%
Orders:                    ████████████████████ 100%
Profile/Settings:          ████████░░░░░░░░░░░░ 40% (hardcoded)
VM Metrics (Grafana):      ░░░░░░░░░░░░░░░░░░░░ 0% (placeholder)
Payment Integration:       ░░░░░░░░░░░░░░░░░░░░ 0% (placeholder)
```

### Landing Website (P02)
```
Homepage:                  ████████████████████ 100%
Marketplace Page:          ████████████████████ 100%
App Details:               ████████████████████ 100%
Prețuri:                   ████████████████████ 100%
Despre Noi:                ████████████████████ 100%
Contact:                   ████████████████████ 100%
Blog:                      ████████████████████ 100%
Demo Sandbox:              ██████░░░░░░░░░░░░░░ 30% (placeholder)
i18n Strings:              ████████░░░░░░░░░░░░ 40% (50-60 de externalizat)
```

---

## 🔗 Aliniere cu Whitepaper

### Capitolul 2: Arhitectura pe 4 Piloni

| Pilon | Componentă | Status |
|-------|------------|--------|
| **Fundația Operațională** (Aplicații) | A01-A05 | ✅ Template-uri ready |
| **Stratul de Performanță** (Cloud Privat) | C06 + Backend | ✅ Proxmox funcțional |
| **Stratul de Orchestrare** (n8n) | C04 | ❌ NEÎNCEPUT |
| **Stratul de Încredere** (Blockchain) | C01 | ⚠️ 85% cu probleme securitate |

### Capitolul 3: Arhitectura Încrederii

| Feature | Status | Componentă |
|---------|--------|------------|
| Registru Imutabil (hash-uri) | ✅ Implementat | C01 |
| Glass Box (verificabilitate) | 🔄 Parțial | C01, P04 |
| Descentralizare Faza 1 | ❌ Neînceput | C01 |
| Anchoring Polygon | ❌ Neînceput | C01 |
| Smart Contracts Comisioane | 🔄 Parțial | C01 |
| AI Trust Layer | ❌ Neînceput | C05 |

### Capitolul 4: Ecosistemul

| Feature | Status | Componentă |
|---------|--------|------------|
| Marketplace Aplicații | ✅ Implementat | P02, P01 |
| One-Click Deploy | ✅ Implementat | C02 |
| Developer Mode (SSH) | ✅ Implementat | C02, C06 |
| Concierge Personalizări | ❌ Neînceput | - |
| Programul Ambasadori | ❌ Neînceput | P05 |
| BYOS | ❌ Neînceput | C06 |
| Validatori | ❌ Neînceput | C01 |

### Capitolul 8: BiziX AI

| Feature | Status | Componentă |
|---------|--------|------------|
| Local LLMs | 🔄 MVP basic (RAG) | C05 |
| Human-in-the-Loop | ✅ UI implementat | P01 |
| OCR Facturi | ❌ Neînceput | C05 |
| Clasificare Documente | ❌ Neînceput | C05 |
| Asistent Conversațional | 🔄 UI ready, backend lipsă | P01, C05 |

### Capitolul 10: Tokenomics

| Feature | Status | Componentă |
|---------|--------|------------|
| Token BIZ (nativ) | ✅ Implementat | C01 |
| Staking | 🔄 Logică parțială | C01 |
| Governance Voting | 🔄 Parțial (threshold=1) | C01 |
| Burn Mechanism | ❌ Neînceput | C01 |
| Tier System | ❌ Neînceput | C01, P01 |

---

## 🛣️ Roadmap Tehnic Recomandat

### Săptămâna Aceasta (URGENT)
- [ ] 🔴 Fix securitate blockchain: validare origin pentru `reject_proposal`
- [ ] 🔴 Fix securitate blockchain: verificare ownership în `update_company`
- [ ] 🔴 Configurare `TechnicalCommittee` corect în loc de `EnsureRoot`

### Luna Aceasta (HIGH PRIORITY)
- [ ] 🟠 Pornire n8n și primele workflow-uri (contabilitate asistată)
- [ ] 🟠 Upgrade test coverage blockchain la 40%+
- [ ] 🟠 Externalizare strings hardcoded din Website și Portal
- [ ] 🟠 Integrare Grafana pentru metrics VM în Client Portal

### Trimestrul Următor (MEDIUM)
- [ ] 🟡 Ambassador Portal MVP
- [ ] 🟡 Developer Portal MVP
- [ ] 🟡 AI Gateway complet (OCR, clasificare)
- [ ] 🟡 Anchoring Polygon pentru blockchain
- [ ] 🟡 Wallet și Explorer finalizate

---

## 📋 Technical Debt Centralizat

| Componentă | Descriere | Impact | Effort |
|------------|-----------|--------|--------|
| Blockchain | Teste placeholder, nu teste reale | 🔴 High | 16h |
| Blockchain | Weights hardcodate (10_000) | 🟡 Medium | 8h |
| Backend | MemoryStore în loc de Redis pentru sessions | 🟠 High | 4h |
| Backend | Hardcoded Keycloak config în unele servicii | 🟡 Medium | 2h |
| Bridge | Lipsa testelor automatizate | 🟠 Medium | 16h |
| Portal | TODO-uri în auth/callback.ts pentru Sentry | 🟡 Low | 2h |
| Portal | Hardcoded user data în Profile | 🟡 Low | 4h |
| Website | 50-60 strings hardcoded de externalizat | 🟡 Medium | 8h |

---

## 🔒 Stare Securitate

| Componentă | Autentificare | Autorizare | Rate Limit | Audit |
|------------|---------------|------------|------------|-------|
| Blockchain | N/A (on-chain) | ⚠️ PROBLEME | N/A | ❌ |
| Backend | ✅ Keycloak | ✅ RBAC | ✅ | ✅ |
| Bridge | ✅ API Key | ⬜ N/A | ✅ | ❌ |
| Portal | ✅ Keycloak | ✅ Roles | ⬜ (Gateway) | ❌ |
| Website | ⬜ Public | ⬜ N/A | ⬜ | ⬜ |

---

## 📚 Documentație Completitudine

| Componentă | README | API Docs | Diagrame | Comentarii | Tests |
|------------|--------|----------|----------|------------|-------|
| Blockchain | ✅ | 🔄 | ✅ | ✅ | ❌ |
| Backend | ✅ | ✅ Swagger | ✅ | ✅ | 🔄 |
| Bridge | ✅ | ✅ | ❌ | ✅ | ❌ |
| Portal | 🔄 | ❌ | ❌ | ✅ | 🔄 |
| Website | ✅ | N/A | ❌ | ✅ | ❌ |

---

## 🔄 Dependențe între Componente

```
                         ┌─────────────────┐
                         │  BiziX Chain    │
                         │     (C01)       │
                         │   Substrate     │
                         └────────┬────────┘
                                  │ Events
          ┌───────────────────────┼───────────────────────┐
          │                       │                       │
          ▼                       ▼                       ▼
   ┌─────────────┐        ┌─────────────┐        ┌─────────────┐
   │  Backend    │◄──────►│   Bridge    │        │  Wallet     │
   │   Core      │        │   (C03)     │        │   (P03)     │
   │   (C02)     │        │ Nginx/OIDC  │        └─────────────┘
   └──────┬──────┘        └─────────────┘
          │                                       ┌─────────────┐
          │ API                                   │  Explorer   │
          │                                       │   (P04)     │
          ▼                                       └─────────────┘
   ┌─────────────┐
   │   Client    │◄───────────────────────┐
   │   Portal    │                        │
   │   (P01)     │                        │
   └─────────────┘                        │
          ▲                               │
          │                               │
   ┌──────┴──────┐                 ┌─────────────┐
   │   Landing   │                 │    n8n      │ (NEÎNCEPUT)
   │   Website   │                 │   (C04)     │
   │   (P02)     │                 └─────────────┘
   └─────────────┘
                                  ┌─────────────┐
                                  │ AI Gateway  │ (20% - MVP)
                                  │   (C05)     │
                                  └─────────────┘
```

---

## 📅 Milestone-uri MVP → Producție

| Milestone | Target | Componente Critice | Status |
|-----------|--------|-------------------|--------|
| **MVP Alpha** | ✅ Done | C01, C02, C03, P01, P02 | ✅ |
| **Beta Privată** | În curs | + Wallet, Explorer slefuite | 🔄 70% |
| **Beta Publică** | T1 2025 | + C04 (n8n), fix securitate | ❌ |
| **Producție** | T2 2025 | + P05, P06, C05 complet | ❌ |

---

## 📝 Note Importante

### Ce funcționează bine ✅
- Arhitectura microservicii robustă
- Provisioning VM automatizat end-to-end
- SSO Keycloak funcțional
- Multi-tenancy izolat
- Client Portal matur (MVP+)

### Ce necesită atenție 🔴
- Securitate blockchain (origin validation, ownership)
- n8n complet neînceput
- AI doar MVP basic
- Teste insuficiente peste tot

### Decizii de luat 🤔
- Prioritizare: securitate vs. features noi?
- n8n self-hosted vs. cloud?
- AI: modele locale vs. API externe?

---

*Acest document centralizează progresul tehnic BiziX.*  
*Actualizare: 2025-12-07*
