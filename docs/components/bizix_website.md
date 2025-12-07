# 📋 Documentația Aplicației Frontend BiziX (Landing Website)

## 🏗️ Arhitectură Generală

| Aspect | Tehnologie |
|--------|-----------|
| **Framework** | Vue 3.5 + TypeScript |
| **Build Tool** | Vite 6.1 |
| **Routing** | Vue Router 4.5 (`vite-plugin-pages` pentru file-based routing) |
| **State Management** | Pinia 2.3 |
| **Internationalizare** | vue-i18n 11.1 |
| **CSS Framework** | Bulma 0.9.4 + SCSS |
| **SSR/SSG** | Suport pentru SSR și Static Site Generation |

---

## 📄 1. PAGINI / RUTE PRINCIPALE

### Pagini Publice

| Rută | Fișier | Features | Stadiu |
|------|--------|----------|--------|
| `/` | `index.vue` | Hero animat, timeline (Connect/Simplify/Prosper), secțiuni industrii, testimoniale, trust stats API | ✅ **Complet** |
| `/marketplace` | `marketplace.vue` | Căutare, filtre avansate (categorie/preț/rating/sortare), grid responsive, paginare, sidebar filtre | ✅ **Complet** |
| `/apps/[id]` | `apps/[id].vue` | Detalii aplicație, markdown rendering, screenshots, specificații, CTAs dinamice | ✅ **Complet** |
| `/demo/[appId]` | `demo/[appId].vue` | Sandbox viewer, zona placeholder pentru demo interactiv | ⚠️ **Parțial** (placeholder UI) |
| `/solutii/[slug]` | `solutii/[slug].vue` | Pagini dinamice per industrie (restaurante/saloane/consultanță), beneficii, apps recomandate, testimoniale | ✅ **Complet** |
| `/preturi` | `preturi.vue` | Plans comparison, pricing cards, FAQ, CTA block | ✅ **Complet** |
| `/despre-noi` | `despre-noi.vue` | Povestea companiei, valori, founders, echipa, logo marquee | ✅ **Complet** |
| `/contact` | `contact.vue` | Formular contact, MapBox integration, FAQ, info contact | ✅ **Complet** |
| `/blog` | `blog/index.vue` | Lista articole, subscription form | ✅ **Complet** |
| `/blog/[id]` | `blog/[id].vue` | Articol individual cu markdown | ✅ **Complet** |
| `/cariere` | `cariere.vue` | Lista joburi (date locale), CTA email | ⚠️ **Mock data** |
| `/termeni` | `termeni.vue` | Termeni și condiții | ✅ **Complet** |
| `/confidentialitate` | `confidentialitate.vue` | Politica GDPR | ⚠️ **Mock** (text static) |

### Pagini de Eroare

| Rută | Fișier | Stadiu |
|------|--------|--------|
| `/error/500` | `error/500.vue` | ⚠️ **Hardcoded strings** |
| `/*` (404) | `[...all].vue` | ⚠️ **Hardcoded strings** |

---

## 🧩 2. COMPONENTE REFOLOSIBILE

### Base Components (`/src/components/base/`)
| Director | Componente | Descriere |
|----------|-----------|-----------|
| `avatar/` | Avatar, AvatarGroup | Avatare și grupuri |
| `button/` | Button, Buttons | Butoane cu variante |
| `card/` | Card | Carduri container |
| `collapse/` | Collapse | Accordion/collapse |
| `counter/` | Counter | Numărătoare animate |
| `form/` | Field, VInput, VTextarea | Elemente formular |
| `icon/` | IconBox, Icon | Iconography |
| `image/` | DarkImage | Imagini cu suport dark mode |
| `modal/` | Modal | Modals/dialogs |
| `placeholder/` | Placeholder, PlaceholderSection | Loading states |
| `table/` | Table | Tabele |
| `tabs/` | Tabs | Tab navigation |
| `tag/` | Tag | Etichete/badges |
| `title/` | Title, Subtitle, PageTitle, SectionTitle | Typography |

