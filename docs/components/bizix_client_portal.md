# BiziX Client Portal

> **ID:** P01-CLIENT  
> **Ultima actualizare:** 2025-12-06  
> **Responsabil:** BiziX Team  
> **Repository:** `bizix-user-portal-new`

---

## 1. 📋 Identificare

| Proprietate | Valoare |
|-------------|---------|
| Versiune actuală | v0.1.0 |
| Limbaj(e) | TypeScript, JavaScript |
| Framework-uri | Next.js 15.3.4, React 19.1.0, TailwindCSS 4.x |
| Dimensiune cod | ~400+ fișiere TS/TSX |
| Licență | Private |

---

## 2. 🎯 Scop și Funcționalitate

### Ce face această componentă?
BiziX Client Portal este interfața web principală pentru utilizatorii BiziX, permițând gestionarea aplicațiilor cloud (VMs), plasarea comenzilor pe baza template-urilor preconfigurate și monitorizarea resurselor. Include funcționalități de Human-in-the-Loop (HITL) pentru aprobare acțiuni automatizate.

### Ce probleme rezolvă?
- Gestionarea centralizată a aplicațiilor/VMs (start/stop/restart)
- Checkout flow pentru achiziționarea de aplicații din catalog
- Monitorizare stare servicii și blockchain trust
- Action Center pentru aprobare/respingere acțiuni automatizate
- Autentificare SSO prin Keycloak

### Cine o folosește?
- [x] Utilizator final direct (client BiziX)
- [x] Administrator (parțial - vizualizare)
- [ ] Altă componentă
- [ ] Dezvoltator terț

---

## 3. 🏗️ Arhitectură

### Diagrama Simplificată
```
┌─────────────────────────────────────────────────────────────────┐
│                      Next.js 15 App Router                      │
├─────────────────────────────────────────────────────────────────┤
│  app/                                                           │
│  ├── (dashboard)/        ← Layout protejat cu sidebar           │
│  │   ├── page.tsx        ← Dashboard principal                  │
│  │   ├── apps/           ← VMs Management                       │
│  │   ├── orders/         ← Istoric comenzi                      │
│  │   ├── templates/      ← Catalog șabloane                     │
│  │   ├── action-center/  ← HITL approvals                       │
│  │   └── profile/        ← Setări profil                        │
│  ├── [locale]/           ← i18n wrapper (ro/en)                 │
│  │   └── (dashboard)/    ← Re-export locale-aware               │
│  └── api/                                                       │
│      ├── auth/           ← Keycloak OAuth flow                  │
│      ├── bff/            ← Backend-For-Frontend proxy           │
│      └── flow/           ← Checkout/payment orchestration       │
├─────────────────────────────────────────────────────────────────┤
│  components/ui/          ← Design System (60+ componente)       │
│  hooks/                  ← Custom hooks (auth, bff, csrf)       │
│  src/hooks/              ← TanStack Query hooks                 │
│  src/lib/fetcher/        ← API client cu sanitizare             │
│  lib/query/              ← React Query provider                 │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
              ┌───────────────────────────────────┐
              │  BFF (Backend-For-Frontend)       │
              │  /api/bff/* → Gateway Backend     │
              └───────────────────────────────────┘
```

### Module Interne
| Modul | Scop | Fișiere principale |
|-------|------|--------------------|
| Auth | Keycloak OAuth2/OIDC | `app/api/auth/`, `hooks/use-auth.ts`, `components/auth/` |
| BFF Client | Proxy API cu token refresh | `lib/api/client.ts`, `src/lib/fetcher/` |
| TanStack Query | State management server | `src/lib/query/`, `src/hooks/use-*.ts` |
| UI Components | Design System Radix-based | `components/ui/` (60+ componente) |
| Layout | Shell aplicație | `components/layouts/layout/` |
| Dashboard | KPI, VMs, Health | `components/dashboard/` |
| i18n | Localizare | `messages/`, `i18n.ts`, `next-intl` |

### Dependențe Externe
| Pachet | Versiune | Scop |
|--------|----------|------|
| next | 15.3.4 | Framework React |
| react | 19.1.0 | UI Library |
| @tanstack/react-query | 5.85.3 | Server state |
| next-intl | 3.26.5 | Internationalizare |
| keycloak-js | 26.2.0 | Auth client |
| zod | 3.25.76 | Validare schema |
| xstate | 5.20.2 | State machines (provisioning) |
| radix-ui | 1.4.2 | UI primitives |
| recharts | 2.15.1 | Charts |
| @sentry/nextjs | 10.5.0 | Error tracking |
| @polkadot/api | 14.3.1 | Blockchain integration |

