# 🤖 Ghid: Cum să extragi documentație tehnică de la AI

> Acest ghid te ajută să obții informații consistente de la AI (Claude, ChatGPT, etc.) pentru fiecare componentă BiziX.

---

## 📋 Workflow Recomandat

```
1. Deschide repo-ul/folderul componentei
2. Copiază prompt-ul potrivit de mai jos
3. Paste-uiește în AI + atașează codul relevant
4. Copiază output-ul în docs/[categorie]/[component].md
5. Actualizează TECH_INDEX.md cu stadiul
```

---

## 🎯 Prompt-uri pentru Diferite Tipuri de Componente

### PROMPT 1: Pentru Componente Backend (Node.js, Rust, Python)

```
Analizează acest cod și generează documentație tehnică structurată.

CONTEXT: Aceasta este o componentă din proiectul BiziX, o platformă 
BPaaS pentru IMM-uri bazată pe blockchain.

SARCINI:
1. Identifică scopul principal al componentei
2. Listează toate endpoint-urile API (dacă există)
3. Identifică dependențele externe (npm/cargo/pip packages)
4. Estimează stadiul de completare (% funcțional)
5. Găsește TODO-uri, FIXME-uri, comentarii care indică work in progress
6. Identifică probleme de securitate potențiale
7. Listează ce teste există
8. Descrie cum interacționează cu alte componente

FORMAT OUTPUT: Folosește template-ul markdown atașat.

[PASTE CODUL AICI SAU ATAȘEAZĂ FIȘIERELE]
```

---

### PROMPT 2: Pentru Smart Contracts (Solidity/Ink!)

```
Analizează aceste smart contracts și generează un raport tehnic.

CONTEXT: Contractele fac parte din BiziX Chain, un blockchain pentru 
automatizarea proceselor de business.

SARCINI:
1. Identifică fiecare contract și scopul său
2. Listează funcțiile publice cu parametri și return values
3. Identifică modificatorii de acces (onlyOwner, etc.)
4. Găsește evenimente emise
5. Identifică patterns de securitate folosite (ReentrancyGuard, etc.)
6. Caută vulnerabilități potențiale:
   - Reentrancy
   - Integer overflow/underflow
   - Access control issues
   - Front-running risks
7. Estimează dacă contractul e ready pentru audit

FORMAT: Tabel cu funcții, parametri, și note de securitate.

[PASTE CONTRACTELE AICI]
```

---

### PROMPT 3: Pentru Frontend (React, Vue, Next.js)

```
Analizează această aplicație frontend și documentează structura.

CONTEXT: Aceasta este una din interfețele BiziX (client/ambasador/developer portal).

SARCINI:
1. Identifică page-urile/route-urile principale
2. Listează componentele refolosibile
3. Descrie state management-ul folosit (Redux, Zustand, etc.)
4. Identifică API calls și către ce endpoint-uri
5. Verifică accesibilitatea (a11y)
6. Găsește hardcoded strings care ar trebui externalizate
7. Identifică UI incomplete sau placeholder-e

FORMAT: Lista de pagini cu features și stadiu pentru fiecare.

[PASTE CODUL RELEVANT]
```

---

### PROMPT 4: Pentru Infrastructure as Code (Docker, K8s, Terraform)

```
Analizează configurația de infrastructură și documentează.

CONTEXT: BiziX rulează pe Proxmox VE cu container-e Docker.

SARCINI:
1. Listează serviciile definite
2. Identifică porturile expuse
3. Descrie networking-ul (rețele, volume)
4. Găsește secreturi/credentials hardcodate (⚠️)
5. Verifică dacă există health checks
6. Identifică dependințe între servicii
7. Estimează cerințele de resurse (RAM, CPU, storage)
8. Verifică dacă e production-ready sau doar dev

FORMAT: Tabel cu servicii, porturi, resurse, status.

[PASTE DOCKER-COMPOSE/CONFIGS]
```

---

### PROMPT 5: Pentru n8n Workflows (Integration Engine)

```
Analizează aceste workflow-uri n8n și documentează-le.

CONTEXT: n8n este Integration Engine-ul BiziX care conectează aplicațiile.

SARCINI:
1. Identifică fiecare workflow și trigger-ul său
2. Listează nodurile folosite și în ce ordine
3. Găsește credențiale/API keys necesare
4. Identifică error handling-ul implementat
5. Descrie ce date curg prin workflow
6. Găsește workflow-uri incomplete sau dezactivate
7. Identifică dependențe de alte sisteme

FORMAT: Diagramă text a fiecărui workflow + tabel cu detalii.

[PASTE WORKFLOW JSON EXPORTS]
```

---

## 🔍 Prompt Bonus: Aliniere cu Whitepaper

După ce ai documentația componentei, folosește acest prompt:

```
Am acest extras din whitepaper-ul BiziX:

[PASTE SECȚIUNEA RELEVANTĂ DIN WHITEPAPER]

Și această documentație tehnică a componentei [NUME]:

[PASTE DOCUMENTAȚIA COMPONENTEI]

SARCINI:
1. Ce features din whitepaper sunt IMPLEMENTATE COMPLET?
2. Ce features sunt PARȚIAL implementate?
3. Ce features LIPSESC complet?
4. Ce există în cod dar NU e menționat în whitepaper?
5. Ce discrepanțe există între promisiuni și realitate?

FORMAT: Tabel cu 3 coloane: Feature | Status | Gap/Note
```

---

## 📊 Checklist După Extragere

Pentru fiecare componentă documentată:

- [ ] Fișier creat în `docs/[categorie]/[component].md`
- [ ] TECH_INDEX.md actualizat cu stadiul
- [ ] Aliniere cu whitepaper verificată
- [ ] Gaps identificate și notate
- [ ] Technical debt documentat
- [ ] Next steps definite

---

## 🎯 Ordinea Recomandată de Documentare

**Prioritate 1 — Core (fără ele nu merge nimic)**
1. BiziX Chain (blockchain)
2. Smart Contracts
3. Cloud Infrastructure (Proxmox)

**Prioritate 2 — Glue (conectează totul)**
4. Integration Engine (n8n)
5. AI Gateway

**Prioritate 3 — Interfețe (utilizatorii le văd)**
6. Portal Client
7. Marketplace

**Prioritate 4 — Secondary**
8-17. Restul componentelor

---

## 💡 Tips

1. **Fii specific cu codul** — nu da tot repo-ul, ci fișierele relevante
2. **Cere exemple concrete** — "arată-mi un exemplu de request/response"
3. **Validează output-ul** — AI-ul poate greși, verifică afirmațiile
4. **Iterează** — dacă răspunsul e vag, cere detalii
5. **Salvează prompt-urile bune** — ce funcționează, refolosește

---

*Ghid creat pentru documentarea tehnică BiziX*

