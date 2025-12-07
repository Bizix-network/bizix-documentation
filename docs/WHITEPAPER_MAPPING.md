# 🗺️ Mapare Whitepaper → Componente Tehnice

> Acest document leagă fiecare promisiune din whitepaper de componenta care o implementează.

---

## Capitolul 1: Introducere

| Afirmație din Whitepaper | Componentă(e) | Verificare |
|--------------------------|---------------|------------|
| "Site, programări, facturare, CRM — în câteva minute" | A01-A05 (pachete) | ⬜ |
| "Comisioane recurente, plătite automat" | C02 (smart contracts) | ⬜ |
| "Verificat matematic, nu doar promis" | C01 (blockchain) | ⬜ |

---

## Capitolul 2: Arhitectura pe 4 Piloni

### Pilonul 1: Fundația Operațională
| Feature | Componentă | Status |
|---------|------------|--------|
| ERP integrat | A04, A05 | ⬜ |
| CRM | A04 | ⬜ |
| Management proiect | A05 | ⬜ |
| HR | A05 | ⬜ |
| Pachetele preconfigurate (5) | A01-A05 | ⬜ |

### Pilonul 2: Cloud Privat (Proxmox)
| Feature | Componentă | Status |
|---------|------------|--------|
| Uptime >99% | C05 | ⬜ |
| Izolare date per client | C05 | ⬜ |
| Criptare | C05 | ⬜ |

### Pilonul 3: Integration Engine
| Feature | Componentă | Status |
|---------|------------|--------|
| n8n ca motor | C03 | ⬜ |
| Workflow Chatwoot → ERP | C03 | ⬜ |
| Workflow factură → contabilitate | C03 | ⬜ |
| Workflow WooCommerce → stoc | C03 | ⬜ |

### Pilonul 4: Blockchain
| Feature | Componentă | Status |
|---------|------------|--------|
| Hash-uri imutabile | C01 | ⬜ |
| Ștampilare temporală | C01 | ⬜ |

---

## Capitolul 3: Arhitectura Încrederii

### Registrul Imutabil
| Feature | Componentă | Status |
|---------|------------|--------|
| Hash criptografic per eveniment | C01 | ⬜ |
| Nu conține date sensibile | C01 | ⬜ |
| Auditabilitate absolută | C01, P05 | ⬜ |

### Glass Box
| Feature | Componentă | Status |
|---------|------------|--------|
| Verificare pe explorer.bizix.ro | P05 | ⬜ |
| Nimeni nu poate modifica retroactiv | C01 | ⬜ |

### Descentralizare (3 Faze)
| Feature | Componentă | Status |
|---------|------------|--------|
| Faza 1: 3 noduri BiziX | C01 | ⬜ |
| Faza 1: Cod open-source | C01 | ⬜ |
| Faza 1: Anchoring Polygon | C01 | ⬜ |
| Faza 2: 5+ validatori externi | C01 | ⬜ |
| Faza 2: BiziX <50% putere | C01 | ⬜ |
| Faza 3: 21+ validatori | C01 | ⬜ |
| Faza 3: BiziX <15% putere | C01 | ⬜ |

### Specificații Tehnice Chain
| Feature | Componentă | Status |
|---------|------------|--------|
| Timp bloc 2s | C01 | ⬜ |
| 10.000 TPS | C01 | ⬜ |
| DPoS consens | C01 | ⬜ |
| Ed25519 semnături | C01 | ⬜ |
| Slashing mechanism | C02 | ⬜ |

### Smart Contracts
| Feature | Componentă | Status |
|---------|------------|--------|
| Plăți automate comisioane | C02 | ⬜ |
| Escrow automatizat | C02 | ⬜ |
| Plăți la confirmare recepție | C02 | ⬜ |

### AI Trust Layer
| Feature | Componentă | Status |
|---------|------------|--------|
| Hash date intrare + model + output | C01, C04 | ⬜ |
| Audit AI decisions | P05 | ⬜ |

---

## Capitolul 4: Ecosistemul

### Mediu Dezvoltatori
| Feature | Componentă | Status |
|---------|------------|--------|
| API-uri robuste | P03 | ⬜ |
| SDK-uri | P03 | ⬜ |
| Documentație | P03 | ⬜ |

### Marketplace
| Feature | Componentă | Status |
|---------|------------|--------|
| Listing aplicații | P04 | ⬜ |
| Comision automat dezvoltatori | C02 | ⬜ |
| One-click deploy | C05 | ⬜ |

### Concierge Personalizări
| Feature | Componentă | Status |
|---------|------------|--------|
| Formular cerere | P04 | ⬜ |
| Matching cu dezvoltatori | P04 | ⬜ |
| Escrow pentru proiecte | C02 | ⬜ |
| Milestone tracking | C02 | ⬜ |

### Developer Mode
| Feature | Componentă | Status |
|---------|------------|--------|
| SSH access | C05 | ⬜ |
| Snapshot înainte de SSH | C05 | ⬜ |