### Layout Components (`/src/components/layout/`)
| Director | Componente |
|----------|-----------|
| `container/` | Container |
| `section/` | Section |
| `footer/` | Footer |
| `hero/` | HeroT, HeroE, heroE/ |

### Block Components (`/src/components/blocks/`)
| Director | Exemplu Componente |
|----------|-----------|
| `cta-blocks/` | CtaBlockB, CtaBlockF, CtaBlockH |
| `feature-blocks/` | PulseCards, SideBenefits, StackedSection |
| `footer-blocks/` | FooterD |
| `navbar-blocks/` | NavbarA |
| `product/` | ProductCard, AppGrid |
| `team-blocks/` | TeamBlockE, FoundersSection |
| `testimonial-blocks/` | TestimonialBlockA, TestimonialBlockC |

### Advanced Components (`/src/components/advanced/`)
| Director | Funcționalitate |
|----------|-----------------|
| `blog/` | BlogList, BlogPost, BlogListItem, BlogGridItem |
| `company/` | CompanyStory, ValuesIconSection |
| `contact/` | BlockContact, ContactInfo |
| `faq/` | FaqListBoxed |
| `features/` | TimelineTitle |
| `logo/` | LogoMarquee |
| `pricing/` | PricingCompact, ComparisonBasic |
| `terms/` | TermsBlock |

### Navigation (`/src/components/navigation/`)
| Component | Descriere |
|-----------|-----------|
| `Navbar` | Navbar principal cu menu |
| `LanguageSwitcher` | Selector de limbă |
| `Hamburger` | Menu mobil |

### Misc (`/src/components/misc/`)
| Component | Descriere |
|-----------|-----------|
| `BackToTop` | Scroll to top button |
| `SandboxViewer` | Demo viewer pentru aplicații |
| `Globe` | Glob animat 3D (cobe) |
| `MapBox` | Integrare hartă |
| `SubscriptionCompact` | Formular newsletter |

---

## 📦 3. STATE MANAGEMENT (PINIA STORES)

### `useDarkmode` (`/src/stores/darkmode.ts`)
```typescript
// Funcționalități:
- colorSchema: 'system' | 'dark' | 'light'
- isDark: computed (cu persistență în localStorage)
- onChange(): toggle handler
- initDarkmode(): aplicare CSS class pe body
```

### `useUserSession` (`/src/stores/userSession.ts`)
```typescript
// Funcționalități:
- token: ref (persistat în localStorage)
- user: ref<UserData>
- isLoggedIn: computed
- setUser(), setToken(), logoutUser()
```

---

## 🔌 4. API CALLS & ENDPOINT-URI

| Endpoint | Folosit în | Metodă | Descriere |
|----------|-----------|--------|-----------|
| `/api/trust-stats` | `index.vue` | GET | Statistici trust (documents, nodes, transactions) |
| `/api/apps` | `marketplace.vue` | GET | Lista aplicații cu filtre și paginare |
| `/api/apps/[id]` | `apps/[id].vue` | GET | Detalii aplicație individuală |
| `/api/auth/register` | Multiple componente | External link | Redirect la client portal pentru înregistrare |

**Query params pentru `/api/apps`:**
- `page`, `limit`, `category`, `q` (search), `price`, `minRating`, `recommended`, `sort`

---

## ♿ 5. ACCESIBILITATE (A11Y)

### ✅ Aspecte Bune
- `aria-label` pe input-uri și butoane interactive
- `role="button"` pe elemente clickable non-button
- `role="menuitem"` pe navbar
- `role="alert"` pe PWA reload prompt
- `tabindex` pentru keyboard navigation pe chips/tags
- `@keyup.enter` handlers pentru taste

### ⚠️ Probleme Identificate
| Locație | Problema |
|---------|----------|
| `HorizontalFilterBar.vue` | Reset span fără `aria-label` |
| `ProductCard.vue` | `aria-label="Evaluare"` hardcoded |
| Multiple pagini | `alt=""` pe imagini decorative (OK) dar lipsește pe imagini informative |
| Marketplace modal filters | Labels hardcoded în română |
| Icon buttons | Unele iconuri interactive fără `aria-label` |

