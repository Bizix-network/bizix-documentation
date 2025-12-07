# BiziX Bridge Backend

> **ID:** C01  
> **Ultima actualizare:** 2025-12-06  
> **Responsabil:** BiziX Platform Team  
> **Repository:** `/home/bizix/bizix-bridge-backend`

---

## 1. 📋 Identificare

| Proprietate | Valoare |
|-------------|---------|
| Versiune actuală | v1.2.0 |
| Limbaj(e) | JavaScript (Node.js), Bash |
| Framework-uri | Express.js 4.19.2 |
| Dimensiune cod | ~1800 linii / 15 fișiere principale |
| Licență | ISC |

---

## 2. 🎯 Scop și Funcționalitate

### Ce face această componentă?
Backend API pentru configurarea dinamică a Nginx cu OIDC Proxy (oauth2-proxy + Keycloak). Gestionează automat crearea subdomeniilor `*.bizix.ro`, configurarea reverse proxy-ului, și autentificarea centralizată multi-tenant pentru aplicațiile IMM-urilor din platforma BiziX.

### Ce probleme rezolvă?
- Configurare automată a subdomeniilor pentru fiecare tenant/companie
- Autentificare centralizată SSO cu Keycloak prin oauth2-proxy
- Izolare completă între tenants (fiecare cu serviciu și port separat)
- Management dinamic al configurațiilor Nginx fără intervenție manuală
- Suport pentru 3 tipuri de protecție: full-protected, admin-only, public

### Cine o folosește?
- [x] Altă componentă (care: VM Tenant Backend, BiziX Orchestrator)
- [x] Administrator
- [ ] Utilizator final direct
- [ ] Dezvoltator terț

---

## 3. 🏗️ Arhitectură

