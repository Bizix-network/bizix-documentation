# S01 — Produs, economie și pilot: mini-brief

> **Versiune:** 0.5 — facturare directă: cumpărător punctual, linii punctuale și ciorne incomplete
> **Data:** 2026-09-14
> **Stare:** Mini-brief inițial; S01 nu este încă validat comercial sau aprobat pentru implementare.
> **Legături:** [Plan general](../README.md) · [Hartă UX S06](./06_SPATIU_DE_LUCRU_UX.md) · [Formatul de detaliere](../TEMPLATE_PLAN_SECTIUNE.md) · [Analiza de direcție](../../ANALIZA_DIRECTIE_AI_APPS.md)

Acesta este un format scurt de descoperire, nu planul detaliat complet al secțiunii. Separă răspunsurile confirmate de inițiator de ipotezele propuse pentru explorarea produsului și a experienței utilizatorului. Nu confirmă existența unei implementări funcționale.

## 1. Repere confirmate

Răspunsuri ale inițiatorului din 2026-09-13, nu rezultate ale unor interviuri cu clienți:

| Element | Confirmat |
|---|---|
| Stadiul actual Bizix | Doar dezvoltare internă; fără clienți externi în producție |
| Acces la o firmă pilot | Nu există încă un pilot identificat |
| Primul proces ales | Vânzare până la facturare |
| Primul rol AI ales | Asistent comercial |
| Scenariul primei hărți UX | Firmă mică de servicii B2B; ipoteză de lucru agreată, nevalidată cu un interlocutor real |
| Direcția facturării | Facturarea nativă Bizix este soluția principală și ținta de adopție; FGO și alte platforme sunt integrări complet opționale |

**Completări confirmate de inițiator la 2026-09-14, în revizuirea S02:**

| Element | Confirmat pentru prima versiune |
|---|---|
| Firma / tenantul | Un tenant reprezintă o singură entitate juridică; un utilizator poate avea acces autorizat la mai multe firme, fără partajarea implicită a datelor sau drepturilor |
| Facturarea directă | Inclusă alături de traseul din ofertă acceptată; nu necesită oportunitate sau ofertă și folosește aceleași validări și aprobări |
| Cumpărătorul facturii directe | Client existent sau cumpărător punctual; crearea/actualizarea profilului CRM este explicită și autorizată, nu un efect ascuns al facturării |
| Liniile facturii directe | Catalog și linii punctuale introduse de un om autorizat; AI-ul rămâne limitat la surse și prețuri autorizate; nu se creează implicit articole în catalog |
| Ciorne incomplete | Se pot salva cu lipsuri vizibile; datele furnizate respectă schema și drepturile, iar pregătirea pentru aprobare cere completare și validare |

Deciziile sunt înregistrate ca RC-001, RC-002 și RC-010–RC-012 în planul general și detaliate în [S02](./02_ARHITECTURA_CONTRACTE_DATE.md). Nu confirmă validarea comercială, disponibilitatea tehnică sau includerea avansurilor și facturării parțiale; acestea din urmă se delimitează în S07.

Absența clienților externi nu înseamnă că putem elimina datele, configurațiile sau rezultatele de dezvoltare interne. Acestea rămân de inventariat înainte de schimbări tehnice.

## 2. Clientul și problema: ipoteză de lucru

**Ipoteză aleasă pentru explorarea UX, nevalidată cu o firmă reală:** o firmă mică de servicii B2B, care gestionează solicitări, pregătește oferte și facturează serviciile acceptate. Exemple de investigat: agenție, consultant sau alt prestator de servicii.

Acest profil permite explorarea fluxului ales fără a introduce de la început stocuri, logistică ori POS. Nu limitează piața viitoare a Bizix și nu fixează încă un sector, dimensiunea firmei sau aplicațiile utilizate.

**Utilizator principal propus:** antreprenorul sau responsabilul comercial. Persoana care verifică oferta și aprobă facturarea poate avea un rol distinct sau poate fi aceeași persoană, în funcție de firmă.

**Problema presupusă, de verificat:** informațiile despre client, ofertă, următoarea acțiune și factură sunt dispersate; apar reintroduceri de date, follow-up-uri omise și dificultăți de urmărire a stadiului unei vânzări.

## 3. Promisiunea de produs

> Dintr-un singur spațiu de lucru, firma urmărește o solicitare comercială până la factura emisă în Bizix. Asistentul AI pregătește munca, omul păstrează controlul angajamentelor, iar integrarea cu un program extern rămâne o opțiune pentru firmele care au nevoie să îl păstreze.