---

## 🔤 6. HARDCODED STRINGS (PENTRU EXTERNALIZARE)

### Pagini cu texte hardcoded:

| Fișier | Texte neexternalizate |
|--------|----------------------|
| `marketplace.vue` | `"Toate"`, `"Nou"`, `"Popular"`, `"Preț ↑"`, `"Preț ↓"`, `"Caută..."`, `"Filtre"`, `"Categoria"`, `"Preț"`, `"Recomandări"`, `"Aplică"`, `"Recomandat de BiziX"`, labels în modal |
| `apps/[id].vue` | `"Înapoi la Marketplace"`, `"Descriere"`, `"Capturi de ecran"`, `"Ce include"`, `"Informații"`, `"Acțiuni"`, `"Specificații"`, `"Adăugată"`, `"Rating"`, `"Preț"`, `"Propusă în block"`, `"Autor"`, `"Aplicație — BiziX"` |
| `confidentialitate.vue` | Întregul conținut (Date colectate, Scopul prelucrării, etc.) |
| `[...all].vue` (404) | `"Pagina nu a fost găsită"`, `"Mergi la Acasă"`, `"Vezi Marketplace"` |
| `error/500.vue` | `"Eroare internă a serverului"`, `"Mergi la Acasă"` |
| `cariere.vue` | Job descriptions hardcoded în array |
| `SandboxViewer.vue` | `"Demo aplicație"`, `"Creează cont pentru a salva"`, `"Zona interactivă"`, FAQ items |
| `ProductCard.vue` | `"Recomandat"`, `"Creat de"`, `"Doar X €"`, `"Activă"`, `"Detalii"`, `"Explorează Demo"`, `"Instalează Gratuit"`, `"Activează pentru"`, `"Sau cumpără un pass de o zi"` |

### Estimare: ~50-60 strings de externalizat

---

## 🚧 7. UI INCOMPLETE / PLACEHOLDER-URI

| Locație | Tip | Descriere |
|---------|-----|-----------|
| `demo/[appId].vue` | **Placeholder major** | SandboxViewer arată doar un placeholder generic, nu demo real |
| `SandboxViewer.vue` | **Placeholder** | `placeholder-area`, `placeholder-toolbar`, `placeholder-canvas` - UI stub |
| `confidentialitate.vue` | **Mock content** | Descris explicit ca "(mock)" în SEO description |
| `cariere.vue` | **Mock data** | Jobs array hardcoded local |
| `/data/pages/vehicle/` | **Placeholder images** | Multe `/placeholder.svg` references |
| `/data/pages/support/` | **Placeholder avatars** | Avatare `/placeholder.svg` |
| `/data/pages/workout/` | **Placeholder** | Picture/cover placeholder |
| `ProductCard.vue` | **Fallback** | Logo BiziX ca placeholder când app nu are imagine |

---

## 📊 REZUMAT STADIU GENERAL

| Metric | Valoare |
|--------|---------|
| **Pagini totale** | 15 (+ rute dinamice) |
| **Pagini complete** | 10 (67%) |
| **Pagini parțial complete** | 4 (27%) |
| **Pagini mock/placeholder** | 1 (6%) |
| **Componente refolosibile** | ~60+ |
| **Strings de externalizat** | ~50-60 |
| **API endpoints** | 3 interne + 1 extern |
| **Stores Pinia** | 2 |
| **Limbi suportate** | 2 (ro, en) |

### Priorități Recomandate:
1. 🔴 **High**: Externalizare hardcoded strings din `marketplace.vue`, `apps/[id].vue`, `ProductCard.vue`
2. 🟠 **Medium**: Implementare demo sandbox real (în loc de placeholder)
3. 🟠 **Medium**: Completare politică confidențialitate cu text real
4. 🟡 **Low**: Îmbunătățire a11y (aria-labels complete)
5. 🟡 **Low**: Înlocuire placeholder images din data files