### Diagrama Simplificată
```
                    ┌─────────────────────┐
                    │   VM Tenant Backend │
                    │   (BiziX Platform)  │
                    └──────────┬──────────┘
                               │ HTTP API (x-api-key)
                               ▼
                    ┌─────────────────────┐
                    │ BiziX Bridge Backend│
                    │    (Express.js)     │
                    │     Port: 3000      │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│  Keycloak API   │ │ oauth2-proxy    │ │   Nginx         │
│  (Admin API)    │ │ (Multi-Tenant)  │ │ (Reverse Proxy) │
│  Port: 443      │ │ Ports: 4180-4280│ │ Port: 80/443    │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

### Module Interne
| Modul | Scop | Fișiere principale |
|-------|------|--------------------|
| Core API | Server Express și routing | `index.js` |
| Autentificare | Validare API token cu timing-safe compare | `authMiddleware.js` |
| Validare | Sanitizare și validare input-uri | `validationMiddleware.js` |
| Keycloak Admin | Preluare client secrets | `keycloakAdmin.js` |
| OAuth2-Proxy | Management configurații multi-tenant | `oauth2ProxyConfig.js` |
| Port Manager | Alocare porturi per tenant | `portManager.js` |
| Debug Logger | Logging centralizat cu categorii | `debugLogger.js` |

### Dependențe Externe
| Pachet | Versiune | Scop |
|--------|----------|------|
| express | ^4.19.2 | Framework web server |
| axios | ^1.12.2 | HTTP client pentru Keycloak API |
| dotenv | ^16.4.5 | Încărcare variabile de mediu |
| express-rate-limit | ^7.2.0 | Rate limiting per endpoint |

### Servicii Externe
| Serviciu | Tip | Obligatoriu? |
|----------|-----|--------------|
| Keycloak | API (OIDC Provider) | Da (pentru protecție OIDC) |
| Nginx | Reverse Proxy | Da |
| oauth2-proxy | OIDC Authenticator | Da (pentru protecție OIDC) |
| systemd | Service Manager | Da (management servicii) |

---

## 4. 🔌 API / Interfețe

### Endpoints Expuse
| Metodă | Endpoint | Descriere | Auth? |
|--------|----------|-----------|-------|
| GET | /health | Health check public | Nu |
| POST | /configure-nginx | Configurare Nginx cu OIDC | Da (x-api-key) |
| DELETE | /delete-nginx-config | Ștergere configurație | Da (x-api-key) |

### Rate Limiting
| Endpoint | Limite | Window |
|----------|--------|--------|
| Global | 100 cereri/IP | 15 minute |
| POST /configure-nginx | 10 cereri/IP | 15 minute |
| DELETE /delete-nginx-config | 20 cereri/IP | 15 minute |

### Formate Date

**Input POST /configure-nginx:**
```json
{
  "companyName": "example-company",
  "vmIp": "10.0.0.100",
  "vmid": "vm123",
  "protectionType": "full-protected",
  "vmPort": 3000,
  "adminPath": "/admin/",
  "oidcClientId": "vm-tenant-example-1234567890-example.bizix.ro"
}
```

**Output Succes:**
```json
{
  "success": true,
  "message": "Nginx a fost configurat cu OIDC Proxy și repornit cu succes.",
  "domain": "example-company.bizix.ro",
  "protectionType": "full-protected",
  "adminPath": null,
  "oidcClientId": "vm-tenant-example-1234567890-example.bizix.ro"
}
```

**Input DELETE /delete-nginx-config:**
```json
{
  "companyName": "example-company",
  "vmid": "vm123"
}
```

**Output Health Check:**
```json
{
  "status": "healthy",
  "timestamp": "2025-12-06T10:00:00.000Z",
  "uptime": 3600.5,
  "debugMode": false
}
```

---

## 5. 📊 Stadiu de Dezvoltare

### Overview
| Categorie | Procent |
|-----------|---------|
| **TOTAL** | **85%** |
| Funcționalități core | 95% |
| UI/UX | N/A (API only) |
| Teste | 5% |
| Documentație | 90% |
| Securitate | 80% |

### Funcționalități COMPLETE ✅
- [x] Configurare Nginx cu 3 tipuri de protecție (full-protected, admin-only, public)
- [x] Arhitectură multi-tenant pentru oauth2-proxy (izolare completă per tenant)
- [x] Alocare dinamică porturi (4180-4280, max 100 tenants)
- [x] Autentificare API cu timing-safe compare
- [x] Rate limiting pe multiple niveluri
- [x] Validare și sanitizare input-uri (prevenire injection)
- [x] Integrare Keycloak Admin API (preluare client secrets)
- [x] SSO cross-domain cu cookie shared
- [x] Cleanup automat la ștergere (serviciu + config + port)
- [x] Sistem de logging centralizat cu categorii
- [x] Graceful shutdown
- [x] Backward compatibility pentru apeluri vechi

### Funcționalități PARȚIALE 🔄
- [ ] Skip auth routes pentru assets statice — lipsește: configurare dinamică per tenant
- [ ] Monitoring per tenant — lipsește: endpoint dedicat pentru statistici

### Funcționalități PLANIFICATE 📋
- [ ] Endpoint GET pentru listare configurații active — TODO în: roadmap v1.3.0
- [ ] Rotație automată cookie secret — blocat de: prioritizare features
- [ ] Health check pentru fiecare serviciu oauth2-proxy — TODO în: roadmap v1.3.0
- [ ] Webhook-uri pentru notificări la configurare/ștergere

### Bugs Cunoscute 🐛
| ID | Descriere | Severitate | Workaround? |
|----|-----------|------------|-------------|
| - | Niciun bug critic cunoscut | - | - |

### Technical Debt 💳
| Descriere | Impact | Effort fix |
|-----------|--------|------------|
| Lipsa testelor automatizate | Mediu - risc la refactoring | 16 ore |
| Script test în package.json este placeholder | Low - nu blochează nimic | 1 oră |
| Hardcoded path-uri în scripturi | Low - funcționează | 2 ore |

---

## 6. 🧪 Teste și Calitate

### Teste Existente
| Tip | Există? | Coverage | Tool |
|-----|---------|----------|------|
| Unit | ⬜ | 0% | - |
| Integration | ⬜ | 0% | - |
| E2E | ⬜ | 0% | - |
| Manual Scripts | ✅ | - | Bash (test-debug-mode.sh, test-vmPort.sh) |

### Linting & Formatting
- [ ] ESLint / Clippy / etc. configurat
- [ ] Prettier / rustfmt configurat
- [ ] Pre-commit hooks

### CI/CD
- [ ] Pipeline configurat
- [ ] Auto-deploy

---

## 7. 🚀 Deployment

### Cum se face deploy?
```bash
# 1. Instalare dependențe
npm install

# 2. Configurare variabile de mediu (.env)
cat > .env << EOF
API_TOKEN=$(openssl rand -hex 32)
NODE_ENV=production
PORT=3000
HOST=10.2.3.2
DEBUG=false
KEYCLOAK_URL=https://auth.bizix.ro
KEYCLOAK_REALM=bizix
KEYCLOAK_ADMIN_USERNAME=admin
KEYCLOAK_ADMIN_PASSWORD=<secret>
COOKIE_DOMAIN=.bizix.ro
EOF

# 3. Configurare sudoers pentru scripturi
sudo visudo
# Adaugă: bizix ALL=(ALL) NOPASSWD: /home/bizix/bizix-bridge-backend/scripts/*.sh, ...

# 4. Rulare cu process manager (producție)
npm install -g pm2
pm2 start index.js --name bizix-bridge-backend
pm2 save
pm2 startup

