# BiziX Backend Core

> **ID:** C01  
> **Ultima actualizare:** 2025-12-06  
> **Responsabil:** Razvan Popa (@inodelisia)  
> **Repository:** `https://github.com/Bizix-network/bizix-backend-core`

---

## 1. 📋 Identificare

| Proprietate | Valoare |
|-------------|---------|
| Versiune actuală | v0.0.3 |
| Limbaj(e) | JavaScript (Node.js) |
| Framework-uri | Express.js, Mongoose, BullMQ |
| Dimensiune cod | ~15,000+ linii / 200+ fișiere |
| Licență | Proprietar BiziX |

---

## 2. 🎯 Scop și Funcționalitate

### Ce face această componentă?
BiziX Backend Core este coloana vertebrală tehnică a platformei BiziX, oferind provizionare automată de VM-uri pre-configurate cu aplicații SaaS (WordPress, Chatwoot, EspoCRM, etc.) în 2-3 minute. Transformă comenzile utilizatorilor în mașini virtuale funcționale, complet configurate cu DNS, reverse proxy și SSO.

### Ce probleme rezolvă?
- Automatizarea completă a ciclului de viață al VM-urilor (creare, configurare, monitorizare, ștergere)
- Integrarea seamless între multiple servicii externe (Proxmox, Keycloak, Cloudflare, EuPlătesc)
- Procesarea asincronă a operațiunilor lungi prin queue-based architecture
- Management centralizat al autentificării și autorizării prin OIDC/OAuth2
- Audit trail complet pentru toate operațiunile critice

### Cine o folosește?
- [x] Utilizator final direct (prin frontend)
- [x] Altă componentă (care: Frontend Vue.js, Admin Panel, Bizix Bridge)
- [x] Administrator (prin bizixctl CLI)
- [ ] Dezvoltator terț

---

## 3. 🏗️ Arhitectură

### Diagrama Simplificată
```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT (Frontend)                         │
└─────────────────────────┬───────────────────────────────────┘
                          │
                          ▼
┌─────────────────────────────────────────────────────────────┐
│              API GATEWAY (Port 5000)                         │
│  • Rutare requests • Validare JWT (Keycloak)                │
│  • Rate limiting • CORS • OpenAPI Validation                │
│  • Bot Protection • IP Blocking • Audit Trail               │
└──────┬──────┬──────┬──────┬──────┬──────┬───────────────────┘
       │      │      │      │      │      │
       ▼      ▼      ▼      ▼      ▼      ▼
    ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐ ┌────┐
    │5001│ │5002│ │5003│ │5004│ │5005│ │5006│
    │AUTH│ │TMPL│ │ VM │ │ORDR│ │INFR│ │BLCK│
    └─┬──┘ └─┬──┘ └─┬──┘ └─┬──┘ └─┬──┘ └─┬──┘
      │      │      │      │      │      │
      └──────┴──────┴───┬──┴──────┴──────┘
                        │
            ┌───────────┼───────────┐
            │           │           │
            ▼           ▼           ▼
      ┌─────────┐ ┌─────────┐ ┌─────────┐
      │ MongoDB │ │  Redis  │ │ BullMQ  │
      │(Database│ │ (Cache) │ │ (Queue) │
      └─────────┘ └─────────┘ └─────────┘
```

### Module Interne

| Modul | Scop | Fișiere principale |
|-------|------|--------------------|
| API Gateway | Punct unic de intrare, autentificare, routing | `api-gateway/server.js`, `api-gateway/middleware/` |
| Auth Service | Profile utilizatori, OIDC client management | `services/auth-service/` |
| Template Service | Catalog template-uri VM active | `services/template-service/` |
| VM Service | Lifecycle VM + Worker BullMQ | `services/vm-service/` |
| Order Service | Comenzi & plăți EuPlătesc | `services/order-service/` |
| Infrastructure Service | Proxmox, Nginx, DNS, Cloud-Init | `services/infrastructure-service/` |
| Blockchain Service | Listener evenimente Substrate | `services/blockchain-service/` |
| Shared Components | Logger, BullMQ, Response Helpers | `shared/` |

