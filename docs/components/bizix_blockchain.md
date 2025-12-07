# 📋 DOCUMENTAȚIE TEHNICĂ - BiziX Blockchain Node

## 1. 🎯 SCOPUL PRINCIPAL AL COMPONENTEI

**BiziX Blockchain Node** este un nod blockchain bazat pe **Substrate** (Polkadot SDK v1.9.0) conceput ca infrastructură pentru o platformă **BPaaS (Blockchain Platform as a Service)** destinată IMM-urilor din România.

### Funcționalități Principale:
- **Managementul propunerilor de aplicații SaaS** - workflow complet de submitere, discuție, votare și aprobare
- **Registru de companii românești** - stocare on-chain a datelor de firmă (CUI, denumire, EUID, etc.)
- **Sistem de comisioane și plăți** - transferuri între dezvoltatori, francizați și platforma centrală
- **Governance decentralizat** - Technical Committee, Treasury, Identity Management

---

## 2. 🔌 ENDPOINT-URI API (RPC)

### BizixCore RPC

| Endpoint | Descriere |
|----------|-----------|
| `bizix_getValue` | Returnează ID-ul curent al propunerii |

### Company Registry RPC

| Endpoint | Descriere |
|----------|-----------|
| `companyRegistry_getCompanyData` | Obține datele unei companii după CUI |
| `companyRegistry_getQueryFee` | Returnează costul interogării |
| `companyRegistry_getCompanyDataIfPaid` | Obține datele doar dacă s-a plătit |

### Extrinsics (Dispatchables) - BizixCore

| Call Index | Funcție | Descriere |
|------------|---------|-----------|
| 0 | `submit_proposal` | Submitere propunere nouă |
| 1 | `approve_proposal` | Aprobare propunere |
| 2 | `reject_proposal` | Respingere propunere |
| 3 | `get_proposals_by_status` | Filtrare propuneri după status |
| 4 | `vote_on_proposal` | Votare propunere |
| 5 | `close_voting` | Închidere votare (Technical Committee) |
| 6 | `change_proposal_status` | Schimbare status propunere |
| 7 | `pay_commission` | Plată comision |

### Extrinsics - Company Registry

| Call Index | Funcție | Descriere |
|------------|---------|-----------|
| 0 | `add_company` | Adăugare companie nouă |
| 1 | `update_company` | Actualizare date companie |
| 2 | `claim_company` | Claiming ownership companie |
| 3 | `transfer_company_ownership` | Transfer ownership |
| 4 | `pay_for_company_data` | Plată pentru acces date |

---

## 3. 📦 DEPENDENȚE EXTERNE (Cargo Packages)

### Core Substrate (Polkadot SDK v1.9.0)

| Pachet | Versiune | Scop |
|--------|----------|------|
| `frame-support` | polkadot-v1.9.0 | Framework FRAME |
| `frame-system` | polkadot-v1.9.0 | System pallet |
| `sp-core` | polkadot-v1.9.0 | Primitive core |
| `sp-runtime` | polkadot-v1.9.0 | Runtime utilities |
| `sp-std` | polkadot-v1.9.0 | Standard library |
| `sp-api` | polkadot-v1.9.0 | Runtime API |

### Paleți Substrate Standard

| Pachet | Scop |
|--------|------|
| `pallet-aura` | Block authoring |
| `pallet-grandpa` | Finality |
| `pallet-balances` | Token management |
| `pallet-sudo` | Administrative privileges |
| `pallet-timestamp` | Block timestamps |
| `pallet-transaction-payment` | Fee management |
| `pallet-treasury` | Treasury management |
| `pallet-collective` | Technical Committee |
| `pallet-identity` | On-chain identity |

### Dependențe Auxiliare

| Pachet | Versiune | Scop |
|--------|----------|------|
| `parity-scale-codec` | 3.6.1 | Encoding/Decoding |
| `scale-info` | 2.10.0 | Type metadata |
| `jsonrpsee` | 0.22 | RPC server |
| `clap` | 4.5.3 | CLI parsing |
| `serde_json` | 1.0.114 | JSON serialization |