### Servicii Externe
| Serviciu | Tip | Obligatoriu? |
|----------|-----|--------------|
| Keycloak | Auth IDP | Da |
| BiziX Gateway API | REST API | Da |
| Sentry | Error Monitoring | Nu (recomandat) |
| Polkadot Node | Blockchain | Nu (pentru trust) |

---

## 4. 🔌 API / Interfețe

### Endpoints BFF Expuse (`/api/bff/*`)
| Metodă | Endpoint | Descriere | Auth? |
|--------|----------|-----------|-------|
| GET | `/api/bff/templates` | Lista template-uri | Da |
| GET | `/api/bff/templates/:id` | Detalii template | Da |
| GET | `/api/bff/vms` | Lista VMs utilizator | Da |
| GET | `/api/bff/vms/:id` | Detalii VM | Da |
| POST | `/api/bff/vms/:id/start` | Pornire VM | Da |
| POST | `/api/bff/vms/:id/stop` | Oprire VM | Da |
| POST | `/api/bff/vms/:id/restart` | Restart VM | Da |
| GET | `/api/bff/orders` | Istoric comenzi | Da |
| POST | `/api/bff/orders/create-order` | Creare comandă | Da |
| GET | `/api/bff/actions` | Lista acțiuni HITL | Da |
| POST | `/api/bff/actions/:id/approve` | Aprobare acțiune | Da |
| POST | `/api/bff/actions/:id/reject` | Respingere acțiune | Da |
| GET | `/api/bff/users/profile` | Profil utilizator | Da |
| PUT | `/api/bff/users/profile` | Update preferințe | Da |
| GET | `/api/bff/notifications` | Notificări | Da |
| GET | `/api/bff/anchors/:type/:id` | Blockchain anchor | Da |

### Endpoints Auth (`/api/auth/*`)
| Metodă | Endpoint | Descriere |
|--------|----------|-----------|
| GET | `/api/auth/login` | Redirect Keycloak |
| GET | `/api/auth/callback` | OAuth callback |
| POST | `/api/auth/logout` | Logout |
| GET | `/api/auth/session` | Sesiune curentă |
| POST | `/api/auth/refresh` | Token refresh |
| GET | `/api/auth/me` | User info |

### Evenimente Emise
| Eveniment | Payload | Când? |
|-----------|---------|-------|
| `auth:refresh` | — | După token refresh |
| `auth:logout` | — | La logout/401 |

---

## 5. 📊 Stadiu de Dezvoltare

### Overview
| Categorie | Procent |
|-----------|---------|
| **TOTAL** | **~75%** |
| Funcționalități core | 85% |
| UI/UX | 80% |
| Teste | 40% |
| Documentație | 30% |
| Securitate | 70% |

### Pagini & Funcționalități COMPLETE ✅

| Pagină | Route | Status | Features |
|--------|-------|--------|----------|
| **Dashboard** | `/` | ✅ Complet | KPI cards, My VMs widget, Blockchain Trust, Health Strip |
| **Templates** | `/templates` | ✅ Complet | Grid/List view, căutare, filtre categorie, sortare, paginare, Quick View |
| **Template Detail** | `/templates/[id]` | ✅ Complet | Specificații, CTA creare VM |
| **Apps (VMs)** | `/apps` | ✅ Complet | Grid/List, status badges, acțiuni (start/stop/restart), paginare |
| **VM Detail** | `/apps/[id]` | 🔄 Parțial | Detalii VM, placeholder metrics (Grafana TBD) |
| **Orders** | `/orders` | ✅ Complet | Tabel istoric, filtre status/dată, export CSV, paginare |
| **Action Center** | `/action-center` | ✅ Complet | HITL table, approve/reject, auto-approve toggle, Provenance chip |
| **Checkout** | `/checkout` | ✅ Complet | Stepper 2 pași, validare Zod, XState provisioning |
| **Processing** | `/processing` | ✅ Complet | Status tracking comandă |
| **Profile** | `/profile` | 🔄 Parțial | Preferences (hardcoded email/name) |
| **Settings** | `/settings` | 🔄 Parțial | Display preferences (readonly) |
| **Security** | `/security` | 🔄 Parțial | Sessions list (placeholder IP) |