### Dependențe Externe

| Pachet | Versiune | Scop |
|--------|----------|------|
| express | ^4.19.2 | Framework web server |
| mongoose | ^8.3.5 | ODM pentru MongoDB |
| bullmq | ^5.56.0 | Job queue pentru operațiuni async |
| ioredis | ^5.6.1 | Client Redis pentru BullMQ |
| keycloak-connect | ^26.1.1 | Integrare Keycloak OIDC |
| @keycloak/keycloak-admin-client | ^26.3.2 | Admin API Keycloak |
| proxmox-api | ^1.1.0 | Client Proxmox VE API |
| @polkadot/api | ^10.11.2 | Client Substrate blockchain |
| axios | ^1.6.8 | HTTP client |
| helmet | ^8.1.0 | Security headers |
| express-openapi-validator | ^5.1.6 | OpenAPI request/response validation |
| swagger-ui-express | ^5.0.0 | Documentație API interactivă |
| pino | ^9.1.0 | Structured logging |
| bcryptjs | ^2.4.3 | Password hashing |
| jsonwebtoken | ^9.0.2 | JWT handling |

### Servicii Externe

| Serviciu | Tip | Obligatoriu? |
|----------|-----|--------------|
| MongoDB | Database | Da |
| Redis | Cache/Queue backend | Da |
| Keycloak (auth.bizix.ro) | Identity Provider OIDC | Da |
| Proxmox VE | Hypervisor | Da |
| Cloudflare | DNS management | Da |
| EuPlătesc | Payment gateway | Nu (bypass available) |
| Substrate Blockchain | Audit trail | Nu |
| Bizix Bridge | Nginx + OIDC config | Da |

---

## 4. 🔌 API / Interfețe

### Endpoints Expuse (API Gateway - Port 5000)

#### Rute Publice
| Metodă | Endpoint | Descriere | Auth? |
|--------|----------|-----------|-------|
| GET | /health | Health check gateway | Nu |
| GET | /health/services | Health check toate serviciile | Nu |
| GET | /api/templates | Lista template-uri active | Nu |
| GET | /api/templates/:id | Detalii template | Nu |
| GET | /api-docs | Swagger UI Documentation | Nu |

#### Rute Protejate (necesită Bearer Token)
| Metodă | Endpoint | Descriere | Auth? |
|--------|----------|-----------|-------|
| POST | /auth/first-login | Sincronizare după login Keycloak | Da |
| POST | /auth/logout | Logout utilizator | Da |
| GET | /api/users/profile | Profil utilizator curent | Da |
| PUT | /api/users/profile | Update profil | Da |
| GET | /api/user | ID utilizator curent | Da |
| GET | /api/vms | Lista VM-uri utilizator | Da |
| GET | /api/vms/:id | Detalii VM specific | Da |
| POST | /api/vms/:id/start | Pornire VM | Da |
| POST | /api/vms/:id/stop | Oprire VM | Da |
| POST | /api/vms/:id/restart | Restart VM | Da |
| GET | /api/orders | Istoric comenzi | Da |
| POST | /api/orders/create-order | Creare comandă nouă | Da |

#### Rute Interne (necesită Internal-API-Key + IP whitelist)
| Metodă | Endpoint | Descriere | Auth? |
|--------|----------|-----------|-------|
| POST | /proxmox/create-vm | Creare VM completă | Internal |
| GET | /proxmox/debug/vms | Lista toate VM-urile | Internal |
| GET | /proxmox/debug/sync-status | Status sincronizare | Internal |
| POST | /proxmox/debug/cleanup-orphaned | Cleanup VM orphaned | Internal |
| POST | /api/webhook/webhook | Webhook EuPlătesc | Internal |
| POST | /internal/auth/oidc-client | Creare client OIDC | Internal |
| GET | /admin/security/blocked-ips | Lista IP-uri blocate | Internal |
| GET | /admin/audit/logs | Audit logs | Internal |