---

## 4. 📊 ESTIMARE STADIU COMPLETARE

| Componentă | Status | Procent |
|------------|--------|---------|
| **Infrastructură Blockchain Core** | ✅ Complet | 100% |
| **BizixCore Pallet** | ✅ Complet | 95% |
| **Company Registry Pallet** | ✅ Complet | 95% |
| **RPC APIs** | ✅ Complet | 90% |
| **Governance (Treasury, Committee)** | ✅ Complet | 100% |
| **Identity Management** | ✅ Complet | 100% |
| **Unit Tests** | ⚠️ Placeholder | 10% |
| **Benchmarking** | ✅ Implementat | 80% |
| **Documentație** | ⚠️ Parțial | 50% |

### **SCOR GENERAL: ~85% funcțional**

---

## 5. 🚧 TODO-URI, FIXME-URI ȘI WORK IN PROGRESS

### Probleme Găsite:

1. **FIXME în `service.rs` (linia 276)**:
   ```rust
   // FIXME #1578 make this available through chainspec
   ```

2. **TODO în `runtime/src/lib.rs` (linia 307)**:
   ```rust
   //TO DO: type TechnicalCommittee = pallet_collective::EnsureMember<AccountId, TechnicalCollective>;
   ```
   ⚠️ **Problema**: `TechnicalCommittee` este setat la `EnsureRoot` în loc de validarea prin membri, ceea ce înseamnă că doar `sudo` poate controla propunerile.

3. **Valoare Temporară în `bizix/src/lib.rs` (linia 380)**:
   ```rust
   let approval_threshold = 1; // Temporar setat la 1 pentru testare
   ```
   ⚠️ **Problema**: Threshold-ul de aprobare este hardcodat la 1 vot.

4. **Teste Placeholder**:
   - `pallets/bizix/src/tests.rs` - conține teste generice din template, nu teste reale pentru funcționalitate
   - `pallets/company_registry/src/tests.rs` - același template neactualizat

5. **RPC BizixCore Minimal**:
   - Doar `get_value()` este implementat, lipsesc RPC-uri pentru:
     - Obținere propuneri
     - Filtrare după status
     - Obținere voturi

---

## 6. 🔐 PROBLEME DE SECURITATE POTENȚIALE

### 🔴 CRITICE

1. **Lipsă Validare Origin pentru `reject_proposal`**:
   ```rust:294:313:pallets/bizix/src/lib.rs
   pub fn reject_proposal(
       origin: OriginFor<T>,
       proposal_id: u32
   ) -> DispatchResult {
       // Verifică dacă apelantul este membru al consiliului tehnic
       //T::TechnicalCouncilOrigin::ensure_origin(origin)?;  // COMENTAT!
   ```
   ⚠️ **Oricine poate respinge propuneri!**

2. **TechnicalCommittee = EnsureRoot**:
   - Doar contul `sudo` poate:
     - Schimba statusul propunerilor
     - Închide votările
   - Technical Committee pallet este configurat dar nefolosit pentru control propuneri

3. **Lipsă Validare Ownership în `update_company`**:
   ```rust:178:215:pallets/company_registry/src/lib.rs
   pub fn update_company(...) -> DispatchResult {
       let sender = ensure_signed(origin)?;
       // NU verifică dacă sender == owner!
   ```
   ⚠️ **Oricine poate modifica datele oricărei companii!**

### 🟡 MODERATE

4. **Threshold Votare Hardcodat**:
   - `approval_threshold = 1` nu este configurabil
   - O singură persoană poate aproba propuneri

5. **Lipsă Rate Limiting**:
   - Nicio limită pe numărul de propuneri/companii per utilizator
   - Potențial DOS prin spam

6. **Weight Constant**:
   - Toate funcțiile au `weight(10_000)` hardcodat
   - Ar trebui să folosească benchmarking weights