### Funcționalități PARȚIALE 🔄
- [ ] **VM Detail Metrics** — lipsește: integrare Grafana
- [ ] **Profile Account Info** — lipsește: fetch real din Keycloak
- [ ] **Settings** — lipsește: funcționalitate save reală
- [ ] **Security Sessions** — lipsește: IP real, logout session

### Funcționalități PLANIFICATE 📋
- [ ] **Payment Integration** — TODO în: `/checkout/payment` (placeholder PSP)
- [ ] **Notifications real-time** — TODO în: WebSocket/SSE
- [ ] **AI Chat Drawer** — parțial implementat, necesită backend AI

### Bugs Cunoscute 🐛
| ID | Descriere | Severitate | Workaround? |
|----|-----------|------------|-------------|
| #1 | Metrics VM sunt placeholder | 🟡 | Vizualizare în Grafana direct |
| #2 | IP în Security page hardcodat | 🟢 | — |

### Technical Debt 💳
| Descriere | Impact | Effort fix |
|-----------|--------|------------|
| TODO-uri în auth/callback.ts pentru Sentry | Mediu | 2 ore |
| Hardcoded user data în Profile | Mic | 4 ore |
| Checkout persistență localStorage dezactivată | Mic | 2 ore |

---

## 6. 🧪 Teste și Calitate

### Teste Existente
| Tip | Există? | Coverage | Tool |
|-----|---------|----------|------|
| Unit | ✅ | ~40% | Jest, Testing Library |
| Integration | ✅ | ~20% | MSW (Mock Service Worker) |
| E2E | ✅ | ~10% | Playwright |

### Linting & Formatting
- [x] ESLint configurat (incluzând jsx-a11y, security)
- [x] Prettier configurat
- [x] Pre-commit hooks (Husky + lint-staged)

### CI/CD
- [x] Pipeline configurat (GitHub Actions implied)
- [x] PM2 ecosystem pentru producție

---

## 7. 🚀 Deployment

### Cum se face deploy?
```bash
# Development
pnpm dev

# Build pentru producție
pnpm build:production

# Start producție
pnpm start

# PM2 producție
pnpm pm2:start
```

### Environment Variables Required
- `NEXT_PUBLIC_KEYCLOAK_URL`
- `NEXT_PUBLIC_KEYCLOAK_REALM`
- `NEXT_PUBLIC_KEYCLOAK_CLIENT_ID`
- `BFF_API_URL` (Gateway backend)
- `SESSION_SECRET`

---

## 8. 🔒 Securitate

### Autentificare / Autorizare
| Aspect | Implementat? | Metodă |
|--------|--------------|--------|
| Autentificare | ✅ | Keycloak OIDC |
| Autorizare (RBAC) | ✅ | Roles din token |
| Rate limiting | ⬜ | TBD (Gateway) |
| CSRF Protection | ✅ | Custom hook |

### Gestionare Secreturi
- [x] .env (local dev only)
- [ ] Vault / AWS Secrets
- [ ] ~~Hardcodat~~ ⚠️ (nu în producție)

### ESLint Security Rules
- `security/detect-eval-with-expression`: error
- `security/detect-unsafe-regex`: error
- `security/detect-object-injection`: warn
- `jsx-a11y/*`: multiple rules configured

---

## 9. 📚 Documentație Existentă

| Tip | Există? | Locație | Actualizat? |
|-----|---------|---------|-------------|
| README | ✅ | `/README.md` | ⬜ |
| Comentarii cod | ✅ | inline | ✅ |
| API docs (Swagger) | ⬜ | — | — |
| Diagrame | ⬜ | — | — |
| Template component | ✅ | `/docs/template_component.md` | ✅ |

---

## 10. 🎯 Componente UI Refolosibile

### Design System (`components/ui/`) - 60+ componente

| Categorie | Componente |
|-----------|------------|
| **Layout** | Card, Dialog, Drawer, Sheet, Popover, Tabs, Accordion |
| **Form** | Input, Select, Checkbox, Switch, RadioGroup, DateField, FileUpload |
| **Data Display** | Table, DataGrid (cu DnD), Badge, Avatar, Skeleton, Progress |
| **Navigation** | Breadcrumb, Pagination, NavigationMenu, Menubar, DropdownMenu |
| **Feedback** | Alert, AlertDialog, Sonner (toast), Tooltip, HoverCard |
| **Charts** | Chart (wrapper recharts), Sparkline |
| **Advanced** | Command (cmdk), CommandPalette, Calendar, Kanban, Tree, Stepper |
| **Animation** | CountingNumber, SlidingNumber, TypingText, TextReveal, WordRotate |