### Evenimente Emise (BullMQ)
| Eveniment | Payload | Când? |
|-----------|---------|-------|
| vm-creation:create-vm-for-order | `{userId, node, vmName, vmVersion, orderId}` | La creare comandă |
| order-processing:* | `{orderId, status, ...}` | La procesare comandă |

### Evenimente Consumate
| Eveniment | De la | Reacție |
|-----------|-------|---------|
| ProposalApproved | Substrate Blockchain | Update template status |
| Payment Webhook | EuPlătesc | Trigger VM creation |

### Formate Date
**Input (Create Order):**
```json
{
  "templateId": 9000,
  "vmName": "company-wordpress",
  "vmVersion": "latest",
  "billingDetails": {
    "companyName": "MyCompany",
    "cui": "RO12345678",
    "address": "..."
  },
  "amount": 10.00,
  "currency": "RON"
}
```

**Output (VM Created):**
```json
{
  "success": true,
  "data": {
    "vmid": 2001,
    "ipAddress": "10.2.3.100",
    "publicURL": "http://mycompany.bizix.ro/",
    "domain": "mycompany.apps.bizix.ro",
    "metadataAccessToken": "abc123...",
    "estimatedTime": "2-3 minutes"
  },
  "requestId": "uuid-v4",
  "timestamp": "2025-12-06T10:00:00Z"
}
```

---

## 5. 📊 Stadiu de Dezvoltare

### Overview
| Categorie | Procent |
|-----------|---------|
| **TOTAL** | **75%** |
| Funcționalități core | 90% |
| UI/UX (Admin) | 40% |
| Teste | 35% |
| Documentație | 80% |
| Securitate | 85% |

### Funcționalități COMPLETE ✅
- [x] Arhitectură microservicii funcțională
- [x] Autentificare Keycloak OIDC
- [x] Crearea VM-urilor via Proxmox
- [x] Configurare automată Cloud-Init
- [x] Configurare DNS Cloudflare
- [x] Configurare Nginx reverse proxy
- [x] Protecție OIDC pentru aplicații (admin-only, full-site)
- [x] Queue-based processing cu BullMQ
- [x] Rate limiting și bot protection
- [x] Audit trail pentru operațiuni sensibile
- [x] OpenAPI validation și Swagger UI
- [x] Structured logging cu Pino
- [x] Health checks pentru toate serviciile
- [x] Graceful shutdown

### Funcționalități PARȚIALE 🔄
- [ ] E2E Testing — lipsește: coverage complet
- [ ] Admin Dashboard — lipsește: UI frontend
- [ ] Monitoring Dashboard — lipsește: Grafana integration complet
- [ ] Email Notifications — lipsește: implementare SendGrid/SMTP

### Funcționalități PLANIFICATE 📋
- [ ] Multi-tenancy complet — TODO în: next major release
- [ ] AI-powered recommendations — TODO în: Q1 2026
- [ ] Marketplace templates terți — blocat de: legal agreements
- [ ] Commission payments blockchain — blocat de: smart contract deployment

### Bugs Cunoscute 🐛
| ID | Descriere | Severitate | Workaround? |
|----|-----------|------------|-------------|
| #1 | Cloud-init snippets necesită SSH access | 🟡 Medium | Folosește description fallback |
| #2 | Sync inconsistencies între MongoDB și Proxmox | 🟡 Medium | Debug endpoints + manual cleanup |

### Technical Debt 💳
| Descriere | Impact | Effort fix |
|-----------|--------|------------|
| MemoryStore pentru sessions în loc de Redis | High memory pe scale | 4 ore |
| Hardcoded Keycloak config în unele servicii | Dificil de schimbat | 2 ore |
| TODO în blockchain service pentru Template API | Minor | 1 oră |
| Unele secreturi loggate în dev mode | Security în dev | 2 ore |

---

## 6. 🧪 Teste și Calitate