7. **Plăți Comision Fără Verificare**:
   - `pay_commission` nu verifică dacă propunerea este aprobată
   - Oricine poate plăti comisioane pentru orice propunere

---

## 7. 🧪 TESTE EXISTENTE

### Stare Actuală: ⚠️ INSUFICIENT

| Fișier | Status |
|--------|--------|
| `pallets/bizix/src/tests.rs` | Template generic (neactualizat) |
| `pallets/bizix/src/mock.rs` | Mock runtime pentru teste |
| `pallets/company_registry/src/tests.rs` | Template generic (neactualizat) |
| `pallets/company_registry/src/mock.rs` | Mock runtime pentru teste |
| `pallets/bizix/src/benchmarking.rs` | Suport benchmarking |
| `pallets/company_registry/src/benchmarking.rs` | Suport benchmarking |

### Teste Lipsă Critice:
- ❌ Teste pentru `submit_proposal`
- ❌ Teste pentru `vote_on_proposal`
- ❌ Teste pentru `change_proposal_status`
- ❌ Teste pentru `pay_commission`
- ❌ Teste pentru workflow complet propuneri
- ❌ Teste pentru `add_company`, `claim_company`
- ❌ Teste pentru `pay_for_company_data`
- ❌ Integration tests
- ❌ Fuzz testing

---

## 8. 🔗 INTERACȚIUNI CU ALTE COMPONENTE

```
┌─────────────────────────────────────────────────────────────────┐
│                     BiziX Runtime                                │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐    ┌────────────────────┐    ┌─────────────┐  │
│  │  BizixCore   │◄──►│ TechnicalCommittee │◄──►│  Treasury   │  │
│  │   Pallet     │    │     (Collective)   │    │   Pallet    │  │
│  └──────────────┘    └────────────────────┘    └─────────────┘  │
│         │                     │                       │          │
│         ▼                     ▼                       ▼          │
│  ┌──────────────┐    ┌────────────────────┐    ┌─────────────┐  │
│  │   Balances   │◄──►│     Identity       │◄──►│    Sudo     │  │
│  │    Pallet    │    │      Pallet        │    │   Pallet    │  │
│  └──────────────┘    └────────────────────┘    └─────────────┘  │
│         ▲                                                        │
│         │                                                        │
│  ┌──────────────┐                                               │
│  │  Company     │                                               │
│  │  Registry    │                                               │
│  │   Pallet     │                                               │
│  └──────────────┘                                               │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
              │                         │
              ▼                         ▼
       ┌─────────────┐          ┌─────────────────┐
       │  RPC Layer  │          │   Consensus     │
       │ (jsonrpsee) │          │ (Aura+Grandpa)  │
       └─────────────┘          └─────────────────┘
              │
              ▼
       ┌─────────────────────────────────────────┐
       │         External Clients                │
       │  (Polkadot.js, Custom dApps, IPFS)     │
       └─────────────────────────────────────────┘
```

### Diagrama Flux Date:

| De la | Către | Tip Interacțiune |
|-------|-------|------------------|
| BizixCore | Balances | Plăți comisioane (Currency trait) |
| BizixCore | TechnicalCommittee | Verificare origin pentru acțiuni privilegiate |
| CompanyRegistry | Balances | Plăți pentru interogări date |
| Treasury | Balances | Managementul fondurilor |
| Identity | Balances | Depozite pentru identitate |
| Toate paleții | System | Block numbers, events, storage |

---

## 📋 RECOMANDĂRI PENTRU CONTINUARE

1. **🔴 URGENT**: Remediați problemele de securitate identificate
2. **🟡 IMPORTANT**: Implementați teste reale pentru toate funcționalitățile
3. **🟡 IMPORTANT**: Configurați `TechnicalCommittee` corect în loc de `EnsureRoot`
4. **🟢 RECOMANDAT**: Adăugați RPC-uri complete pentru BizixCore
5. **🟢 RECOMANDAT**: Implementați rate limiting și validări suplimentare
6. **🟢 RECOMANDAT**: Actualizați README.md cu informații specifice BiziX