### Componente Business (`components/`)

| Componentă | Scop |
|------------|------|
| `auth/AuthProvider` | Context autentificare |
| `dashboard/KpiCards` | Widget KPI-uri |
| `dashboard/MyVms` | Widget liste VMs |
| `dashboard/BlockchainTrust` | Status chain |
| `dashboard/HealthStrip` | Status servicii |
| `templates/TemplateCard` | Card template catalog |
| `provenance/ProvenanceChip` | Blockchain anchor badge |
| `layout/AiDrawer` | Chat AI sidebar |

---

## 11. 🔄 State Management

### Abordare
- **Server State**: TanStack Query (React Query) v5
- **Client State**: React useState/useReducer local
- **Complex Flows**: XState (checkout provisioning)
- **Form State**: React Hook Form + Zod

### Query Keys centralizate
```typescript
queryKeys = {
  health: () => ["health"],
  templates: (params) => ["templates", params],
  template: (id) => ["template", id],
  vms: (params) => ["vms", params],
  vm: (id) => ["vm", id],
  orders: (params) => ["orders", params],
  actions: (params) => ["actions", params],
  profile: () => ["profile"],
  chainHealth: () => ["chain-health"],
}
```

### Stale Time Presets
| Resursa | StaleTime |
|---------|-----------|
| Templates, Profile | 60s |
| VMs | 10s |
| Orders | 30s |
| Health | 5s |
| Anchors | ∞ (imutabile) |

---

## 12. 🌍 Internationalizare (i18n)

### Limbi suportate
- 🇷🇴 Română (ro) - default
- 🇬🇧 English (en)

### Fișiere traduceri
- `/messages/ro.json`
- `/messages/en.json`

### Hardcoded Strings de Externalizat 🚨
| Fișier | String | Secțiune sugerată |
|--------|--------|-------------------|
| `apps/page.tsx` | "Aplicațiile mele" | `nav.apps` |
| `apps/page.tsx` | "Deschide", "Start", "Stop" | `common.actions` |
| `orders/page.tsx` | "Orders (istoric)" | `nav.orders` |
| `profile/page.tsx` | "Account Information" | `profile.accountInfo` |
| `checkout/page.tsx` | "Template & Config", "Billing" | `checkout.steps` |
| `action-center/page.tsx` | "Aprobă", "Respinge" | `actions.approve/reject` |

---

## 13. 📊 Accesibilitate (a11y)

### Implementat
- [x] ESLint jsx-a11y plugin configurat
- [x] `aria-label` pe butoane icon-only
- [x] `aria-expanded` / `aria-controls` pe expandable rows
- [x] Focus management în dialog/sheet
- [x] Semantic HTML (nav, main, section)
- [x] Skip links în layout

### De îmbunătățit
- [ ] Contrast ratio verificare sistematică
- [ ] Screen reader testing
- [ ] Keyboard navigation complete testing

---

## 14. 🎬 Next Steps

### Imediat (această săptămână)
- [ ] Externalizare strings hardcoded rămase
- [ ] Completare Profile cu date reale Keycloak
- [ ] Fix placeholder-uri în VM detail metrics

### Curând (luna aceasta)
- [ ] Integrare Grafana pentru metrics VM
- [ ] Payment integration (Stripe/PSP)
- [ ] Notifications real-time (WebSocket)
- [ ] Creștere test coverage la 60%+

### Viitor (backlog)
- [ ] AI Chat functional (backend integration)
- [ ] Mobile responsive improvements
- [ ] Dark/Light theme persistence
- [ ] Audit accessibility complet

---

## 15. 📝 Estimare Stadiu Final

| Criteriu | Status |
|----------|--------|
| MVP Funcțional | ✅ DA |
| Beta | ✅ DA |
| Producție | 🔄 75% |

**Concluzie**: Aplicația este un **MVP funcțional matur** / **Early Beta**, cu majoritatea funcționalităților core implementate. Lipsesc:
- Integrare PSP plăți
- Metrics reale (Grafana)
- Polish pe unele pagini (Profile/Settings/Security)
- Externalizare completă i18n

---

*Generat cu template BiziX v1.0*
*Actualizat: 2025-12-06*