### Teste Existente
| Tip | Există? | Coverage | Tool |
|-----|---------|----------|------|
| Unit | ✅ | ~35% | Jest |
| Integration | ✅ | ~15% | Jest + Supertest |
| E2E | ⬜ | 0% | - |

### Fișiere de Test Identificate (10 total)
```
api-gateway/tests/unit/middleware/cache.test.js
api-gateway/tests/unit/middleware/errorHandler.test.js
api-gateway/tests/unit/middleware/rateLimiter.test.js
api-gateway/tests/unit/utils/openApiValidation.test.js
api-gateway/tests/integration/health.test.js
services/auth-service/tests/internal.test.js
services/auth-service/tests/signup.test.js
services/auth-service/tests/auth.oauth.test.js
services/order-service/tests/unit/models/Order.test.js
services/vm-service/tests/unit/models/VM.test.js
```

### Linting & Formatting
- [ ] ESLint / Clippy / etc. configurat - Nu găsit .eslintrc
- [ ] Prettier / rustfmt configurat - Nu găsit .prettierrc
- [ ] Pre-commit hooks - Nu găsit husky config

### CI/CD
- [ ] Pipeline configurat - Necesită verificare repo
- [ ] Auto-deploy - PM2 ecosystem.config.js pentru deploy manual

---

## 7. 🚀 Deployment

### Cum se face deploy?
```bash
# 1. Clone și install
git clone https://github.com/Bizix-network/bizix-backend-core.git
cd bizix-backend-core
npm run install:all

# 2. Configurare environment
cp env.microservices.example .env
# Editează .env cu configurări reale

# 3. Start cu PM2 (producție)
npm run pm2:start

# 4. Sau development cu tmux
npm run tmux:dev

# 5. Monitorizare
npm run pm2:logs
npm run pm2:monit
```

### Docker (Development Stack)
```bash
# MongoDB, Redis, Keycloak
docker-compose -f docker/development/docker-compose.yml up -d

# Observability (Grafana, Loki, Promtail)
docker-compose -f docker/observability/docker-compose.yml up -d
```

---

## 8. 🔒 Securitate

### Autentificare / Autorizare
| Aspect | Implementat? | Metodă |
|--------|--------------|--------|
| Autentificare | ✅ | JWT via Keycloak (bearer-only) |
| Autorizare (RBAC) | ✅ | Keycloak roles |
| Rate limiting | ✅ | express-rate-limit (general, critical, login) |
| Bot Protection | ✅ | Custom middleware |
| IP Blocking | ✅ | Dynamic IP blocking |

### Gestionare Secreturi
- [x] .env (local dev only)
- [ ] Vault / AWS Secrets / etc. — Recomandat pentru producție
- [ ] Hardcodat (⚠️ PROBLEMĂ) — Există fallback defaults

### Vulnerabilități Cunoscute
| CVE / Descriere | Severitate | Status |
|-----------------|------------|--------|
| Session MemoryStore în loc de Redis | 🟡 Medium | TODO |
| Debug logs pot expune date în dev | 🟢 Low | Documented |

### Măsuri de Securitate Implementate
- ✅ Helmet pentru security headers (HSTS, CSP, X-Frame-Options)
- ✅ CORS configurabil
- ✅ Input validation via OpenAPI Validator
- ✅ Internal API Key pentru comunicare între servicii
- ✅ IP whitelist pentru rute interne
- ✅ Audit trail pentru operațiuni sensibile
- ✅ Request ID tracking pentru debugging
- ✅ Graceful error handling (nu expune stack traces în prod)

### Audit
- [x] Audit intern efectuat (data: Octombrie 2025 - OIDC Migration)
- [ ] Audit extern efectuat (firma: ___, data: ___)

---

## 9. 📚 Documentație Existentă