# SAU development
npm run dev
```

---

## 8. 🔒 Securitate

### Autentificare / Autorizare
| Aspect | Implementat? | Metodă |
|--------|--------------|--------|
| Autentificare | ✅ | x-api-key header cu timing-safe compare |
| Autorizare (RBAC) | ⬜ | N/A (single service account) |
| Rate limiting | ✅ | express-rate-limit (3 niveluri) |
| IP Blocking | ✅ | 5 tentative eșuate = 15 min lockout |

### Header-uri de Securitate
- ✅ `X-Content-Type-Options: nosniff`
- ✅ `X-Frame-Options: DENY`
- ✅ `X-XSS-Protection: 1; mode=block`
- ✅ `Strict-Transport-Security: max-age=31536000; includeSubDomains`
- ✅ `X-Powered-By` eliminat

### Gestionare Secreturi
- [x] .env (local dev only)
- [ ] Vault / AWS Secrets / etc.
- [ ] Hardcodat (⚠️ PROBLEMĂ) - Nu există

### Vulnerabilități Potențiale Identificate
| Descriere | Severitate | Status |
|-----------------|------------|--------|
| Execuție scripturi Bash cu `sudo` | 🟡 Mediu | Mitigat prin validare strictă input |
| Cookie secret shared între tenants | 🟢 Low | Design intentional pentru SSO |
| Debug mode poate expune date sensibile | 🟡 Mediu | Warning în documentație, sanitizare automată |

### Audit
- [ ] Audit intern efectuat (data: ___)
- [ ] Audit extern efectuat (firma: ___, data: ___)

---

## 9. 📚 Documentație Existentă

| Tip | Există? | Locație | Actualizat? |
|-----|---------|---------|-------------|
| README | ✅ | `./README.md` | ✅ |
| Comentarii cod | ✅ | inline | ✅ |
| API docs | ✅ | `docs/API_MIGRATION_GUIDE.md` | ✅ |
| Diagrame | ⬜ | - | - |
| CHANGELOG | ✅ | `./CHANGELOG.md` | ✅ |
| Quick Start | ✅ | `docs/QUICK_START.md` | ✅ |
| OIDC Setup | ✅ | `docs/OIDC_SETUP_GUIDE.md` | ✅ |
| Multi-Tenant Arch | ✅ | `./MULTI_TENANT_ARCHITECTURE.md` | ✅ |
| Debugging | ✅ | `./DEBUG_MODE.md`, `./OIDC_DEBUGGING_GUIDE.md` | ✅ |

---

## 10. 🎯 Aliniere cu Whitepaper

### Features din Whitepaper care TREBUIE implementate aici

| Secțiune WP | Feature | Status | Note |
|-------------|---------|--------|------|
| BPaaS | Multi-tenancy | ✅ | Izolare completă per tenant |
| BPaaS | Autentificare centralizată | ✅ | SSO cu Keycloak |
| BPaaS | Provisionare automată | ✅ | Configurare Nginx + oauth2-proxy |
| BPaaS | Self-service subdomain | ✅ | *.bizix.ro dinamic |

### Ce implementează EXTRA (nu e în WP)
- Rate limiting avansat cu IP blocking
- Debug logging cu categorii și sanitizare
- Skip auth routes pentru assets statice
- Backward compatibility pentru API vechi

### Ce LIPSEȘTE vs. Whitepaper
- Integrare blockchain pentru audit trail (planificat)
- Facturare automată per tenant

---

## 11. 🔗 Dependențe & Integrări

### Depinde de (această componentă are nevoie de):
| Componentă | Obligatorie? | Pentru ce? |
|------------|--------------|------------|
| Keycloak | Da | OIDC Provider, Admin API pentru secrets |
| Nginx | Da | Reverse proxy pentru aplicații |
| oauth2-proxy | Da | Autentificare OIDC |
| systemd | Da | Management servicii per-tenant |
| Let's Encrypt | Da | Certificate SSL wildcard |

### Depind de ea (alte componente care au nevoie):
| Componentă | Pentru ce? |
|------------|------------|
| VM Tenant Backend | Configurare automată Nginx la creare tenant |
| BiziX Orchestrator | Provisioning complet al aplicațiilor |

---

## 12. 📝 Note & Istoric

### Decizii Importante
| Data | Decizie | Rațional |
|------|---------|----------|
| 2025-01-17 | Arhitectură multi-tenant oauth2-proxy | Evitarea suprascrierilor și izolare completă |
| 2025-10-07 | Adăugare parametru vmPort | Suport pentru aplicații pe porturi non-standard |
| 2025-10-01 | Backward compatibility protectionType | Migrare graduală fără breaking changes |

### Changelog Recent
| Data | Versiune | Modificări |
|------|----------|------------|
| 2025-01-17 | v1.2.0 | Multi-tenant architecture, port manager, auto-detect old service |
| 2025-10-07 | v1.1.0 | Parametru vmPort, skip auth routes |
| 2025-10-07 | v1.0.1 | Debug logging system |
| 2025-10-01 | v1.0.0 | OIDC Proxy integration, 3 tipuri protecție |

---

## 13. 🎬 Next Steps

### Imediat (această săptămână)
- [ ] Adăugare teste unitare pentru validationMiddleware.js
- [ ] Configurare ESLint

### Curând (luna aceasta)
- [ ] Endpoint GET pentru listare configurații active
- [ ] Health check individual per serviciu oauth2-proxy
- [ ] Implementare Jest pentru teste automatizate

### Viitor (backlog)
- [ ] Integrare cu sistem de monitoring (Prometheus metrics)
- [ ] Rotație automată cookie secret
- [ ] Webhook-uri pentru notificări
- [ ] Dashboard admin pentru vizualizare tenants

---

*Generat cu template BiziX v1.0 - 2025-12-06*