Valoarea urmărită este reducerea muncii repetitive și a incertitudinii operaționale, nu simpla adăugare a unui chat sau înlocuirea forțată a tuturor aplicațiilor firmei. Beneficiile trebuie măsurate, nu presupuse ca demonstrate.

## 4. Fluxul de referință

```text
Contact / solicitare
    → Oportunitate și următoarea acțiune
    → Ofertă în ciornă
    → Verificare și trimitere aprobată
    → Acceptare consemnată
    → Intenție de facturare și aprobare
    → Emitere în Bizix implicit, sau extern numai la alegerea firmei
    → Confirmare sau reconciliere + istoric în CRM
```

Acceptarea ofertei se consemnează pe baza unei confirmări reale, nu este dedusă liber de AI. Fluxul trebuie să poată fi executat manual; AI-ul operează aceleași capabilități și reguli.

**Intrare suplimentară confirmată pentru prima versiune:** facturare directă → completarea cumpărătorului, liniilor și datelor comerciale/fiscale → intenție și aprobare → aceeași emitere și reconciliere. Nu cere oportunitate, ofertă sau acceptare fictivă. „Directă” descrie originea facturării, nu execuție fără control uman și nici schimbarea automată a sistemului de emitere. Capitolul 7 din S02 propune traseul și contractele. Ciorna poate fi incompletă, poate folosi cumpărător punctual și linii punctuale autorizate; lipsurile blochează pregătirea pentru aprobare, nu salvarea muncii valide. Datele fiscale obligatorii și UX-ul se definitivează în S07/S06.

**Facturarea nativă Bizix este traseul principal și implicit al produsului țintă.** Clientul trebuie să o poată folosi fără cont, credențiale sau abonament FGO. FGO și alte platforme sunt integrări complet opționale pentru adoptare graduală și păstrarea clienților care nu pot sau nu doresc încă să schimbe programul existent.

Obiectivul este ca firma să prefere în timp facturarea Bizix prin valoarea oferită. Trecerea este explicită și controlată, nu forțată; facturile vechi își păstrează originea, iar operațiunile în curs se reconciliază înainte de schimbarea sistemului pentru documente noi.

Minimul funcțional, cerințele fiscale aplicabile și momentul disponibilității în producție se stabilesc în S07. Facturarea nativă nu este exclusă din pilot prin principiu, dar nici nu este declarată deja implementată. Pentru explorarea UX folosim date fictive; un prototip nu emite facturi și nu contactează clienți reali.

Un timeout la emitere este un rezultat necunoscut, nu permisiune de a crea altă factură sau de a schimba automat furnizorul. Detaliile tehnice și comportamentul conectorului aparțin S08/S09.

## 5. Limitele primei versiuni explorate

**Incluse în ipoteza de produs:**

- spațiul firmei, roluri și acces la informațiile necesare;
- contacte, oportunități, oferte și următoarele acțiuni comerciale;
- ciorne pregătite de AI, verificare umană și Action Center;
- facturare nativă Bizix din ofertă acceptată și directă, fără oportunitate/ofertă: intenție, aprobare, document și status; integrare externă numai opțional;
- notificări autorizate și istoric comun al activității.

**Excluse din primul pilot:**

- ERP și contabilitate complete;
- stocuri, logistică, POS, salarizare și toate industriile simultan;
- plăți autonome, modificarea liberă a prețurilor sau condițiilor contractuale de către AI;
- campanii de marketing în masă și importul presupus complet al datelor oricărui furnizor;
- marketplace public și generator universal cu personalizare nelimitată.

Aceste limite se referă la pilot, nu la întreaga viziune Bizix. Un exemplu ulterior de personalizare reutilizabilă ar putea fi o regulă de aprobare a ofertelor sau un pachet de servicii; cererea reală și drepturile se validează înainte de transformarea în produs.

## 6. Mandatul inițial al asistentului comercial

| Poate pregăti sau propune | Nu primește implicit dreptul să facă |
|---|---|
| Rezumate din datele CRM și documentele autorizate | Să consulte datele altei firme sau să își extindă permisiunile |
| Ciorne de ofertă din servicii, prețuri și reguli aprobate | Să inventeze prețuri, discounturi ori angajamente |
| Următoarea acțiune și un follow-up | Să trimită mesaje externe fără autorizarea necesară |
| Actualizări CRM și pregătirea datelor de facturare | Să presupună acceptarea ofertei sau emiterea reușită a unei facturi |

Pentru prima variantă, trimiterea ofertei și emiterea facturii, fie în Bizix, fie extern, sunt precedate de aprobare explicită. Automatizarea unor acțiuni standard pe baza unui mandat preaprobat poate fi evaluată ulterior. Bugetele și limitele sunt aplicate de sistem, nu doar de instrucțiunile modelului.

## 7. Economia: ipoteze, nu prețuri stabilite