| Tip | Există? | Locație | Actualizat? |
|-----|---------|---------|-------------|
| README | ✅ | `README.md` | ✅ |
| Comentarii cod | ✅ | inline | ✅ |
| API docs (Swagger) | ✅ | `/api-docs`, `api-gateway/openapi/` | ✅ |
| Diagrame | ✅ | `docs/diagrams/` | ✅ |
| Architecture Summary | ✅ | `ARCHITECTURE_SUMMARY.md` | ✅ |
| Implementation Summary | ✅ | `IMPLEMENTATION_SUMMARY.md` | ✅ |
| Changelog | ✅ | `CHANGELOG.md` | ✅ |
| Guides | ✅ | `docs/guides/` | ✅ |

---

## 10. 🎯 Aliniere cu Whitepaper

### Features din Whitepaper care TREBUIE implementate aici


### Ce implementează EXTRA (nu e în WP)


### Ce LIPSEȘTE vs. Whitepaper


---

## 11. 🔗 Dependențe & Integrări

### Depinde de (această componentă are nevoie de):
| Componentă | Obligatorie? | Pentru ce? |
|------------|--------------|------------|
| MongoDB | Da | Persistență date |
| Redis | Da | BullMQ queue backend |
| Keycloak | Da | Autentificare/Autorizare |
| Proxmox VE | Da | Hypervisor pentru VM-uri |
| Cloudflare | Da | DNS management |
| Bizix Bridge | Da | Nginx OIDC configuration |
| EuPlătesc | Nu | Payment processing (bypass available) |
| Substrate Node | Nu | Blockchain audit trail |

### Depind de ea (alte componente care au nevoie):
| Componentă | Pentru ce? |
|------------|------------|
| Frontend (Vue.js) | Toate API calls |
| Admin Dashboard | Management VM-uri și utilizatori |
| bizixctl CLI | Operațiuni administrative |
| Monitoring Dashboard | Health și metrics |

---

## 12. 📝 Note & Istoric

### Decizii Importante
| Data | Decizie | Rațional |
|------|---------|----------|
| Oct 2025 | Migrare la Keycloak-only auth | Simplificare, security îmbunătățită |
| Oct 2025 | Adăugare OIDC Protection pentru Nginx | SSO pentru toate aplicațiile |
| Oct 2025 | BullMQ în loc de procesare sincronă | Scalabilitate, reliability |
| Oct 2025 | OpenAPI Validation | Contract-first development |

### Changelog Recent
| Data | Versiune | Modificări |
|------|----------|------------|
| Oct 2025 | v0.0.3 | Nginx OIDC Protection integration |
| Oct 2025 | v0.0.3 | OpenAPI Validation + Swagger UI |
| Oct 2025 | v0.0.3 | Audit Trail middleware |
| Oct 2025 | v0.0.3 | Bot Protection + IP Blocking |

---

## 13. 🎬 Next Steps

### Imediat (această săptămână)
- [ ] Adăugare E2E tests pentru fluxul de creare VM
- [ ] Fix pentru Cloud-init snippets SSH

### Curând (luna aceasta)
- [ ] Integrare completă Grafana monitoring
- [ ] Email notifications pentru comenzi
- [ ] Upgrade test coverage la 60%

### Viitor (backlog)
- [ ] AI-powered template recommendations
- [ ] Multi-tenancy complet
- [ ] Marketplace pentru template-uri terțe
- [ ] Commission payments via smart contracts

---

## 14. 🔍 TODO-uri și FIXME-uri Identificate

### Din cod (selecție):
```
services/blockchain-service/blockchainListener.js:2
// TODO: Blockchain service should use Template from template-service API

services/infrastructure-service/routes.js:922
// Endpoint pentru debugging - lista VM-urilor

Multiple files:
// [DEBUG] logs throughout - clean up for production
```

### Recomandări:
1. **HIGH**: Înlocuire MemoryStore cu Redis pentru sessions
2. **MEDIUM**: Cleanup debug endpoints în producție
3. **MEDIUM**: Adăugare ESLint/Prettier pentru consistență cod
4. **LOW**: Documentare completă pentru toate debug endpoints

---

*Generat cu template BiziX v1.0*  
*Document generat automat: 2025-12-06*

