# BiziX — Direcție strategică: aplicații native, angajați AI și ecosistem deschis

> **Data consolidării:** 2026-09-12
> **Status:** Principii de produs asumate în discuție; arhitectură de referință propusă; validare tehnică și comercială în așteptare.
> **Scop:** Documentarea direcției, a limitelor și a criteriilor de validare. Nu reprezintă implementare, audit de securitate sau angajament de livrare.

## Principiul central

> **Avantajul Bizix nu ar trebui să fie că poate genera software, ci că poate transforma acel software într-un sistem de business coerent, sigur și întreținut în timp.**

Bizix devine un spațiu operațional unic al firmei, în care oamenii, angajații AI și automatizările folosesc aceleași capabilități de business, sub reguli comune. Aplicațiile native sunt construite pentru această fundație; personalizările pot deveni componente reutilizabile, iar platformele externe rămân conectabile prin API.

**„Totul într-un singur loc” înseamnă experiență unificată, nu ecosistem închis.** Un client poate folosi CRM și programări în Bizix, dar facturare în FGO, fără să fie obligat să migreze toate serviciile sale.

## Cuprins

1. [Context și diagnostic](#1-context-și-diagnostic)
2. [Principii de produs](#2-principii-de-produs)
3. [Arhitectura de referință](#3-arhitectura-de-referință)
4. [Date, capabilități și contracte](#4-date-capabilități-și-contracte)
5. [Aplicații native și modelul de extensie](#5-aplicații-native-și-modelul-de-extensie)
6. [Personalizare și transformarea în produse reutilizabile](#6-personalizare-și-transformarea-în-produse-reutilizabile)
7. [Angajații AI](#7-angajații-ai)
8. [Integrări API și ecosistem deschis](#8-integrări-api-și-ecosistem-deschis)
9. [Caz de referință: CRM Bizix și facturare FGO](#9-caz-de-referință-crm-bizix-și-facturare-fgo)
10. [Servicii comune și infrastructură proprie](#10-servicii-comune-și-infrastructură-proprie)
11. [Blockchain și încredere verificabilă](#11-blockchain-și-încredere-verificabilă)
12. [Stack și reutilizarea tehnologiilor existente](#12-stack-și-reutilizarea-tehnologiilor-existente)
13. [Scalare, securitate și continuitate](#13-scalare-securitate-și-continuitate)
14. [Model economic și responsabilități](#14-model-economic-și-responsabilități)
15. [Plan de validare și criterii de acceptare](#15-plan-de-validare-și-criterii-de-acceptare)
16. [Riscuri și schimbări față de roadmap](#16-riscuri-și-schimbări-față-de-roadmap)
17. [Registrul deciziilor și întrebărilor deschise](#17-registrul-deciziilor-și-întrebărilor-deschise)
18. [Surse și limitele verificării](#18-surse-și-limitele-verificării)

---

## 1. Context și diagnostic

### 1.1 Ce știm și ce nu știm

Analiza pornește de la whitepaper, catalogul de aplicații și rapoartele tehnice din acest repository. Documentația componentelor este în principal din decembrie 2025. Repository-ul analizat conține documentație, nu implementările componentelor descrise.

Procentele de progres, estimările de test coverage și etichetele „production ready” sunt afirmații istorice ale documentelor, nu măsurători reverificate la data acestei analize. Nu deducem numărul clienților activi, dimensiunea echipei sau costurile reale de operare din aceste etichete.

Există neconcordanțe: [TECH_INDEX](./TECH_INDEX.md) raportează probleme de autorizare în blockchain, iar [IMPLEMENTATION_SUMMARY](./IMPLEMENTATION_SUMMARY.md) bifează unele remedieri. Starea trebuie stabilită din cod, teste și deployment. `EnsureRoot` este o alegere de control administrativ care trebuie evaluată față de modelul de guvernanță, nu automat aceeași categorie de defect ca lipsa autorizării.

### 1.2 Ce merită reutilizat

| Element | Ce descrie documentația | Direcție și verificări necesare |
|---|---|---|
| Backend Core | Node.js/Express, MongoDB, comenzi, provisioning, BullMQ | Reutilizarea funcțiilor utile prin contracte; verificarea autorizării, izolării și testelor |
| Keycloak și Bridge | OIDC, reverse proxy, configurare de acces | Păstrarea identității; SSO nu înlocuiește autorizarea din aplicații |
| Client Portal | Next.js/React, design system și Action Center | Bază pentru spațiul de lucru, aprobări și administrarea angajaților AI |
| Proxmox și automatizările de infrastructură | Managementul mediilor de execuție | Reutilizare unde este economic și operațional justificat |
| Integrarea blockchain | Nod Substrate și servicii de interacțiune | Evaluare de securitate și compatibilitate; izolare în spatele serviciului de trust |

Core-ul documentează multi-tenancy complet ca funcționalitate planificată. Bridge-ul descrie alocarea per tenant a porturilor, cu o limită de aproximativ 100 de tenants în configurația prezentată. Acestea sunt puncte de verificat, nu baze suficiente pentru o promisiune de scalare.

### 1.3 Problema structurală

Catalogul actual conține aplicații, integrări și plugin-uri eterogene. Costul nu se reduce la instalare: trebuie întreținute modele de date, permisiuni, actualizări, vulnerabilități și integrări diferite.

**SSO + provisioning automat + dashboard comun nu echivalează cu integrarea proceselor de business.** Whitepaper-ul recunoaște că unele aplicații sunt inițial independente, iar sincronizarea este o etapă ulterioară.

Nu presupunem că fiecare poziție din catalog înseamnă o VM distinctă pentru fiecare client și nu declarăm costul nesustenabil fără măsurători. De asemenea, personalizarea OSS nu este imposibilă: există framework-uri cu extensii și configurări separate de baza comună. Decizia pentru aplicații native Bizix urmărește controlul unui model coerent, nu ideea că toate alternativele sunt neviabile.

AI poate reduce efortul de dezvoltare, dar nu elimină verificarea, migrarea datelor, operarea și răspunderea pentru rezultate. Productivitatea se evaluează pe schimbări acceptate și întreținute, nu doar pe viteza generării inițiale.

---

## 2. Principii de produs

1. **Aplicații native Bizix:** aplicațiile de business oferite ca native se reconstruiesc pe fundația comună, fără perpetuarea unui catalog de produse independente ca model principal.
2. **Reutilizarea fundației:** nu reconstruim de la zero autentificarea, framework-urile web, bazele de date sau protocoalele de comunicare.
3. **Experiență unificată:** un spațiu al firmei, navigare comună, date coerente, inbox operațional, costuri vizibile și suport unic.
4. **Ecosistem deschis:** clientul poate păstra aplicații externe și le poate conecta prin API; adoptarea Bizix poate fi graduală.
5. **Capabilități comune:** interfețele, oamenii, AI-ul și workflow-urile folosesc aceleași acțiuni și reguli de business.
6. **Personalizare fără divergență necontrolată:** configurările și extensiile trebuie să poată evolua odată cu baza comună.
7. **Reutilizare cu drepturi clare:** personalizările generalizabile pot deveni produse, fără reutilizarea implicită a datelor sau know-how-ului confidențial al clientului.
8. **Autonomie controlată:** omul aprobă mandatul și regulile; AI-ul execută în limitele lor; excepțiile și acțiunile sensibile cer control suplimentar.
9. **Independență prin înlocuibilitate:** Bizix controlează serviciile și contractele proprii, fără promisiunea nerealistă de a elimina orice dependență externă.
10. **Încredere verificabilă:** blockchain-ul susține dovezi și relații între participanți, nu înlocuiește securitatea aplicațiilor sau verificarea rezultatelor AI.
11. **Portabilitate reală:** clientul rămâne prin valoarea oferită, nu prin imposibilitatea de a recupera datele și soluția sa.

Acoperirea completă a activității unei categorii de firme are prioritate față de construirea simultană a multor module incomplete. „Totul într-un singur loc” nu promite acces prin API la orice funcție a oricărui furnizor și nici eliminarea tuturor etapelor de autorizare sau conformitate externă.

---

## 3. Arhitectura de referință

### 3.1 Structură logică

```text
                      Spațiul de lucru Bizix
                /               |                \
          Interfețe          Angajați AI       Automatizări
                \               |                /
             Contracte de capabilități + permisiuni + politici
                                |
                 Module de business și adaptoare aprobate
                  /                              \
         Aplicații native                  Platforme externe
       CRM, programări etc.                FGO, calendare etc.
                  \                              /
          Workflow | Notificări | Fișiere | AI Gateway | Trust
```

Diagrama definește responsabilități, nu impune un microserviciu sau o bază de date separată pentru fiecare casetă.

| Strat | Responsabilități |
|---|---|
| Core de platformă | Companii, identități, apartenență, drepturi, abonamente, instalări, versiuni, politici și audit |
| Module de business | Entități, reguli, acțiuni și evenimente ale domeniului propriu |
| Servicii comune | Workflow-uri, notificări, documente, fișiere, acces AI și dovezi |
| Integration Layer | Conexiuni externe, adaptoare, mapări, sincronizare, reconciliere și disponibilitate |
| App Studio / AI Constructor | Specificații, configurări, cod, teste, preview și publicare controlată |
| Runtime operațional | Execuția aplicațiilor, a sarcinilor și a angajaților AI |

Facturarea abonamentului Bizix nu este același domeniu cu facturarea realizată de firma clientului. Datele, drepturile și evidențele lor trebuie delimitate.

### 3.2 Un singur spațiu operațional

Portalul trebuie să reunească modulele native și funcțiile externe disponibile prin API, căutarea autorizată, conversațiile, sarcinile, aprobările și istoricul operațiunilor. Nu urmărim doar un meniu cu linkuri către produse diferite.

Action Center-ul descris în documentația portalului este un punct de plecare pentru inbox-ul comun al oamenilor și AI-ului. Existența rutelor și interfeței în documentație nu confirmă implementarea completă a execuției, aprobărilor și auditului din backend.

### 3.3 Separarea construcției de operare

AI Constructor modifică specificații și cod în medii de dezvoltare. AI Operator folosește numai capabilități deja publicate și autorizate. Pot împărți accesul la modele prin AI Gateway, dar nu identitățile privilegiate, instrumentele sau mediile de execuție.

Un angajat AI nu își poate modifica singur permisiunile, politicile de aprobare ori codul din producție. O cerere de schimbare a aplicației devine o propunere pentru pipeline-ul de personalizare.

---

## 4. Date, capabilități și contracte

### 4.1 Proprietatea datelor și sursa autoritativă

Fiecare categorie de informații are un responsabil și o sursă autoritativă declarată. Nu permitem modificarea directă a tabelelor unui modul de către toate celelalte module.

Modelul comun începe cu identități stabile, companie, apartenență și conceptele necesare primului proces validat. Nu proiectăm anticipat un model universal pentru toate industriile.

Pentru fiecare integrare stabilim:

- cine deține fiecare entitate sau câmp;
- identificatorul intern și corespondența cu identificatorii externi;
- direcția sincronizării și regulile de conflict;
- proveniența, versiunea și momentul ultimei sincronizări;
- ce informații sunt copii de consultare și ce operațiuni pot modifica sursa;
- ce snapshot-uri trebuie păstrate pentru documente și aprobări.

O bază coerentă de informații nu presupune o singură bază de date fizică. O copie locală a unei facturi externe nu devine automat sursa ei autoritativă.

### 4.2 Registrul capabilităților

Fiecare modul sau adaptor publică un contract versionat pentru acțiunile pe care le poate executa. Exemplele `crm.customer.read`, `booking.appointment.create` și `invoicing.issue` sunt nume conceptuale, nu API-uri deja implementate.

| Element al contractului | Cerință |
|---|---|
| Identificare | Nume stabil, versiune, modul sau adaptor responsabil |
| Date | Scheme validate pentru intrări, rezultate și erori |
| Context | Companie, actor și mandat stabilite din identitatea verificată, nu acceptate necontrolat din payload |
| Autorizare | Drepturi, resurse accesibile și politici aplicabile |
| Efecte | Citire, modificare, comunicare externă sau operațiune sensibilă |
| Fiabilitate | Idempotență unde este posibil, corelare, timeout și tratarea rezultatului necunoscut |
| Evoluție | Compatibilități, deprecări și teste de contract |
| Audit | Actor, acțiune, rezultat, aprobare și consum relevant |

Regulile domeniului sunt aplicate de implementarea capabilității, indiferent dacă apelul vine din UI, AI sau workflow. Înregistrarea unui modul în catalog nu acordă automat permisiuni tuturor agenților.

OpenAPI și schemele de evenimente sunt candidați pentru contractele publice. MCP poate fi un adaptor pentru consumul capabilităților de către AI, nu un substitut pentru autorizare sau pentru semantica de business.

### 4.3 Evenimente și procese durabile

Evenimentele au identificator, versiune de schemă, companie, sursă și corelare cu operațiunea inițială. O modificare de business și înregistrarea intenției de publicare a evenimentului trebuie legate durabil, de exemplu prin transactional outbox.

Consumatorii trebuie să trateze redelivery, evenimente duplicate, sosire în altă ordine și bucle de sincronizare. Nu promitem „exactly once” global peste servicii externe.

Starea procesului este persistentă, nu doar în conversația AI sau în memoria workerului:

```text
Creată → În lucru → Așteaptă informații / aprobare
                  → Finalizată / Eșuată / Anulată
                  → Rezultat extern necunoscut → Reconciliere
```

Retry-urile au limite, backoff și politici potrivite efectului. Un proces poate fi preluat de un om. Compensarea unei acțiuni de business nu este același lucru cu anularea unei tranzacții SQL.

---

## 5. Aplicații native și modelul de extensie

### 5.1 Ce reconstruim

Reconstruim aplicațiile native la nivel de produs, selectând procesele relevante pentru clienții Bizix. Nu clonăm integral toate funcțiile ERP-urilor, CRM-urilor sau platformelor de colaborare consacrate.

Produsele existente pot fi studiate pentru procese, cerințe, interacțiuni și cazuri-limită. Rezultatul cercetării devine specificație proprie și teste de acceptare. Preluarea codului sau a altor materiale se analizează separat, conform licențelor; rescrierea prin AI nu elimină obligațiile aplicabile.

Framework-ul intern înseamnă în primul rând SDK, contracte, convenții, metadate, extensii și pipeline. Nu înseamnă reconstruirea React, OIDC, SMTP sau a unei baze de date.

### 5.2 Compoziție în loc de fork per client

```text
Core și servicii comune Bizix
    └── Aplicație de bază
          └── Pachet pentru industrie
                └── Module reutilizabile opționale
                      └── Configurări și extensii private ale firmei
```

| Nivel | Exemple | Model de întreținere |
|---|---|---|
| Configurare declarativă | Câmpuri, formulare, branding, reguli simple | Metadate validate; fără copierea întregului proiect |
| Extensie de cod | Calcul particular, UI specială, integrare nouă | Cod versionat, contracte, teste și izolare |
| Soluție specializată | Modul complex sau aplicație externă păstrată de client | Integrare prin capabilități; runtime dedicat dacă este justificat |

Un pachet complet de industrie poate fi comercializat ca produs, dar trebuie să fie o compoziție întreținută, nu o copie divergentă a instalării unui client.

### 5.3 App Spec și artefactele de versiune

App Spec descrie compoziția, configurările, legăturile cu servicii și politicile de aplicat. Nu încercăm să exprimăm orice software într-un limbaj declarativ universal.

Configurările simple pot fi interpretate de runtime. Generarea de cod este rezervată variațiilor care au nevoie de ea. Specificația singură nu garantează regenerarea identică a aplicației.

O versiune publicabilă păstrează împreună specificația, versiunile modulelor și generatorului, codul extensiilor, dependențele fixate, migrările, rezultatele verificărilor, aprobările și artefactul construit. Un prompt sau numele modelului AI nu înlocuiesc aceste elemente.

Exemplul următor este ilustrativ: schema, denumirile și versiunile nu reprezintă produse implementate. `connection_ref` indică o conexiune configurată separat; nu conține credențiale. Drepturile efective și mandatul AI necesită aprobare în sistemul de identitate și politici.

```yaml
# exemplu conceptual: salon de înfrumusețare
spec_version: "draft-1"
app:
  name: salon-elena
  template: servicii-programari
  locale: ro
modules:
  - id: crm
    version: "1.0.0"
  - id: booking
    version: "1.0.0"
capability_bindings:
  invoicing:
    provider: fgo
    connection_ref: facturare-principala
extensions:
  - id: service-packages
    version: "1.0.0"
    config:
      uses: 5
      validity_days: 90
roles:
  receptie:
    capabilities:
      - crm.customer.read
      - booking.appointment.create
workflows:
  - template: booking.confirmation
    version: "1.0.0"
    channels: [email, sms]
ai_employees:
  - template: receptionist
    version: "1.0.0"
    policy_ref: receptionist-restricted
```

---

## 6. Personalizare și transformarea în produse reutilizabile

### 6.1 Pipeline de personalizare

```text
Cerere → Clarificarea cerinței → Specificație și impact
       → Validare politici → Configurări / cod / migrări
       → Teste → Preview → Aprobare → Publicare → Monitorizare
```

AI explică schimbarea în termeni de business, inclusiv efectele asupra datelor, permisiunilor, integrărilor și costurilor. Cerințele ambigue nu devin reguli ascunse în cod.

Verificările includ teste de acceptare definite independent, autorizare și izolare între tenants, compatibilitate, dependențe, securitate și migrări. Testele generate de același AI nu sunt singura dovadă de corectitudine. Aprobarea funcțională a antreprenorului nu înlocuiește verificarea tehnică pentru schimbările sensibile.

Preview-ul folosește date sintetice sau pregătite corespunzător. AI Constructor nu primește implicit secrete și acces la producție. Publicarea folosește artefactul verificat, nu o nouă generare neverificată.

Rollback-ul codului nu inversează automat migrările. Strategia trebuie să includă compatibilitate între versiuni, migrări etapizate, backup verificat și proceduri explicite pentru schimbări ireversibile.

### 6.2 Pipeline separat de transformare în produs reutilizabil

```text
Personalizare validată pentru clientul A
    → Verificarea drepturilor și a interesului comercial
    → Separarea particularităților private
    → Generalizare și parametrizare
    → Teste în contexte diferite + compatibilități + migrări
    → Responsabil de mentenanță + licență + versiune
    → Catalog intern → Adoptare de către clientul B
```

O personalizare funcțională nu devine automat produs distribuibil. Mai întâi validăm că nu depinde de date, configurări ascunse sau proceduri confidențiale ale clientului inițial.

Exemplu: o firmă cere pachete de cinci ședințe. Capabilitatea generalizată gestionează numărul de utilizări, valabilitatea, suspendarea, consumul și notificările. Altă firmă o configurează pentru opt ședințe. Nu copiem datele, prețurile sau întreaga aplicație a primei firme.

### 6.3 Drepturi și contribuții

| Rezultat | Tratament propus |
|---|---|
| Capabilitate generică | Reutilizabilă conform unui acord clar |
| Configurări, date și memorie AI ale firmei | Private, fără publicare implicită |
| Logică specifică sau know-how confidențial | Nu se reutilizează fără acord explicit |
| Dezvoltare exclusivă | Regim contractual și comercial distinct |

Plata unei personalizări nu stabilește singură toate drepturile asupra rezultatului. Anonimizarea numelui firmei nu este suficientă dacă se dezvăluie un proces confidențial. Clienții pot primi reduceri, credit sau participare la venituri pentru contribuții reutilizabile, dacă acest model este convenit.

La început, catalogul este administrat intern. Marketplace-ul public vine după validarea review-ului, compatibilităților, licențelor și responsabilității pentru mentenanță.

---

## 7. Angajații AI

### 7.1 Definiție

Un angajat AI este un operator software identificat ca AI, cu mandat delimitat. Nu este doar un prompt, o identitate umană simulată sau un model separat obligatoriu pentru fiecare client.

| Element | Conținut |
|---|---|
| Identitate | Principal tehnic distinct, companie și istoric propriu |
| Mandat | Obiective, responsabil uman și limite de delegare |
| Competențe | Capabilități și workflow-uri versionate |
| Permisiuni | Date și operațiuni autorizate; fără cont administrator partajat |
| Cunoaștere | Surse aprobate, acces și retenție |
| Autonomie | Acțiuni automate, aprobări și escaladări |
| Resurse | Buget, limite de acțiuni, concurență și durată |
| Evaluare | Rezultate, intervenții umane, erori și costuri |

Când acționează în numele unui utilizator, accesul este limitat atât de mandatul agentului, cât și de drepturile aplicabile delegării. Drepturile se verifică la execuție și pot fi revocate.

### 7.2 Roluri inițiale posibile

- **Recepționer AI:** identifică persoana, clarifică solicitarea, verifică disponibilitatea, gestionează programări în limitele mandatului și escaladează excepțiile.
- **Asistent comercial:** pregătește oferte și follow-up-uri, fără discounturi sau angajamente nelimitate.
- **Operator documente:** extrage informații și pregătește ciorne, cu verificarea câmpurilor și a surselor.
- **Analist:** consultă date autorizate și explică rezultate calculate de servicii deterministe, fără să inventeze valori operaționale.

Un pachet de competențe AI poate deveni reutilizabil, împreună cu modulele, workflow-urile, politicile și testele de care depinde. Memoria și procedurile private ale unei firme nu se transferă implicit altora.

### 7.3 Autonomie pe bază de politici

| Acțiune | Politică de referință |
|---|---|
| Citire, căutare, raport | Automat în limitele drepturilor |
| Creare ciornă | Automat, cu validarea intrărilor |
| Programare sau reminder standard | Automat numai în regulile preaprobate |
| Discount peste limită, excepție contractuală | Aprobare explicită |
| Plată, schimbare de drepturi, ștergere importantă | Controale suplimentare și aprobare explicită |

Nu există un singur comutator global „AI poate face orice”. Politicile sunt aplicate în backend, separat de prompt și de încrederea declarată de model.

Aprobarea se leagă de acțiunea concretă, versiunea datelor, destinatar, efecte și cost relevant. Schimbările semnificative cer reaprobare; înainte de execuție se reverifică drepturile și condițiile de business. Acțiunile ireversibile nu sunt tratate ca simple modificări de UI.

### 7.4 Date, memorie și siguranță

- Datele operaționale se citesc din sursele autoritative; RAG sau memoria conversației nu țin locul soldului ori disponibilității curente.
- Documentele și emailurile sunt date de procesat, nu autoritate pentru schimbarea politicilor agentului. Se tratează explicit riscul de prompt injection.
- Contextul, indexurile, cache-urile și memoria sunt izolate după companie și drepturile actorului.
- Memoria persistentă are proveniență, reguli de retenție, corectare și ștergere. Ipotezele AI nu devin automat fapte ale firmei.
- Credențialele conectorilor și cheile de semnare nu sunt introduse în prompturi sau afișate agentului.
- Modelele locale și API-urile externe sunt alese conform politicii de date, costului și evaluărilor; procesarea în UE ori conformitatea nu se presupun doar din numele furnizorului.
- Indisponibilitatea modelului nu trebuie să oprească aplicațiile deterministe. Sarcinile pot aștepta sau pot fi preluate de un om.

Un reminder la o oră cunoscută nu necesită inferență AI. Folosim modelul unde interpretarea aduce valoare, iar workflow-ul durabil pentru coordonare și execuție.

---

## 8. Integrări API și ecosistem deschis

### 8.1 Principiu de interoperabilitate

Un client poate folosi module native Bizix și platforme externe în același proces. Aceasta nu contrazice reconstrucția aplicațiilor native: definim originea produselor Bizix, nu interzicem alegerile clientului.

```text
UI / angajat AI / workflow
    → Capabilitate Bizix autorizată
    → Selectarea implementării configurate pentru companie
        → Modul nativ, dacă este disponibil și ales
        → Adaptor extern aprobat, de exemplu FGO
    → Rezultat normalizat, cu proveniență și status
```

Providerul se configurează pe companie și capabilitate sau pe un context explicit. Nu este ales arbitrar de AI. Conexiunea și versiunea folosite de o operațiune sunt păstrate pentru audit și reconciliere.

Adaptoarele traduc modele, nomenclatoare, erori și limite. Nu presupunem echivalența tuturor furnizorilor. Capabilitățile indisponibile sunt marcate ca atare; extensiile specifice furnizorului sunt declarate explicit, fără a contamina întregul model comun.

### 8.2 Contractul unui conector

| Zonă | Cerințe |
|---|---|
| Conectare | Autorizare de către client, identitatea firmei, mediul, planul și drepturile necesare |
| Secrete | Stocare server-side protejată, referințe în configurări, rotație și revocare; fără secrete în manifest, loguri sau context AI |
| Acces | Conexiuni separate per companie, drepturi minime și verificare înaintea fiecărei acțiuni |
| Capabilități | Operațiuni efectiv suportate de API, versiune și condiții comerciale |
| Mapare | Identificatori, câmpuri, nomenclatoare, rotunjiri și sursa autoritativă |
| Sincronizare | Direcție, checkpoint-uri, actualitate, conflicte și prevenirea buclelor |
| Fiabilitate | Limite per conexiune, retry controlat, deduplicare și reconciliere |
| Securitate transport | Endpoint-uri aprobate, validare de input, protejarea accesului la rețeaua internă și verificarea webhook-urilor, dacă există |
| Observabilitate | Stare conexiune, ultima sincronizare, erori, latență și consum |
| Evoluție | Teste de contract, sandbox, schimbări de API și procedură de deconectare sau migrare |

Bizix trebuie să poată și expune API-uri autorizate pentru integratori externi, nu doar să consume API-uri. În ambele direcții se aplică aceleași reguli de acces și audit.

### 8.3 Sincronizare fără dublă autoritate

Nu implementăm implicit sincronizare bidirecțională pentru toate câmpurile. Direcția se alege per tip de date și operațiune. Conflictele sensibile se prezintă în Action Center, fără a aplica automat „ultima scriere câștigă” asupra documentelor financiare.

Webhook-urile sunt utilizate numai dacă suportul și verificarea lor sunt confirmate. În lipsa lor, polling-ul și reconcilierea sunt proiectate în limitele API-ului. UI-ul arată sursa, momentul ultimei actualizări și stările în așteptare, neactualizate sau cu eroare.

Nu promitem integrare automată cu orice platformă. Contractarea, autorizarea, configurarea unor serii, consimțământul și unele mapări necesită participarea clientului. Automatizarea crește după acest onboarding.

---

## 9. Caz de referință: CRM Bizix și facturare FGO

### 9.1 Scenariu și delimitare

Clientul păstrează facturarea în FGO și utilizează CRM, programări, sarcini și angajați AI în Bizix. Acesta este un caz de arhitectură și un candidat de validare, nu un conector implementat sau un parteneriat comercial confirmat.

| Informație sau proces | Sursă autoritativă propusă |
|---|---|
| Lead-uri, oportunități, interacțiuni și sarcini comerciale | CRM Bizix |
| Programări și reguli de rezervare | Modulul de programări Bizix |
| Intenția de facturare și aprobarea payload-ului | Workflow Bizix |
| Factura emisă în FGO, seria, numărul și documentul rezultat | FGO |
| Datele fiscale ale unei facturi deja emise | Snapshot-ul documentului emis, nu contactul CRM modificabil |
| Valori facturate și achitate consultate prin conector | Datele raportate de FGO, cu momentul ultimei verificări |
| Legătura între vânzare și factură, sincronizare și audit | Integration Layer Bizix |

Datele de contact sau de facturare necesare emiterii se mapează explicit. Modificarea adresei din CRM nu rescrie retroactiv o factură emisă. Sumele raportate ca achitate de furnizor nu constituie singure o confirmare bancară independentă.

### 9.2 Fluxul propus

1. O vânzare sau un serviciu ajunge în starea care permite pregătirea facturii.
2. Bizix creează o intenție de facturare, cu identificator stabil și snapshot al datelor necesare.
3. Un om sau AI-ul pregătește datele; regulile fiscale, valorile și nomenclatoarele se validează, nu se deduc liber de model.
4. Politica verifică mandatul și solicită aprobarea acolo unde este necesară.
5. Capabilitatea de facturare folosește conexiunea FGO configurată pentru companie.
6. Conectorul păstrează intenția înainte de apel și, la succes, asociază răspunsul FGO: serie, număr și referința documentului.
7. CRM-ul afișează informațiile și documentul printr-un acces autorizat, cu proveniența FGO și starea sincronizării.
8. Notificările și actualizările ulterioare se execută conform mandatului, fără trimiterea implicită repetată de către ambele sisteme.
9. Consultarea statusului actualizează proiecția locală. Excepțiile sau neconcordanțele intră în reconciliere și, dacă este necesar, în Action Center.

Nu presupunem că toate facturile create manual în FGO pot fi importate automat. Descoperirea și importul lor trebuie verificate separat față de urmărirea documentelor emise prin conector.

### 9.3 Ce confirmă documentația publică

La 2026-09-12 au fost consultate [pagina oficială de integrare](https://www.fgo.ro/integrare/api/) și [documentația API v7.0](https://api.fgo.ro/v1/testing.html).

- Sunt publicate medii de test și producție distincte, care nu se sincronizează între ele.
- Este descris onboarding cu utilizator API, cheie privată și configurarea seriilor de documente. Accesul în producție și unele operațiuni depind de abonament; eligibilitatea concretă se confirmă pentru contul clientului.
- Sunt documentate `POST /factura/emitere`, `POST /factura/print` și `POST /factura/getstatus`.
- Exemplul de răspuns pentru status include valoarea facturii și valoarea achitată. Acesta nu trebuie confundat cu o confirmare completă a fluxului e-Factura/SPV.
- Emiterea documentează `IdExtern` și opțiunea `VerificareDuplicat`, în condițiile contractului API.
- Documentația indică pentru facturare o limită de un apel pe secundă per utilizator API și un timeout de 15 secunde. Limitele trebuie reverificate la implementare și respectate central pentru toate operațiunile care folosesc aceeași conexiune.
- Nu a fost confirmat suportul pentru webhook-uri de facturare în documentația consultată. Nu îl presupunem în arhitectura pilotului.

Nu au fost făcute apeluri autentificate, nu au fost create conturi și nu au fost emise facturi. Accesul, comportamentul deduplicării, operațiunile disponibile pe planul ales și sincronizarea efectivă trebuie testate separat într-un mediu autorizat.

### 9.4 Fiabilitate și rezultat necunoscut

Un timeout la emitere nu dovedește că factura nu a fost creată. Bizix păstrează operațiunea în starea „rezultat necunoscut”, fără a genera automat o nouă intenție.

`IdExtern` și `VerificareDuplicat` trebuie folosite și testate conform documentației. Existența acestor câmpuri nu justifică o promisiune generală de execuție exact o dată. Reconcilierea utilizează numai mecanisme confirmate ale API-ului sau verificare umană; nu presupunem un endpoint de căutare după identificator extern care nu a fost verificat.

**Indisponibilitatea FGO nu declanșează emiterea automată în alt furnizor sau într-un modul nativ.** Ar putea rezulta două documente pentru aceeași operațiune. CRM-ul și celelalte procese independente continuă, iar facturarea rămâne explicit în așteptare.

Schimbarea furnizorului este o operațiune controlată: se reconciliază lucrările în curs, se păstrează referințele istorice, se aprobă noua rutare și se tratează separat datele de migrat. Operațiunile vechi nu sunt redirecționate automat către noua conexiune.

---

## 10. Servicii comune și infrastructură proprie

### 10.1 Notification Hub

```text
Aplicații / workflow-uri / angajați AI
    → API de notificări Bizix
    → Drepturi, preferințe, template-uri, cote și intenții persistente
    → Adaptoare de transport email / SMS / push / in-app
    → Statusuri și evenimente normalizate
```

Bizix controlează orchestrarea, template-urile, consumul, consimțământul unde este necesar, dezabonarea, suprimarea și auditul. Se diferențiază clar „acceptat de transport”, „livrat” și „citit”; acestea nu sunt același rezultat.

Retry-ul sau schimbarea transportului după un timeout trebuie să țină cont de rezultatul necunoscut și de riscul de dublare. O regulă de notificare preaprobată nu cere o nouă confirmare pentru fiecare mesaj, dar nu permite trimitere arbitrară către orice destinatar.

### 10.2 Email Engine

Pentru trimitere tranzacțională se evaluează un MTA sau o platformă matură self-hosted, precum Postal, cu un strat Bizix propriu. Un serviciu complet de căsuțe poștale este o cerință separată de simpla trimitere a notificărilor.

Trebuie validate IP-urile, reverse DNS/PTR, portul 25, SPF, DKIM, DMARC, reputația, bounce-urile, reclamațiile și monitorizarea. Separarea traficului și controlul abuzului reduc riscul ca un client să afecteze livrarea celorlalți.

Un relay extern poate fi o opțiune de continuitate, nu o dependență obligatorie. Dacă este exclus, limitările de livrabilitate și operare trebuie acceptate și testate. Self-hosting-ul nu garantează un cost mai mic la orice volum.

Stalwart este o alternativă de evaluat, dar multi-tenancy este documentat ca funcționalitate Enterprise. Nu presupunem că orice configurație comercială dorită este acoperită gratuit de ediția comunitară.

### 10.3 SMS Engine

Bizix poate deține API-ul, regulile, template-urile, rutarea, evidența costurilor și reconcilierea. Transportul către telefoane necesită operatori sau agregatori și acorduri adecvate pentru trafic A2P.

Se evaluează adaptoare HTTP/SMPP și un gateway existent, de exemplu Jasmin, înainte de reconstruirea protocolului. Alegerea rutelor, sender ID-urile, rapoartele de livrare, codarea și costul segmentelor trebuie verificate pentru piețele deservite.

Un gateway cu modemuri și SIM-uri nu elimină operatorul. Nu se bazează oferta pe abonamente de consum fără permisiune contractuală; politicile unor operatori interzic explicit utilizarea automată sau revânzarea în acel regim.

### 10.4 Alți piloni

- **Fișiere și documente:** stocare cu interfață standard, drepturi, versionare, scanare, retenție și export.
- **Generare documente:** template-uri și PDF-uri; documentele oficiale externe își păstrează proveniența.
- **Semnare:** integrare cu soluții adecvate cerințelor; un hash nu înlocuiește semnătura calificată.
- **Workflow:** coordonare durabilă, aprobări, escaladări și recuperare.
- **AI Gateway:** modele înlocuibile, politici de date, limite, evaluări și evidența consumului.
- **Trust Service:** înregistrarea și verificarea dovezilor prin contracte independente de SDK-ul blockchain.

---

## 11. Blockchain și încredere verificabilă

Blockchain-ul poate susține proveniența versiunilor, aprobări, integritatea documentelor și acordurile sau decontările dintre participanți. Nu este folosit pentru fiecare citire CRM, editare de formular ori pas de inferență.

| Poate susține | Nu dovedește singur |
|---|---|
| Existența și integritatea unui angajament criptografic, în condițiile de încredere ale lanțului | Corectitudinea rezultatului AI |
| Urmărirea versiunilor și aprobărilor înregistrate | Că exact acel cod rulează efectiv în producție |
| Trasabilitatea unui acord sau a unei plăți on-chain | Dreptul legal de a reutiliza cod ori know-how |
| Verificarea unor dovezi exportate | Conformitatea juridică integrală sau autenticitatea datelor înainte de înregistrare |

Dovezile trebuie legate de artefacte semnate, înregistrări de deployment și aprobări verificabile. Identitatea actorului și faptul că o operațiune s-a executat efectiv nu se deduc doar dintr-un hash publicat de operator.

Datele personale, documentele, memoria AI și credențialele rămân off-chain. Nici hash-urile nu sunt automat anonime: se analizează riscul de corelare și se folosesc angajamente adecvate, inclusiv grupare sau randomizare unde este necesar.

BiziX Chain rămâne o investiție de evaluat pentru acest rol. Un lanț Substrate propriu nu moștenește automat securitatea rețelei Polkadot. Dacă toate nodurile sunt controlate de Bizix, garanțiile trebuie descrise transparent. Promisiunea de protecție față de administrator cere verificabilitate independentă; ancorarea externă nu poate fi amânată fără reevaluarea acestei promisiuni.

Pentru operațiunile obișnuite, ancorarea poate fi asincronă, cu status „în așteptare” și reconciliere. Nu afișăm drept confirmată o dovadă care încă nu a fost înregistrată și finalizată. Procesele al căror efect este chiar o tranzacție on-chain au cerințe distincte de confirmare.

Pentru instrumentele tehnice se poate reutiliza Polkadot.js Apps, cu personalizări limitate. Pentru integrări noi, documentația Polkadot recomandă PAPI sau Dedot, întrucât Polkadot.js API este în maintenance mode. Compatibilitatea cu runtime-ul Bizix, metadata și API-urile nodului trebuie verificată înainte de alegere.

Cheile de semnare sunt administrate separat de modelele AI, prin politici și limite verificabile. Tokenomics extins, guvernanța și marketplace-ul financiar nu trebuie să blocheze validarea aplicațiilor; lansarea lor necesită evaluări de securitate, operare și conformitate proprii.

---

## 12. Stack și reutilizarea tehnologiilor existente

Alegerile următoare sunt candidați de implementare, nu afirmații că toate componentele există sau sunt deja aprobate definitiv.

| Zonă | Candidat și rațiune |
|---|---|
| Frontend | React/Next.js și design system-ul existent; continuitate cu portalul |
| Backend nou | Node.js + TypeScript, cu convenții și module stricte; Express existent poate fi reutilizat, fără migrare obligatorie la alt framework |
| Date pentru aplicațiile noi | PostgreSQL; strategia multi-tenant se validează prin teste de izolare, migrare și restaurare |
| Datele core-ului existent | Păstrare inițială prin API-uri; nu impunem rescrierea MongoDB ca precondiție a pivotului |
| Identitate | Keycloak/OIDC plus autorizare efectivă în backend |
| Joburi | BullMQ existent, pentru sarcini potrivite unei cozi |
| Procese complexe | Evaluarea unui motor durabil, precum Temporal self-hosted, înainte de construirea unuia intern |
| Integrare | Contracte de capabilități, OpenAPI, evenimente versionate și adaptoare |
| Deployment | Artefacte containerizate și versionate; reutilizarea Proxmox unde este potrivit |

TypeScript este recomandat pentru continuitate și ecosistem, nu ca „cel mai bun limbaj pentru AI” demonstrat universal. Uniformitatea aplicațiilor native nu cere rescrierea în TypeScript a unui server de mail sau a unui motor de baze de date.

Construim un strat Bizix subțire peste tehnologii mature. Un framework existent, precum Frappe, poate fi evaluat ca bază tehnică sau referință pentru metadate și extensii, fără revenirea la modelul unui catalog eterogen de aplicații independente. Alegerea se face pe aceeași cerință pilot, nu pe preferință abstractă pentru un framework propriu.

n8n poate fi util în anumite integrări, dar nu se presupune licențiere nelimitată pentru încorporarea în SaaS. Expunerea editorului către clienți cere verificarea acordului OEM; utilizarea ca backend se verifică separat față de scenariul și planul ales.

Pentru fiecare dependență se verifică licența, drepturile de distribuire/hosting, condițiile de extensie, securitatea și mentenanța. Un serviciu self-hosted poate avea licență comercială. Dependențele folosite în build sunt controlate și fixate; AI-ul nu instalează arbitrar pachete în producție.

---

## 13. Scalare, securitate și continuitate

### 13.1 Granițe înainte de microservicii

Pentru noul strat de business, punctul de plecare recomandat este o structură modulară, fără fragmentare prematură în zeci de servicii. Microserviciile existente nu sunt rescrise doar pentru uniformitate.

Separăm procesele unde securitatea sau încărcarea o cere: AI Constructor, execuția extensiilor, workerii AI, transporturile și serviciile de infrastructură. Codul arbitrar generat pentru un client nu rulează direct în procesul comun privilegiat al tuturor clienților.

Containerele, namespace-urile, schema-per-tenant sau RLS nu sunt singure o demonstrație completă de izolare. Strategia combină drepturi minime, izolare a execuției și rețelei, politici în baza de date, controlul fișierelor/cozilor și teste negative. În PostgreSQL, rolurile privilegiate și proprietarii tabelelor pot ocoli anumite politici RLS, în condițiile documentate.

### 13.2 Controlul resurselor și disponibilitate

- Cote de concurență, trafic, mesaje, stocare și buget AI per companie și angajat AI.
- Limite de pași și durată a sarcinilor; fără retry sau bucle de agenți nelimitate.
- Măsurarea consumului și protecție față de un client care monopolizează resursele comune.
- Monitorizare pe capabilități și conectori, nu doar pe disponibilitatea serverelor.
- Reconciliere, cozi de erori și proceduri de intervenție umană.
- Testarea indisponibilității modelelor, transporturilor și blockchain-ului, cu degradare explicită a funcțiilor afectate.

### 13.3 Actualizare, backup și ieșire

Se testează upgrade-ul bazei comune împreună cu extensiile, procesele în curs și versiunile conectorilor. Se definesc perioade de compatibilitate și deprecări, fără a presupune că toate instalările se actualizează simultan.

Backup-ul trebuie verificat prin restaurare, inclusiv pentru o singură companie. Obiectivele de recuperare și pierderea acceptabilă de date se stabilesc înaintea promisiunilor comerciale.

Portabilitatea include date, fișiere, configurări, cod/artefacte necesare, migrări și drepturile de a rula dependențele relevante. Un YAML exportat nu elimină lock-in-ul dacă numai un runtime inaccesibil îl poate executa. Se validează restaurarea soluției în afara controlului operațional Bizix, cu reautorizarea serviciilor externe unde este necesar.

Whitepaper-ul promite VM privată și acces SSH opțional. Trecerea la runtime partajat schimbă această promisiune și trebuie comunicată și contractată explicit. Controlul complet nu este oferit în infrastructura partajată; un mediu dedicat poate rămâne o opțiune distinctă.

---

## 14. Model economic și responsabilități

Monetizarea poate combina abonamentul platformei, module/pachete verticale, personalizări, competențe AI, consum și mentenanță. Prețurile și distribuția veniturilor rămân de validat.

O personalizare generează fie un activ reutilizabil, fie o obligație privată de întreținere. Nu presupunem că orice cerere are piață la alți clienți sau că generarea ieftină face întreținerea gratuită.

Indicatorii utili includ:

- costul unei modificări acceptate, inclusiv revizuire și migrări;
- costul unei sarcini AI finalizate corect și intervenția umană necesară;
- consumul de infrastructură, modele, email/SMS și conectori;
- reutilizarea modulelor și compatibilitatea după upgrade;
- timpul economisit clientului, adoptarea, retenția și disponibilitatea de plată;
- costul suportului, incidentelor și actualizărilor furnizorilor externi.

Fiecare modul, conector și pachet de competențe are un responsabil de mentenanță. Publicarea trebuie să declare drepturile, compatibilitățile și suportul. Plata autorilor sau contributorilor nu este automatizată on-chain înainte de clarificarea bazei comerciale și juridice.

În whitepaper, distribuția abonamentului poate ajunge la 110% dacă procentele sunt cumulative și comisionul ambasadorului este 30%. Trebuie clarificat din ce categorie se scade diferența. Referral-ul are cost economic; nu este echivalent cu achiziție gratuită.

---

## 15. Plan de validare și criterii de acceptare

### 15.1 Etape bazate pe rezultate, nu pe estimări arbitrare

| Etapă | Livrabil și condiție de trecere |
|---|---|
| V0 — Stabilirea realității | Codul și deployment-urile existente verificate; starea autorizării, clienții activi, licențele, echipa și dependențele clarificate |
| V1 — Un proces complet | O verticală și un client pilot; aplicație nativă minimală, contracte și execuție manuală funcțională, dezvoltate cu AI ca instrument intern |
| V2 — Integrare și operare AI | Un angajat AI cu mandat restrâns; notificări și conector extern validate în medii autorizate; aceleași reguli ca în UI |
| V3 — Reutilizare și evoluție | O a doua firmă, configurare diferită, modul extras din personalizare, upgrade comun și restaurare fără pierderea personalizărilor |
| V4 — Produs operabil | Costuri, suport, mentenanță, securitate și drepturi validate; extinderea graduală a App Studio și a catalogului |

Programările pentru firme de servicii și un Recepționer AI sunt candidați buni, nu alegeri definitive. Verticala se alege după accesul la clienți reali și valoarea procesului. Nu presupunem existența clienților sau resursele unei echipe fără confirmare.

Primul produs nu este un ERP complet, o platformă universală de generare și un marketplace public construite simultan. Extragem SDK-ul și mecanismele comune din procese funcționale, înainte de extinderea masivă.

### 15.2 Scenarii obligatorii de verificare

| Scenariu | Rezultat așteptat |
|---|---|
| UI și AI execută aceeași acțiune | Aceleași reguli, drepturi și validări de business |
| Două companii și conexiuni distincte | Nicio traversare neautorizată a datelor, memoriei AI, fișierelor ori credențialelor |
| Permisiune revocată în timpul unei sarcini | Operațiunea ulterioară este blocată sau reevaluată |
| Payload modificat după aprobare | Aprobarea veche nu autorizează noul efect |
| Solicitări duplicate sau concurente | Nu apar efecte suplimentare necontrolate; regulile de consistență sunt respectate |
| Emitere FGO cu timeout | Status necunoscut și reconciliere; fără emitere într-un alt furnizor |
| FGO indisponibil sau conexiune revocată | Status explicit, reluare controlată sau cerere de reautorizare; CRM-ul rămâne utilizabil |
| Modificare externă și sincronizare întârziată | Proveniență și actualitate vizibile; conflicte tratate fără rescriere arbitrară |
| Capabilitate externă nesuportată | Funcția este indisponibilă explicit, nu simulată ca succes |
| Model AI indisponibil sau buget epuizat | Aplicațiile deterministe funcționează; sarcina așteaptă sau este preluată de om |
| Email cu instrucțiuni malițioase pentru agent | Conținutul nu extinde mandatul și nu dezactivează politicile |
| Upgrade de modul, conector sau agent | Configurările, extensiile și procesele în curs rămân compatibile ori sunt migrate controlat |
| Modificare de schemă și restaurare | Datele și legăturile sunt păstrate conform planului de recuperare verificat |
| Blockchain indisponibil | Dovezile sunt în așteptare, nu prezentate ca finalizate |
| Export și rulare separată | Soluția poate fi recuperată cu datele, artefactele și drepturile necesare |

Testele de acceptare sunt definite independent de generator, completate de teste de contract, securitate și operare. Trecerea lor oferă dovezi pentru scenariile verificate, nu o garanție de absență a tuturor defectelor.

Demonstrația decisivă este: aplicație de bază → personalizare A → modul reutilizabil → utilizare B → upgrade pentru ambele → recuperare verificată. Numărul aplicațiilor generate sau viteza primului demo nu sunt singure criterii de succes.

---

## 16. Riscuri și schimbări față de roadmap

| Risc | Răspuns structural |
|---|---|
| Framework propriu fără limită de scope | Strat intern subțire și extracție dintr-un proces pilot |
| Fork-uri independente pentru fiecare client | Compoziție, configurări și extensii cu compatibilități declarate |
| Ecosistem închis | Adaptoare externe, API-uri autorizate și portabilitate |
| „AI angajat” cu privilegii excesive | Identitate distinctă, mandat, politici server-side, bugete și escaladare |
| Dublă autoritate asupra datelor | Proprietar per entitate/câmp, mapări și snapshot-uri |
| Dublarea facturilor sau mesajelor | Intenții persistente, deduplicare, rezultat necunoscut și reconciliere |
| Personalizare revândută necorespunzător | Drepturi contractuale și pipeline separat de generalizare |
| Izolare insuficientă | Controale pe toate straturile și teste negative între companii |
| Dependențe sau licențe incompatibile | Verificare înainte de adoptare, adaptoare și plan de înlocuire |
| Promisiuni de încredere prea puternice | Model explicit de încredere, dovezi de build/deploy și verificabilitate independentă |
| Cost de mentenanță mai mare decât venitul | Măsurarea costului complet și limite comerciale ale personalizării |
| Migrare forțată a clienților existenți | Inventar, coexistență, mapare de date și tranziție aprobată |

Schimbări propuse de prioritizare:

- Verificarea securității și a stării reale precede utilizarea de producție a componentelor afectate. Prototiparea izolată a aplicațiilor nu depinde de finalizarea tokenomics.
- Evenimentele de business, contractele și procesele durabile devin fundația comună pentru aplicații, AI și integrări.
- AI Gateway deservește atât construcția, cât și operarea, cu privilegii separate; nu abandonăm accesul operațional la date în favoarea generatorului.
- Integration Layer este o componentă de prim rang. n8n sau alt instrument poate contribui, dar nu înlocuiește contractele și guvernanța datelor.
- Developer Portal și catalogul evoluează către module, conectori, pachete verticale și competențe AI, nu doar fișiere de specificație.
- Notificările și FGO se validează ca parte a unui flux real, nu ca infrastructură izolată fără utilizator.
- Marketplace-ul public, mecanismele economice complexe și extinderea pe multe industrii urmează validării produsului și operării.

Whitepaper-ul, pitch-urile, roadmap-ul istoric și contractele comerciale trebuie aliniate separat după aprobarea schimbărilor aplicabile. Acest document nu le modifică automat și nu transformă promisiunile lor în funcții implementate.

---

## 17. Registrul deciziilor și întrebărilor deschise

### 17.1 Principii asumate în discuție

| ID | Decizie de direcție |
|---|---|
| D01 | Reutilizăm componentele utile ale core-ului, cu verificarea stării lor reale |
| D02 | Construim aplicații native Bizix pentru o fundație comună, nu perpetuăm catalogul eterogen ca model principal |
| D03 | „Totul într-un singur loc” este o experiență integrată, fără obligația de a renunța la platformele externe |
| D04 | Interfețele, oamenii, AI-ul și automatizările folosesc aceleași capabilități și politici |
| D05 | Personalizările pot deveni produse reutilizabile, prin generalizare și drepturi explicite |
| D06 | Separăm pipeline-ul de personalizare de pipeline-ul de transformare în produs reutilizabil |
| D07 | Angajații AI sunt operatori cu mandat și identitate, separați de AI Constructor |
| D08 | Integrările API sunt parte structurală a platformei; CRM Bizix + FGO este un caz de referință |
| D09 | Deținem orchestrarea serviciilor comune și urmărim furnizori înlocuibili, nu absența tuturor dependențelor |
| D10 | Blockchain-ul susține dovezi și relații verificabile, fără a substitui regulile și securitatea aplicațiilor |
| D11 | Compatibilitatea, recuperarea și costul întreținerii sunt criterii de succes, alături de generare |

Asumarea unei direcții nu înseamnă validare comercială, implementare sau audit tehnic finalizat.

### 17.2 Alegeri care necesită validare

| Alegere | Ce trebuie clarificat înainte de fixare |
|---|---|
| Prima verticală și procesul pilot | Clienți accesibili, problemă reală, valoare și complexitate |
| Clienți și aplicații existente | Ce trebuie păstrat, conectat sau migrat; fără obligativitatea implicită a migrării |
| Structura echipei și operarea | Responsabili de dezvoltare, securitate, suport, email/SMS și chain |
| Framework și SDK intern | Cât construim și ce reutilizăm, pe baza aceleiași funcționalități pilot |
| Izolare și persistență | RLS, schemă sau bază per tenant și runtime dedicat; cost, migrări și restaurare |
| Workflow engine | Cerințe de durabilitate, versiuni, operare și licență |
| Email/SMS | Produse, licențe, transporturi, acorduri comerciale și livrabilitate |
| FGO și conectorii următori | Conturi autorizate, plan, capabilități, mapări, rate limits și comportament la erori |
| Primul angajat AI | Mandat, date, buget, evaluări, autonomie și escaladare |
| Reutilizarea personalizărilor | Licențe, exclusivitate, contribuții și împărțirea veniturilor |
| Trust și blockchain | Starea BiziX Chain, verificabilitatea independentă, chei, costuri și cerințe juridice |
| Oferta comercială | Preț, consum, suport, limite de personalizare și opțiuni de continuitate |

Nu sunt validate automat afirmațiile variantei inițiale despre framework complet de la zero, cost foarte mic per client, regenerare identică din spec, independență SMS prin SIM-uri, rollback universal sau calendar de implementare.

---

## 18. Surse și limitele verificării

### Documente interne consultate

- [Whitepaper Bizix](../README.md): arhitectură, aplicații, App Standard, AI, cloud privat, portabilitate și model economic.
- [TECH_INDEX](./TECH_INDEX.md) și [EXECUTIVE_SUMMARY](./EXECUTIVE_SUMMARY.md): raportarea istorică a stadiului componentelor.
- [Backend Core](./components/BIZIX_BACKEND_CORE_COMPONENT.md), [Bridge](./components/bizix_bridge.md), [Client Portal](./components/bizix_client_portal.md) și [Blockchain](./components/bizix_blockchain.md): descrieri tehnice, nu cod reverificat.
- [IMPLEMENTATION_ROADMAP](./IMPLEMENTATION_ROADMAP.md) și [IMPLEMENTATION_SUMMARY](./IMPLEMENTATION_SUMMARY.md): priorități istorice și neconcordanțe de verificat.
- [Raport audit whitepaper](./RAPORT_AUDIT_DETALIAT.md), [întrebări critice](./Q_AND_A_CRITIC.md) și [prezentarea pentru investitori](../bizix_investor_pitch_en.md): promisiuni și riscuri de aliniat.
- [Catalogul de aplicații MVP](../lista%20aplicatii%20MVP.csv): inventar eterogen de aplicații, plugin-uri și integrări.

### Surse publice consultate la 2026-09-12

- [FGO — integrare API](https://www.fgo.ro/integrare/api/) și [documentația API v7.0](https://api.fgo.ro/v1/testing.html): onboarding, medii, operațiuni, limitări și deduplicare.
- [Polkadot.js API](https://docs.polkadot.com/reference/tools/polkadot-js-api/) și [Dedot](https://docs.polkadot.com/reference/tools/dedot/): starea SDK-ului și alternativele.
- [Frappe — personalizarea DocTypes](https://docs.frappe.io/framework/user/en/basics/doctypes/customize): exemplu de personalizare separată de baza comună.
- [Temporal self-hosted](https://docs.temporal.io/self-hosted-guide): infrastructură disponibilă pentru procese durabile.
- [Postal — cerințe](https://docs.postalserver.io/getting-started/prerequisites/) și [Gmail — reguli pentru expeditori](https://support.google.com/mail/answer/81126): cerințe de operare și livrabilitate.
- [Stalwart — Enterprise](https://stalw.art/docs/server/enterprise/): funcții și condiții de licențiere relevante.
- [Jasmin — instalare și conectare SMSC](https://docs.jasminsms.com/en/latest/installation/index.html): gateway și conectare la transport.
- [Vodafone — politica de utilizare](https://www.vodafone.ro/personal/servicii-si-tarife/termeni-si-proceduri-legale/politica-de-utilizare-a-serviciilor/index.htm): exemplu de restricții ale serviciilor de consum; acordurile A2P se verifică separat.
- [n8n — integrare OEM](https://docs.n8n.io/deploy/host-n8n/deploy-as-an-oem-integration): diferența dintre încorporarea editorului și folosirea ca backend.
- [PostgreSQL — row security](https://www.postgresql.org/docs/current/ddl-rowsecurity.html): limitele și privilegiile relevante pentru RLS.
- [Structured Outputs](https://developers.openai.com/api/docs/guides/structured-outputs): conformitatea de schemă nu elimină greșelile de conținut.
- [Lovable — hosting și portabilitate](https://docs.lovable.dev/tips-tricks/deployment-hosting-ownership): generarea, exportul și găzduirea codului există deja în piață; diferențierea Bizix nu poate fi doar generarea.

Sursele externe descriu capabilități și condiții la momentul consultării. Ele nu dovedesc compatibilitatea, eligibilitatea comercială, performanța sau securitatea unei integrări Bizix. Acestea se stabilesc prin verificarea codului și experimente controlate, fără a expune date ori a produce efecte reale neautorizate.