**Model comercial de explorat:** abonament pentru spațiul firmei și capabilitățile incluse, cu limite de consum explicite. Personalizările, extensiile și consumul suplimentar pot avea un regim separat. Modelul și disponibilitatea de plată nu sunt încă validate.

Costurile de urmărit: infrastructură, stocare, modele AI, comunicare, integrare, verificare umană, suport și mentenanță. Abonamentele externe plătite direct de client se disting de costurile suportate de Bizix.

Nu stabilim încă un preț, o marjă, un volum de clienți sau un buget AI numeric. Înainte de ofertarea pilotului trebuie clarificate volumul operațiunilor, cine operează serviciul, costurile reale și limitele comerciale. Generarea ieftină a codului nu implică mentenanță gratuită.

## 8. Cum verificăm valoarea

| Aspect | Ce urmărim |
|---|---|
| Claritatea UX | Facturarea Bizix este traseul implicit; utilizatorul găsește următoarea acțiune și înțelege ce aprobă, unde este factura și ce înseamnă un status în așteptare |
| Independența facturării native | Fluxul Bizix poate fi folosit fără cont, credențiale sau apeluri FGO; integrarea externă se activează numai prin alegere explicită |
| Efortul operațional | Timp activ și reintroduceri de date pentru același proces, comparate cu modul actual de lucru al firmei pilot |
| Utilitatea AI | Ciorne acceptate, corecții și intervenții necesare; dacă reduce sau doar mută munca omului |
| Controlul procesului | Oportunități fără următoare acțiune, aprobări întârziate și rezultate externe nereconciliate |
| Sustenabilitatea | Cost complet pe proces și pe client, inclusiv suport și mentenanță, raportat la disponibilitatea de plată |

Acum putem verifica schematic coerența fluxului și a ecranelor. Siguranța execuției se demonstrează ulterior prin teste, iar valoarea comercială prin discuții și utilizare reală. Pragurile de acceptare se stabilesc după obținerea unei referințe de lucru, înainte de acceptarea pilotului; nu sunt rezultate deja obținute.

## 9. Prima hartă UX de lucru și continuarea prototipării

Explorarea S06 folosește acest mini-brief, în paralel cu rafinarea S01:

- **Hartă globală:** înregistrare, configurarea firmei, activarea modulelor, conectarea serviciilor, activitate zilnică, AI/aprobări, personalizare și administrare/export.
- **Prototip schematic aprofundat, încă necreat:** fluxul vânzare → ofertă → facturare și intrarea prin facturare directă confirmată la 2026-09-14, nu toate aplicațiile platformei.
- **Zone inițiale de explorat:** vedere de ansamblu, contacte/oportunități, detaliul ofertei, Action Center, integrări și documentul/statusul facturii. Nu reprezintă încă o listă definitivă de ecrane.
- **Stări necesare:** fără date, în lucru, refuz, acces interzis, aprobare în așteptare, conexiune indisponibilă și rezultat necunoscut.

Prima hartă este creată în [FigJam](https://www.figma.com/board/8eaisIfYFnggwsgt0ME29P), cu vederea globală și detaliul fluxului pilot din ofertă. Facturarea directă nu este încă reprezentată în aceste diagrame; confirmarea cerinței nu înseamnă că board-ul a fost actualizat. Sursele și regulile de interpretare sunt păstrate în [S06](./06_SPATIU_DE_LUCRU_UX.md). Prototipul cu ecrane și interacțiuni rămâne o etapă distinctă, încă necreată. Designul vizual final și implementarea urmează după feedback. Constatările UX actualizează S01 și contractele din S02.

## 10. Ce rămâne de clarificat

1. Ce nevoi și constrângeri ale unei firme reale confirmă sau infirmă ipoteza aleasă?
2. Ce firmă sau interlocutor reprezentativ putem implica și cum lucrează astăzi?
3. Ce sistem de facturare și alte instrumente folosește efectiv?
4. Care este principala problemă pentru care ar plăti: efort, follow-up-uri, erori sau vizibilitate?
5. Ce volum de operațiuni, responsabilități umane și costuri putem susține?
6. Ce personalizare reală ar fi utilă și unui al doilea client?
7. Ce date și justificări cere facturarea directă și ce funcții suplimentare — avansuri, facturare parțială, recurență sau corecții — trebuie incluse în minimul S07?

Lipsa unui pilot nu blochează explorarea schematică, dar nu permite să declarăm produsul sau UX-ul validate de piață. Ipoteza pentru explorare este aleasă. Următorii pași sunt revizuirea hărții UX, wireframe-uri pentru fluxul ales și identificarea unui interlocutor real pentru verificarea nevoilor.