### Opțiuni Deployment
| Feature | Componentă | Status |
|---------|------------|--------|
| Fully Managed | C05 | ⬜ |
| Self-Hosted | C05 | ⬜ |

### Portabilitate
| Feature | Componentă | Status |
|---------|------------|--------|
| Export CSV, JSON, XML | P01 | ⬜ |
| Export facturi PDF/A, UBL | P01 | ⬜ |
| Export Merkle proofs | C01, P01 | ⬜ |

### Ambasadori
| Feature | Componentă | Status |
|---------|------------|--------|
| Înregistrare instant | P02 | ⬜ |
| Link personalizat | P02 | ⬜ |
| Dashboard comisioane | P02 | ⬜ |
| Comision 20/25/30% pe tier | C02 | ⬜ |

### BYOS
| Feature | Componentă | Status |
|---------|------------|--------|
| ISO specializat | C06 | ⬜ |
| Fleet Manager | C06 | ⬜ |
| Full-disk encryption | C06 | ⬜ |
| WireGuard tunnel | C06 | ⬜ |
| Staking obligatoriu | C02 | ⬜ |

### Validatori
| Feature | Componentă | Status |
|---------|------------|--------|
| Staking 10.000 BIZ | C02 | ⬜ |
| Comision delegatori 0-20% | C02 | ⬜ |
| Recompense bloc | C01, C02 | ⬜ |

---

## Capitolul 5: Guvernanță

| Feature | Componentă | Status |
|---------|------------|--------|
| Propuneri on-chain (BIP) | C02 | ⬜ |
| Tipuri propuneri (4 nivele) | C02 | ⬜ |
| Vot on-chain | C02 | ⬜ |
| Quorum diferențiat | C02 | ⬜ |
| Multiplicator vot (holder/staker/validator) | C02 | ⬜ |

---

## Capitolul 8: BiziX AI

### Local LLMs
| Feature | Componentă | Status |
|---------|------------|--------|
| Modele locale (nu third-party) | C04 | ⬜ |
| Procesare EU | C04 | ⬜ |

### Human-in-the-Loop
| Feature | Componentă | Status |
|---------|------------|--------|
| Salvare ca "Ciornă" | C04, C03 | ⬜ |
| Notificare pentru aprobare | C03 | ⬜ |

### Funcționalități AI
| Feature | Componentă | Status |
|---------|------------|--------|
| OCR facturi | C04 | ⬜ |
| Clasificare documente | C04 | ⬜ |
| Asistent conversațional | C04 | ⬜ |
| Analiză predictivă | C04 | ⬜ |
| Sumarizare | C04 | ⬜ |
| Natural language to SQL | C04 | ⬜ |

---

## Capitolul 10: Tokenomics

### Token BIZ
| Feature | Componentă | Status |
|---------|------------|--------|
| Supply 100M fix | C01 | ⬜ |
| 18 zecimale | C01 | ⬜ |
| ERC-20 wrapped | C02 | ⬜ |

### Staking
| Feature | Componentă | Status |
|---------|------------|--------|
| Lock-up periods (flex/3/6/12 luni) | C02 | ⬜ |
| Multiplicatori recompense | C02 | ⬜ |
| Unstaking period | C02 | ⬜ |

### Burn
| Feature | Componentă | Status |
|---------|------------|--------|
| 20% burn pe utilizare | C02 | ⬜ |

### Tier System
| Feature | Componentă | Status |
|---------|------------|--------|
| Tier 1 (holder) | P01, C02 | ⬜ |
| Tier 2 (staker 1K) | P01, C02 | ⬜ |
| Tier 3 (delegator 5K) | P01, C02 | ⬜ |
| Tier 4 (validator 10K) | P01, C02 | ⬜ |

---

## Capitolul 11: Ambasadori

| Feature | Componentă | Status |
|---------|------------|--------|
| Pagină personalizabilă | P02 | ⬜ |
| Materiale de suport | P02 | ⬜ |
| Prag minim retragere 50 BIZ | C02 | ⬜ |
| Conturi dormante 12 luni | P02, C02 | ⬜ |

---

## 📊 Sumar per Componentă

| Componentă | Features Alocate | Procent din Total |
|------------|------------------|-------------------|
| C01 - BiziX Chain | ___ | ___% |
| C02 - Smart Contracts | ___ | ___% |
| C03 - Integration Engine | ___ | ___% |
| C04 - AI Gateway | ___ | ___% |
| C05 - Infrastructure | ___ | ___% |
| C06 - Fleet Manager | ___ | ___% |
| P01 - Portal Client | ___ | ___% |
| P02 - Portal Ambasador | ___ | ___% |
| P03 - Portal Developer | ___ | ___% |
| P04 - Marketplace | ___ | ___% |
| P05 - Explorer | ___ | ___% |
| P06 - Wallet | ___ | ___% |

---

*Acest document trebuie actualizat pe măsură ce documentezi fiecare componentă.*

