# S02 — Arhitectură, contracte și date

> **Versiune:** 0.6 — aliniere cu desemnarea, transferul și recuperarea S03 v0.3; schemele comerciale propuse în v0.3 rămân neschimbate
> **Data:** 2026-09-15
> **Stare:** În revizuire; direcții și cerințe confirmate conform capitolului 9, fără aprobarea integrală a S02 sau autorizarea implementării.
> **Implementare:** Neverificată; au fost consultate documentele, nu codul sau deployment-urile componentelor.
> **Legături:** [Plan general](../README.md) · [Mini-brief S01](./01_PRODUS_ECONOMIE_PILOT.md) · [Drepturi minime S03](./03_IDENTITATE_ACCES_IZOLARE.md) · [Hartă UX S06](./06_SPATIU_DE_LUCRU_UX.md) · [Template](../TEMPLATE_PLAN_SECTIUNE.md) · [Analiza de direcție](../../ANALIZA_DIRECTIE_AI_APPS.md)

Acest document definește direcția și propune contractele noii platforme pentru fluxul contact → oportunitate → ofertă → aprobare → factură și pentru facturarea directă, fără oportunitate sau ofertă. Nu este schema finală a bazei de date, specificația fiscală a facturării sau un inventar tehnic verificat. Denumirile de entități, capabilități și evenimente sunt propuneri, nu API-uri existente.

## Identificare și stare

| Câmp | Valoare |
|---|---|
| ID și titlu | S02 — Arhitectură, contracte și date |
| Versiune și data revizuirii | 0.6 / 2026-09-15 |
| Stare document | În revizuire; confirmările din capitolul 9 nu validează automat contractele detaliate |
| Stare implementare | Neverificată în repository-urile de implementare |
| Livrabil și etapă generală | M0: arhitectură de referință, proprietatea datelor și contractele minime pentru M1/M2; extensii delimitate pentru M3/M4 |
| Responsabil de produs | De desemnat |
| Responsabil tehnic | De desemnat |
| Responsabil de verificare și operare | De desemnat, împreună cu S03/S04/S16 |
| Intrări de referință | S01 v0.5, S03 v0.3 pentru drepturi, autoritate și continuitatea accesului, S06 v0.4, planul general v0.10; documentația istorică numai ca sursă de inventar |
| Secțiuni dependente | S03–S16 consumă contractele relevante; nu se cere finalizarea lor integrală pentru primul flux |

**Repere confirmate în S01:** dezvoltare internă, fără clienți externi în producție; niciun pilot identificat; proces vânzare până la facturare; primul AI este asistent comercial; facturare Bizix principală și integrări externe opționale. Firma mică de servicii B2B este ipoteza aleasă pentru explorare, nu un profil validat comercial.

**Confirmări ale inițiatorului la revizuirea din 2026-09-14:** un tenant reprezintă o singură entitate juridică; prima versiune include și facturare directă; nucleul modular este direcția pentru funcțiile noi; PostgreSQL este candidatul principal de evaluat; regulile D03–D07 sunt confirmate ca cerințe de proiectare. La detalierea capitolului 7 au fost confirmate și cumpărătorul existent sau punctual, catalogul plus liniile punctuale și salvarea ciornelor incomplete. Capitolul 9 precizează limitele confirmării și corespondența cu registrul general RC-001–RC-012.

**Statutul detaliilor:** modelul concret, denumirile, schemele, bibliotecile, topologia și mecanismele de execuție rămân propuneri de verificat. Acordul asupra direcției și cerințelor nu confirmă că implementarea le îndeplinește, nu aprobă integral S02 și nu autorizează operațiuni tehnice sau comerciale.

## 1. Obiectiv și valoare

S02 trebuie să prevină construirea unor aplicații aparent unite printr-un meniu, dar cu date, aprobări și reguli incompatibile. Un contact folosit într-o ofertă, o acțiune pregătită de AI și factura rezultată trebuie să poată fi urmărite în același proces, fără acces direct și necontrolat între bazele modulelor.

Rezultatul urmărit este un set restrâns de contracte prin care:

- omul, AI-ul și workflow-ul cer aceeași operațiune de business, cu aceleași validări și politici;
- fiecare informație are un proprietar, o proveniență și o regulă de actualizare;
- aprobarea privește exact efectul executat, nu o ciornă care se poate schimba ulterior;
- restartul, concurența sau un timeout nu produc automat alte facturi ori mesaje;
- modulele și personalizările pot evolua fără copierea întregii soluții pentru fiecare firmă.

Documentația veche descrie în principal provisioning, identitate, comenzi de platformă și portal. Nu avem în această redactare dovada unui proces comercial nativ complet. Succesul S02 se verifică prin acoperirea fluxului cu proprietari și contracte explicite, apoi prin probele din capitolul 11. Latențele, volumele și costurile acceptabile se stabilesc în S01/S04/S16 înainte de acceptarea livrabilului tehnic.

## 2. Scope și granițe

| Inclus în livrabilul curent | Exclus sau amânat | Responsabilitate externă |
|---|---|---|
| Granițe logice, reguli de colaborare și opțiunea inițială de structurare | Topologie finală, dimensionare și deployment | S04, pe baza probelor |
| Model minim pentru fluxul comercial și proprietatea datelor | Model universal pentru toate industriile, ERP și contabilitate complete | S01/S07 pentru extinderea produsului |
| Contextul comun de companie, actor și mandat | Modelul complet de roluri, delegare, izolare și politici de date | S03 |
| Forma contractelor, disponibilitate declarată și compatibilitate | Implementarea registrului, instalărilor și rutării | S05 |
| Date și stări pe care UI-ul trebuie să le poată prezenta | Ecrane, wireframe-uri și design system | S06 |
| Contracte pentru oferte și facturare, inclusiv proveniență | Calcul fiscal, serii, corecții și cerințe e-Factura/SPV aplicabile | S07; S09 pentru adaptarea externă |
| Intenție, aprobare, execuție și evenimente ca modele distincte | Alegerea și implementarea motorului durabil | S08 |
| Interfața comună și limitele unui adaptor | Implementarea FGO sau a altui conector și verificarea API-ului său | S09 |
| Referințe la comunicări, fișiere, consum și dovezi | Transporturi, AI, trust și facturarea abonamentului Bizix | S10/S11/S14/S15 |
| Reguli minime pentru extensii, export și migrare | App Studio complet, marketplace și operațiuni de migrare | S12/S13/S16 |

S02 definește contractele; secțiunile responsabile le rafinează și le implementează fără a schimba unilateral sensul comun. O schimbare transversală se reflectă aici și în planurile afectate. Facturarea afacerii clientului din S07 nu se confundă cu facturarea serviciilor Bizix din S15.

## 3. Starea actuală și reutilizarea

Sursele de mai jos au fost citite pentru această redactare la 2026-09-14. Data citirii nu reprezintă verificarea tehnică a afirmațiilor istorice.

| Element existent sau documentat | Sursă | Stare constatată | Direcție propusă și probă necesară |
|---|---|---|---|
| Brief și flux comercial | S01 v0.5 și S06 v0.4 | Documente de lucru; fără pilot comercial și fără prototip interactiv declarat | Reutilizăm reperele; verificăm excepțiile cu un interlocutor real |
| Core Node.js/Express, MongoDB, comenzi, provisioning, BullMQ | [Backend Core](../../components/BIZIX_BACKEND_CORE_COMPONENT.md), document din 2025-12-06 | Descriere istorică; codul, testele și deployment-ul nu au fost inspectate | Candidat de adaptare prin API; verificăm contractele, autorizarea, persistența și licențele înainte de reutilizare |
| Keycloak/OIDC, Bridge și portal | [Inventar tehnic](../../TECH_INDEX.md) și analiza de direcție | Existența și calitatea funcțiilor sunt de reverificat; UI-ul Action Center nu dovedește execuția durabilă | Candidați de reutilizare în S03/S05/S06; fără înlocuire sau acceptare automată |
| Infrastructură și blockchain | Inventarul tehnic și analiza de direcție | Afirmații istorice, inclusiv neconcordanțe privind securitatea | Evaluare în S04/S14; fără dependență implicită a fiecărei scrieri de business de blockchain |
| Date și configurări interne | S01 confirmă dezvoltarea internă | Inventar tehnic încă nefăcut | Păstrare până la clasificare, export și autorizarea explicită a schimbărilor |

Nu se transferă procentele istorice de progres în starea reconstrucției. M0 trebuie să completeze inventarul cu repository, revizie, mediu, teste și constatări concrete. Accesul la implementări blochează decizia de reutilizare, nu redactarea arhitecturii țintă.

## 4. Dependențe și contracte cu alte secțiuni

| Intrare sau ieșire | Secțiune responsabilă | Contract necesar | Condiție înainte de implementarea dependentă |
|---|---|---|---|
| Flux și limite comerciale | S01 | Scenariul de referință, actorii și excluderile | Confirmarea livrabilului ales; ipotezele comerciale rămân vizibile |
| Context de execuție și acces | S03 | Tenant, actor efectiv, delegare, resurse, aprobare și revocare | Verificare server-side și model de amenințări pentru flux |
| Persistență și livrare | S04 | Tranzacții locale, secrete, medii, backup, observabilitate | Probă de concurență, izolare și restaurare pentru opțiunea aleasă |
| Registru și rutare | S05 | Manifest de modul/capabilitate, versiuni și instalări | Refuz explicit al funcțiilor indisponibile sau incompatibile |
| Experiența de lucru | S06 | Referințe stabile, stări, actualitate, efect aprobat și următoarea acțiune | UI-ul nu deduce succesul din simpla acceptare a cererii |
| Domeniu comercial | S07 | Entități, validări, revizii, snapshot-uri și rezultate de facturare | Stabilirea minimului funcțional și fiscal înainte de emitere reală |
| Procese durabile | S08 | Intenții, aprobări, operațiuni, sarcini, outbox și deduplicare | Probe de restart, redelivery, concurență și rezultat necunoscut |
| Integrare opțională | S09 | Conexiune, mapări, proveniență, capabilități și reconciliere | Validarea exactă a funcțiilor oferite de fiecare furnizor |
| Fișiere și notificări | S10 | Referință autorizată, versiune de document, comandă de trimitere și status | Separarea cererii acceptate de livrarea confirmată |
| Operare AI | S11 | Adaptor peste capabilități, mandat și consum | Nicio cale privilegiată de acces; fluxul manual funcționează independent |
| Extensii și distribuire | S12/S13 | Configurări și artefacte versionate, compatibilitate și drepturi | Fără fork implicit sau acces liber la datele altor module |
| Dovezi și consum | S14/S15 | Referințe la operațiuni, versiuni, unități de consum și proveniență | Auditul nu este confundat cu dovada blockchain sau cu evidența comercială |
| Acceptare și tranziție | S16 | Matrice de probe, export, mapări și continuitate | Acceptare explicită pentru fiecare livrabil și mediu |

## 5. Scenarii de utilizare

Toate scenariile sunt cerințe de verificat, nu operațiuni deja executate. Contextul companiei și mandatul se verifică pentru fiecare pas, inclusiv când procesul este reluat de un worker.

| Scenariu | Actor și declanșator | Date și acțiuni | Rezultat și excepții |
|---|---|---|---|
| SC-01 — Flux manual nativ | Comercialul autorizat lucrează o solicitare a firmei | Contact, oportunitate, ofertă revizionată, aprobare de trimitere, acceptare cu dovadă, intenție de facturare și aprobare de emitere | Factură Bizix numai după confirmare; lipsurile păstrează ciorna sau sarcina deschisă; fără cont FGO |
| SC-02 — Ciornă AI | Asistentul comercial primește o sarcină în mandat | Citește numai date autorizate; propune oferta din servicii și condiții aprobate prin aceleași capabilități | Ciornă cu proveniență; trimiterea și emiterea cer aprobarea explicită prevăzută în S01; AI indisponibil permite preluare manuală |
| SC-03 — Refuz sau date schimbate | Aprobatorul refuză ori comercialul modifică oferta | Motiv de refuz sau revizie nouă; intenția executabilă veche nu se rescrie | Fără efectul refuzat; schimbările relevante cer o nouă aprobare și, unde este cazul, o nouă acceptare comercială |
| SC-04 — Dublu clic sau doi operatori | UI, AI ori API repetă aceeași intenție în același tenant | Cheie de idempotență, identitatea efectului comercial și versiunea așteptată | O operațiune existentă sau conflict explicit; o cheie nouă nu ocolește protecția efectului comercial |
| SC-05 — Facturare externă opțională | Firma a activat explicit un sistem extern | Aceeași intenție aprobată, cu sistemul și conexiunea fixate; adaptorul execută numai funcții suportate | Document cu proveniență externă; timeout înseamnă rezultat necunoscut și reconciliere, nu emitere în Bizix |
| SC-06 — Reluare și revocare | Workerul reia după restart ori un om preia sarcina | Încarcă starea persistentă și reverifică drepturile, aprobarea și datele | Acțiunea încă neexecutată este blocată dacă autorizarea nu mai este validă; efectele deja produse nu sunt anulate fictiv |
| SC-07 — Contact modificat după emitere | Comercialul corectează adresa clientului | CRM actualizează profilul curent; documentele păstrează snapshot-urile | Factura și oferta acceptată nu se rescriu; corecția fiscală este un proces separat în S07 |
| SC-08 — API extern autorizat | Un integrator cere o acțiune pentru o firmă | Aceleași contracte, validări, limite și audit ca în UI; identitate tehnică și delegare explicită | Fără scriere directă în tabele; o capabilitate nepublicată sau neautorizată este refuzată |
| SC-09 — Facturare directă | Utilizatorul autorizat pornește o facturare fără oportunitate sau ofertă | Datele cumpărătorului, linii, condiții și profil de facturare; cerere de facturare persistentă, intenție validată și aprobare explicită | Aceleași reguli de emitere și reconciliere; fără ofertă/acceptare fictivă și fără pierderea identității facturării la reluare |
| SC-10 — Utilizator în două firme | Același utilizator selectează alt tenant pentru care are apartenență | Contextul activ și drepturile se reverifică; fiecare tenant are un singur emitent juridic | Nicio partajare implicită de clienți, facturi, conexiuni sau aprobări între firme |

## 6. Structură și date

### 6.1 Arhitectura logică și separarea fizică

**Direcție confirmată S02-D01 / RC-003, la 2026-09-14:** un nucleu de business modular pentru aplicațiile noi, cu interfețe interne explicite, nu câte un microserviciu obligatoriu pentru fiecare modul. API-ul și workerii pot fi procese distincte din aceeași bază de cod. Microserviciile existente rămân candidate de integrare, fără rescriere pentru uniformitate.

Avantajul urmărit este reducerea dependențelor operaționale inițiale. Compromisul este că unele module pot împărți resursele și ciclul de livrare; granițele trebuie impuse prin convenții și verificări. Un modul logic nu înseamnă automat un serviciu separat, o bază de date separată sau un produs comercial vândut separat.

```text
Spațiul de lucru / AI Operator / workflow / API autorizat
                         |
      Context verificat + contracte + politici + rutare
                         |
       CRM        Oferte        Facturare Bizix
                         |             |
           Procese și aprobări   Adaptor extern opțional
                         |
       Notificări / fișiere / audit / consum / trust

Core existent → adaptoare/API → funcțiile reutilizate după verificare
AI Constructor → mediu separat → artefacte verificate → publicare
```

Schema reprezintă colaborări, nu o ordine obligatorie pentru orice apel. Citirile și modificările locale simple nu trebuie transformate în workflow-uri distribuite. Operațiunile care așteaptă oameni sau implică efecte externe au stare durabilă.

- Fiecare modul deține modelul și regulile sale; poate modifica numai propriile date prin implementarea sa.
- Alte module folosesc capabilități și evenimente, nu ORM-uri, tabele sau modele interne importate direct. Citirile compuse folosesc API-uri ori proiecții autorizate, nu join-uri necontractate peste stocarea internă a altui modul.
- O bază fizică comună pentru modulele noi este o opțiune, nu permisiunea de a ignora proprietatea datelor. Schema-per-module și izolarea per tenant sunt două decizii diferite.
- AI Constructor, codul extensiilor neîncrezute și funcțiile de infrastructură privilegiate necesită granițe de execuție și privilegii distincte, detaliate în S03/S04/S12. Un import TypeScript nu reprezintă o izolare de securitate.
- Nu se decide aici câte repository-uri trebuie să existe. Separarea fizică ulterioară se justifică prin securitate, încărcare, operare sau cicluri de livrare independente și se verifică pe contractele păstrate.

### 6.2 Module și proprietatea datelor

„Proprietar” în tabel înseamnă autoritatea tehnică asupra regulilor și scrierilor, nu dreptul juridic de proprietate asupra datelor clientului.

| Domeniu logic | Date și responsabilități proprii | Ce nu deține |
|---|---|---|
| Identitate și context de firmă | `Tenant`, identități, apartenențe și mandate; S03 definește politica, S05 integrează referințele în runtime | Identitatea clientului comercial, oferte și facturi |
| CRM | `CustomerAccount`, `Contact`, `Opportunity`, activități comerciale și legături către proces | Documentele emise, aprobările și execuția sarcinilor durabile |
| Oferte | `Quote`, revizii, linii comerciale, condiții și `QuoteAcceptance`; propunere: catalogul minim de servicii/prețuri aprobate al domeniului comercial, accesibil și facturării prin contract | Presupunerea acceptării din lipsa răspunsului; regulile fiscale ale facturării |
| Facturare | Profilul de facturare al firmei, `BillingRequest` din ofertă sau directă, pregătirea și validarea payload-ului de emitere, identitatea efectului comercial; `Invoice` nativ și regulile sale | Aprobarea generică, motorul procesului și secretele conexiunilor externe |
| Procese durabile | `ActionIntent`, `Approval`, `Operation`, `Task`, tranziții și evidența încercărilor; S08 | Calculul prețurilor, taxelor sau acceptării comerciale |
| Integrări | `Connection`, `ExternalObjectMapping`, proiecții de facturi externe, checkpoint-uri și dovezile reconcilierii; S09 | Autoritatea asupra facturii emise de furnizor ori suprascrierea liberă a CRM-ului |
| Servicii comune | S10: fișiere versionate și comunicări; S05: audit comun; S15: consum; S14: dovezi | Rescrierea deciziilor și rezultatelor modulelor de business |
| Runtime de platformă | Manifestele, instalările, versiunile active și rutarea către implementări; S05 | Acordarea automată a permisiunilor prin simpla instalare |

O intenție de facturare este o specializare a `ActionIntent`: S07 construiește și validează conținutul de business, S08 îl persistă și gestionează aprobarea/execuția. Această delimitare detaliază formularea „Workflow Bizix” din analiza de direcție, fără a muta logica fiscală în motorul de workflow. Similar, CRM-ul descrie următoarea acțiune comercială, iar o sarcină executabilă și starea ei au autoritate în S08; CRM și Action Center sunt consumatori, nu două registre independente de sarcini.

**RC-011 confirmă folosirea catalogului și a liniilor punctuale introduse de un om autorizat.** Structura catalogului, plasarea sa în domeniul comercial, versionarea intrărilor, moneda, valabilitatea și drepturile de modificare rămân propuneri de detaliat în S07. Folosirea catalogului nu obligă utilizatorul să creeze o ofertă, iar o linie punctuală nu adaugă implicit un articol în catalog. Limitele negocierii se stabilesc în S03/S07; AI-ul nu inventează prețuri sau reguli. Nu se creează implicit un produs complex de catalog ori o copie concurentă în facturare.

### 6.3 Modelul minim și relațiile

**Model de firmă confirmat — RC-001:** în prima versiune, un tenant reprezintă o singură entitate juridică. Un utilizator poate avea apartenențe și drepturi diferite în mai mulți tenants; schimbarea firmei active nu unește datele sau permisiunile. Un grup cu mai multe entități juridice într-un singur tenant rămâne în afara primei versiuni și necesită o decizie ulterioară explicită. S03/S05 detaliază identitatea și apartenențele, iar S07 profilul emitentului; CUI-ul nu devine cheie de autorizare.

Entitățile de business, intențiile, aprobările, operațiunile și referințele de mai jos poartă explicit `tenant_id`, inclusiv când tabelul nu îl repetă. Definițiile globale de module și identitățile care pot aparține mai multor firme sunt distincte de datele operaționale ale unui tenant; accesul la acestea se stabilește în S03/S05.

| Entitate | Câmpuri și relații minime propuse | Invariantă principală |
|---|---|---|
| `Tenant` | ID intern, stare; referință spre o singură entitate juridică și configurațiile ei | Firma care utilizează Bizix, nu clientul ei comercial; un utilizator poate accesa mai mulți tenants numai prin apartenențe autorizate |
| `CustomerAccount` | ID, tenant, denumire, date comerciale/fiscale curente și versiune | În scenariul B2B reprezintă firma cumpărătoare; același CUI nu unește automat conturile din tenants diferiți |
| `Contact` | ID, tenant, nume, date de contact, versiune și legătură cu contul B2B unde se aplică | Reprezintă persoana cu care se discută, nu implicit destinatarul fiscal; emailul nu înlocuiește ID-ul și nu acordă acces |
| `Opportunity` | Client, contacte, responsabil, stadiu, următoarea acțiune și referințe la oferte/sarcini | Referințele aparțin aceluiași tenant; stadiul comercial nu este status de execuție a facturării |
| `Quote`, `QuoteRevision` | Oportunitate, revizie, vânzător/cumpărător ca snapshot, linii, monedă, condiții, valabilitate și versiunea regulilor de calcul | O revizie trimisă sau acceptată nu se modifică în loc; corectarea creează altă revizie |
| `QuoteAcceptance` | Referința reviziei, momentul consemnării, actorul care o consemnează și referința dovezii | Acceptarea clientului comercial este distinctă de aprobarea internă de trimitere sau emitere; se cere pentru traseul din ofertă, nu se inventează pentru factura directă |
| `BillingRequest` | ID stabil, tenant, versiune, origine `accepted_quote` sau `direct`, cumpărător și linii, bază comercială și profil; referință la acceptare numai pentru originea din ofertă | Cerere de facturare deținută de S07, fără serie/număr fiscal; păstrează identitatea facturării înaintea aprobării și între intențiile succesoare |
| `ActionIntent` | Tip și versiune de capabilitate, resurse/revizii, payload fixat, identitatea efectului, inițiator, țintă de execuție și stare; la emitere, referință la `BillingRequest` și versiunea ei | Payload-ul supus aprobării este imuabil; o corecție creează o intenție succesoare legată de aceeași identitate a efectului |
| `Approval` | Intenție, amprenta conținutului aprobat, aprobator, decizie, motiv, moment, expirare și politica evaluată | Nu poate autoriza alt tenant, alt payload, alt destinatar sau alt sistem decât cel aprobat |
| `Operation` | Intenție, ID, stare persistentă, încercări, rezultat/eroare, corelare și referințe la confirmări | Acceptarea cererii nu echivalează cu succesul efectului; reluările păstrează legătura cu aceeași operațiune |
| `Invoice` nativ | ID intern, intenție și efect comercial, emitent/destinatar și linii ca snapshot, serie/număr, date relevante, monedă, totaluri și fișiere | S07 deține regulile și tranzacția emiterii; nu se prezintă ca emis înainte de confirmarea persistentă |
| Proiecție de factură externă | Sistem, conexiune și mediu, identificator extern, intenție, date raportate, referință de document, ultima verificare și stare de sincronizare | Copia locală nu devine factura nativă și nu este sursa autoritativă a documentului extern |
| `Task` | Proces/intenție, tip, responsabil ori rol eligibil, stare, scadență și referințe autorizate | Preluarea de către om schimbă responsabilitatea în același proces, nu creează implicit alt efect |
| `FileReference`, referințe de audit/consum/dovadă | Tenant, obiect/versiune, clasificare și relație cu operațiunea | Referința nu acordă acces; fișierul, auditul și dovada blockchain sunt obiecte distincte |

Cardinalități inițiale: un client poate avea mai multe contacte și oportunități; o oportunitate poate avea mai multe oferte; o ofertă are mai multe revizii. Pentru traseul din ofertă, trimiterea, acceptarea și facturarea indică revizia exactă; facturarea directă nu cere această referință. O intenție poate avea decizii și încercări istorice, fără ca fiecare încercare să însemne o nouă intenție comercială sau o factură nouă.

**Scope confirmat — RC-002:** prima versiune de facturare include atât traseul din ofertă acceptată, cât și emiterea directă, fără oportunitate sau ofertă. Ambele ajung la aceeași pregătire de intenție, aprobare și execuție; factura directă nu ocolește validările comerciale/fiscale și nu produce acceptări fictive.

`BillingRequest` este propunerea tehnică pentru păstrarea unei identități comerciale stabile în ambele trasee, nu o entitate deja implementată sau aprobată ca schemă finală. **RC-010 confirmă cumpărătorul existent sau punctual:** datele pot proveni din CRM ori pot fi completate autorizat numai pentru document; crearea și actualizarea profilului CRM sunt acțiuni explicite, nu efecte ascunse ale facturării. **RC-012 permite ciorne incomplete**, dar nu pregătirea pentru aprobare sau emiterea până la completarea și validarea datelor. Modelul concret de câmpuri și contractele propuse sunt în capitolul 7. Pentru originea `accepted_quote`, cererea indică acceptarea și revizia exactă. Pentru `direct`, acestea nu se cer, iar baza comercială și datele obligatorii se validează conform S07. Nu se deduce confirmarea unei prestații dintr-o propunere AI.

Se păstrează ca exemplu inițial facturarea integrală a unei cereri. Acompturile, facturarea parțială, recurența și corecțiile rămân de delimitat în S07; includerea lor nu rezultă din confirmarea facturării directe. Dacă intră în livrabil, S07 definește tranșele/documentele și identitatea fiecărui efect legitim înainte de implementare. O ofertă cu mai multe revizii sau acceptări nu autorizează automat facturări integrale repetate ale aceluiași angajament comercial.

### 6.4 Convenții comune de date

- **Identificatori:** opaci și stabili, generați de sistem; formatul fizic se fixează odată cu persistența. Referințele verifică tenantul, tipul obiectului și existența sa. Seria/numărul facturii și identificatorul extern nu înlocuiesc ID-ul Bizix.
- **Mapări externe:** unicitate în contextul `(tenant_id, connection_id, environment, object_type, external_id)`; se păstrează originea și legătura internă. Un ID extern din alt cont sau mediu nu identifică același obiect.
- **Bani și cantități:** valori zecimale exacte; în contractele JSON sunt șiruri zecimale, nu numere floating-point. Moneda este explicită. Precizia, rotunjirea, taxele și validarea totalurilor aparțin S07 și se păstrează cu versiunea regulilor; UI-ul și AI-ul nu recalculează autoritativ totalurile.
- **Timp:** momente tehnice ISO 8601 în UTC; datele comerciale/fiscale și fusul orar relevant se păstrează separat. `occurred_at`, `recorded_at` și `last_verified_at` nu sunt interschimbabile.
- **Versiuni:** versiunea entității, revizia documentului, versiunea contractului, versiunea modulului și versiunea configurării sunt concepte distincte. Modificările folosesc versiunea așteptată și refuză suprascrierea concurentă neobservată.
- **Snapshot-uri:** valorile și referințele versionate necesare documentului sau aprobării se fixează la momentul relevant. Schimbarea ulterioară a unui contact, preț, fișier sau profil de facturare nu schimbă retroactiv efectul aprobat.
- **Proiecții:** datele derivate au sursă, versiune și moment de actualizare. Pot fi reconstruite; nu sunt folosite ca dovadă suficientă pentru o acțiune sensibilă fără reverificarea sursei.
- **Conflicte:** nu se aplică implicit „ultima scriere câștigă” asupra documentelor, aprobărilor sau sumelor. Conflictul se respinge ori produce o sarcină de reconciliere.

### 6.5 Persistență, actualitate și ciclul de viață al datelor

Modificarea unui agregat și intenția de publicare a evenimentelor sale se persistă în aceeași tranzacție locală, prin outbox sau mecanism echivalent verificat. Nu se declară tranzacție atomică globală între CRM, notificări și un furnizor extern. Un pas ulterior eșuat nu șterge fictiv pașii deja confirmați.

După o comandă locală, răspunsul include referința și versiunea confirmate. Listele și istoricul pot întârzia; UI-ul arată actualitatea și poate interoga sursa autorizată. Pentru emitere ori aprobare nu se folosește exclusiv o proiecție întârziată.

Datele comerciale, datele personale, documentele fiscale, secretele și telemetria se clasifică separat. S03/S07 stabilesc termenele de retenție, obligațiile de păstrare și condițiile de ștergere; nu se inventează aici un termen legal unic. Ștergerea unui contact nu șterge în cascadă factura, dovada acceptării sau istoricul necesar. Arhivarea, anonimizarea și ștergerea sunt operațiuni diferite, aplicate și copiilor, indexurilor, memoriei AI și backup-urilor conform politicii stabilite.

Exportul păstrează identificatori, relații, proveniență, versiuni, documente și configurări. Nu exportă implicit credențiale. Datele personale și payload-urile complete nu sunt copiate automat în loguri sau pe blockchain. Amprentele și referințele sensibile necesită și ele o politică de acces și retenție.

## 7. Capabilități, API-uri și evenimente

### 7.1 Forma comună a unui contract

**Cerință confirmată S02-D03 / RC-005:** UI, AI și API folosesc aceleași contracte și reguli de business, cu autorizarea fiecărui actor. **Propunere tehnică de verificat:** contracte JSON validate, OpenAPI pentru HTTP și scheme versionate pentru mesaje. SDK-ul și eventualul adaptor MCP consumă aceleași definiții și implementări de business. Confirmarea cerinței nu selectează definitiv OpenAPI, Node.js/TypeScript, bibliotecile sau forma manifestului; exemplele de aici nu sunt o specificație OpenAPI completă.

| Element | Contract propus |
|---|---|
| Identitate | `name`, `contract_version`, `owner_module`, versiuni compatibile și stare de disponibilitate |
| Input și output | Scheme explicite cu câmpuri obligatorii/opționale, limite, unități și referințe; erori tipizate |
| Context verificat | `tenant_id`, `actor_id`, `actor_type`, delegare/mandat când se aplică, `correlation_id`; construite sau validate server-side |
| Autorizare | Drept funcțional, resurse accesibile, mandat, politică și aprobare necesară; nu doar un rol generic de administrator |
| Efect | Citire, modificare locală, comunicare externă ori efect sensibil; resursele modificate și evenimentele produse |
| Consistență | Versiune așteptată, limite tranzacționale, cheie de idempotență și identitatea efectului comercial unde se aplică |
| Execuție | Sincron sau durabil; rezultat imediat versus referință de operațiune; timeout și reconciliere |
| Audit și consum | Legături cu actorul inițiator și executant, intenția, aprobarea, versiunea și unitățile relevante, fără secrete |
| Evoluție | Compatibilități, deprecări, consumatori afectați și teste obligatorii |

Tenantul sau actorul transmise de un client nu devin autoritate prin simpla prezență în JSON. Gateway-ul și implementarea verifică identitatea, apartenența și accesul la resurse; workerul își folosește propria identitate tehnică și păstrează inițiatorul/delegarea. Identificatorii de corelare nu sunt dovezi de autorizare.

### 7.2 Catalogul inițial de capabilități

Toate contractele din tabel sunt propuse în versiunea `1`. Disponibilitatea lor reală rămâne neconfirmată. Câmpurile enumerate definesc modelul semantic minim; schemele executabile exhaustive se livrează în pasul P3, înainte de implementarea consumatorilor.

| Capabilitate | Proprietar și input principal | Output și efect | Control principal |
|---|---|---|---|
| `crm.customer.read` | CRM; referința clientului | Date autorizate, sursă și versiune; numai citire | Acces la tenant, obiect și câmpuri |
| `crm.customer.create`, `crm.customer.update` | CRM; date validate, iar la update ID și versiune așteptată | Referință și versiune; eveniment de creare/modificare | Drept de scriere; fără unire automată după email/CUI |
| `crm.opportunity.create`, `crm.opportunity.update` | CRM; client, responsabil, stadiu; versiune la update | Oportunitate și versiune | Referințe din același tenant, tranziții validate |
| `sales.quote.create`, `sales.quote.revise` | Oferte; oportunitate, linii, condiții și revizia de bază unde se aplică | Ciornă/revizie și totaluri validate | Prețuri și condiții autorizate; fără modificarea reviziei trimise |
| `sales.quote.send.prepare` | Oferte; revizie, destinatari, canal, conținut și fișiere versionate | Intenție de trimitere pentru aprobare; fără mesaj extern | Conținut complet fixat; executarea folosește S10 |
| `sales.quote.acceptance.record` | Oferte; revizie, confirmare și dovadă accesibilă | Acceptare consemnată și eveniment | Drept explicit de consemnare; AI-ul nu inventează confirmarea |
| `invoicing.request.create`, `invoicing.request.update` | Facturare; origine, date de ciornă, iar la update ID, versiune așteptată și modificări explicite; acceptarea se cere numai pentru originea `accepted_quote` | `BillingRequest` persistentă, versiune, validare și lipsuri; fără emitere sau număr fiscal | Ciorne incomplete permise, nu date structurale invalide ori scrieri neautorizate; idempotență și concurență controlate |
| `invoicing.request.read` | Facturare; `billing_request_id` | Ciornă autorizată, versiune, proveniență, actualitatea calculelor și referințe la intenție/operațiune | Nu reconstruiește aprobarea dintr-o proiecție și nu reemite nimic |
| `invoicing.intent.prepare` | Facturare + S08; `billing_request_id`, versiunea așteptată și predecesorul explicit la reluare | Intenție cu snapshot imuabil, țintă fixată și stare `pending_approval`; fără emitere | Validare completă și reguli de domeniu disponibile; ambele origini folosesc aceeași pregătire |
| `workflow.intent.read` | S08; `intent_id` | Versiune, stare, snapshot-ul autorizat și sumarul efectului pentru aprobare | Snapshot-ul este cel fixat, nu ciorna curentă; lipsa accesului la datele necesare împiedică aprobarea |
| `workflow.approval.decide` | S08; `intent_id`, versiune așteptată, `snapshot_id`, decizie și motiv | Decizie persistentă, `approval_id`, versiunea și starea rezultată a intenției | S03 verifică aprobatorul și politica; nu execută implicit efectul în aceeași cerere |
| `workflow.intent.execute` | S08; intenție, aprobare și versiune așteptată | Operațiune durabilă, rutată spre handlerul autorizat | Reverificare, rezervare atomică a efectului și interzicerea execuțiilor concurente necontrolate |
| `invoicing.invoice.read` | Facturare, prin sursa nativă sau proiecția S09 | Document/status, proveniență și moment de verificare | Citire autorizată; nicio emitere implicită |
| `workflow.operation.read` | S08; ID operațiune | Stare, rezultat/eroare, actualitate și următorul pas | Inclusiv rezultatele memorate sunt filtrate prin drepturile curente |
| `workflow.operation.reconcile` | S08 cu handlerul S07/S09/S10; ID operațiune | Dovezi noi și tranziție confirmată sau menținerea incertitudinii | Reconcilierea nu este un alias pentru retrimitere/reemitere |

Intenția indică și capabilitatea de efect: `sales.quote.send` pentru trimiterea reviziei prin S10, respectiv `invoicing.issue` pentru emitere prin implementarea configurată. Handlerul primește payload-ul fixat și contextul verificat, iar rezultatul este o referință de comunicare sau factură și confirmarea efectului, după caz. `workflow.intent.execute` gestionează execuția durabilă, nu înlocuiește autorizarea specifică acestor capabilități și nu acordă dreptul de a executa orice tip de intenție.

Handlerul de emitere sau trimitere verifică și el intenția autorizată; accesul intern nu poate ocoli aprobarea prin apel direct. Dacă pregătirea unei intenții implică mai multe module, crearea ei se deduplică pe identitatea efectului, versiunile sursei și predecesorul explicit când se cere un succesor, fără presupunerea unei tranzacții distribuite. O intenție incompletă nu devine executabilă. Operațiunile de consultare/listare a oportunităților, ofertelor, intențiilor și sarcinilor se detaliază în P3 împreună cu S06, cu filtrare autorizată, paginare și actualitate explicită; catalogul de mai sus nu este inventarul exhaustiv al API-ului UI.

### 7.2.1 Datele cererii de facturare directă

**Confirmări ale inițiatorului, 2026-09-14:** cumpărător existent sau punctual, catalog plus linii punctuale introduse de om autorizat și salvarea ciornelor incomplete — RC-010–RC-012. Forma câmpurilor de mai jos este propunerea de aplicare, nu schema fiscală finală sau un API implementat.

| Grup / câmp propus | Pentru salvarea ciornei | Înainte de pregătirea intenției | Autoritate și reguli |
|---|---|---|---|
| `origin` | Obligatoriu: `direct` sau `accepted_quote`; nu se schimbă prin update | Originea și baza comercială sunt verificate | Pentru `accepted_quote`, referința la acceptare este obligatorie și autorizată încă de la creare; pentru `direct` nu se cere și nu se inventează |
| Context, `billing_request_id`, `version` | Tenant/actor verificați; ID și versiune generate de sistem | Se cere versiunea așteptată a aceleiași cereri | Nu sunt câmpuri editabile ale documentului; identificatorii clientului nu acordă acces |
| `billing_profile_ref` | Poate lipsi; când există conține ID și versiune accesibile | Obligatoriu, complet și valid pentru tenantul și emitentul selectate | S07 deține profilul; sistemul de emitere și configurația de numerotare rezultă din configurarea autorizată |
| `buyer` | Poate lipsi; când există declară exact un mod: `customer_ref` sau `one_off` | Datele necesare cumpărătorului sunt complete și valide conform scenariului fiscal | Referință CRM cu ID/versiune sau date punctuale; nu se amestecă modurile pentru a suprascrie tacit profilul CRM |
| `lines` | Poate lipsi sau fi listă goală; câmpurile furnizate respectă schema | Cel puțin o linie completă pentru exemplul inițial de factură de servicii | Backend-ul validează descrierea, cantitatea, unitatea, prețul, reducerile și tratamentul fiscal aplicabil |
| `currency` | Poate lipsi; dacă există, cod monetar valid | Monedă explicită, suportată de scenariul livrat | O singură monedă a documentului în propunerea inițială; conversiile nu sunt deduse liber de AI |
| `proposed_issue_date`, `payment_terms` | Pot lipsi sau fi incomplete în limitele schemei | Data și condițiile cerute de S07 sunt rezolvate și afișate aprobatorului | Data documentului este distinctă de timestamp-ul tehnic; o dată devenită nepermisă cere remediere, nu înlocuire tacită la execuție |
| `commercial_basis` | Poate fi completată progresiv; descriere și referințe la dovezi, unde există | Justificarea și dovezile cerute de S07 sunt verificate | Facturarea directă nu deduce prestarea unui serviciu dintr-un răspuns AI; dovezile nu sunt obligatoriu un document de ofertă |
| Rezultate calculate | Nu se acceptă totaluri autoritative din input | Totalurile și regulile de calcul aplicate sunt determinate de backend | Output: `validation`, `calculation`, proveniență și versiuni; nu sunt editabile prin API-ul de ciornă |

Lista nu stabilește universal câmpurile obligatorii ale unei facturi fiscale. Identificatorii fiscali, adresa, datele prestației, scadența, regimul taxelor și alte cerințe se concretizează în S07 pentru jurisdicția și scenariul livrate. Emailul cumpărătorului nu devine obligatoriu pentru emitere doar pentru că poate exista un pas ulterior de trimitere. Aprobarea emiterii nu autorizează implicit și comunicarea externă.

**Cumpărător:** `buyer.mode = customer_ref` cere `customer_id` și `customer_version`; sistemul citește datele autorizate și păstrează proveniența. `buyer.mode = one_off` conține `details`, cu denumire, adresă și identificatori aplicabili, completabili progresiv în ciornă. Salvarea, pregătirea și emiterea nu creează sau actualizează implicit `CustomerAccount` ori `Contact`. Transformarea unui cumpărător punctual în client reutilizabil folosește o acțiune CRM explicită, cu drepturile și deduplicarea sa; legarea rezultatului la ciornă este un update versionat, nu modificarea retrospectivă a documentului emis.

**Linii și prețuri:** pentru inputul direct, fiecare linie furnizată declară `kind = catalog` sau `custom`. Varianta `catalog` indică `catalog_item_id`, `item_version` și `price_ref` cu ID-ul și versiunea prețului; backend-ul rezolvă valorile, nu acceptă un preț arbitrar prezentat ca fiind cel din catalog. Varianta `custom` permite descriere și `pricing = {source: manual, amount, basis}` unui om cu drepturile necesare. `basis` distinge explicit prețul net de cel brut în scenariile suportate. Cantitatea și prețul sunt șiruri zecimale exacte, iar moneda aparține documentului. `tax_treatment_ref` și reducerile se validează prin regulile S07; lipsa tratamentului fiscal nu înseamnă taxă zero.

Backend-ul păstrează autorul și proveniența schimbărilor, fără a avea încredere într-un `created_by` furnizat de client. AI-ul poate reutiliza surse/prețuri autorizate și date deja pregătite de om în mandat, dar nu poate introduce ori schimba un preț manual prin simpla setare a `source = manual` sau inventarea unui ID de catalog. Excepțiile și limitele negocierii se stabilesc în S03/S07/S11. Schimbarea prețului unui serviciu de catalog nu modifică implicit catalogul și nu trebuie prezentată fals ca preț de catalog nemodificat.

Pentru originea `accepted_quote`, conținutul comercial provine din revizia acceptată, nu dintr-un payload liber care doar citează ID-ul ofertei. Diferențele față de condițiile acceptate cer rezolvare în domeniu și, unde este necesar, o nouă acceptare; aprobarea internă a facturii nu înlocuiește acceptarea clientului comercial.

### 7.2.2 Salvare, validare și contractele pașilor

`BillingRequest` este document de lucru, nu factură emisă. Starea sa de validare este distinctă de starea intenției, decizia de aprobare și rezultatul execuției. Outputul de citire/salvare include ID, versiune, datele autorizate și:

- `validation.state`: `incomplete`, `invalid`, `valid` sau `unavailable`, cu `missing_fields`, `issues` și momentul/versiunea evaluării;
- `calculation.state`: `incomplete`, `calculated` sau `unavailable`; totalurile și versiunea regulilor apar numai când au fost calculate pe datele precizate;
- referința intenției curente și a operațiunii, dacă există, fără confundarea lor cu validarea ciornei.

`valid` descrie evaluarea datelor la acel moment, nu o permisiune de emitere și nici aprobarea omului. Un total afișabil nu face completă o cerere căreia îi lipsesc date despre cumpărător. Dacă regulile fiscale nu sunt implementate/verificate pentru scenariu ori un calcul nu este disponibil, nu se declară cererea pregătită pentru aprobare. Nu se înlocuiesc valorile necunoscute cu zero.

| Contract | Input și rezultat minim propus | Validare și efect |
|---|---|---|
| `invoicing.request.create` | Anvelopă cu cheie de idempotență; `input` conține originea și datele de ciornă; rezultat: ID, versiunea inițială și validare | Permite lipsuri, dar refuză schema invalidă, referințele inaccesibile și încălcările deja determinabile. Persistă numai cererea; nu emite, nu cere automat aprobare și nu scrie în CRM/catalog |
| `invoicing.request.update` | ID, `expected_version`, `patch` cu câmpurile editabile; rezultat: aceeași cerere, versiune nouă și evaluare actualizată | Câmpul omis rămâne neschimbat; `null` golește numai câmpuri de ciornă declarate opționale. Obiectele și listele furnizate înlocuiesc valorile de la acel nivel, fără merge ascuns. `lines` înlocuiește lista, cu ID-uri de linie verificate sau generate pentru linii noi. Nu poate schimba tenantul, originea, ID-ul, rezultatele calculate sau cheia efectului |
| `invoicing.intent.prepare` | ID cerere și versiune așteptată; la reluare deliberată, `supersedes_intent_id` și `reason`; rezultat: `intent_id`, `intent_version`, `status = pending_approval`, `snapshot_id` și sumar autorizat | Recitește autoritatea datelor, verifică toate cerințele și fixează snapshot-ul, ținta și versiunile. Lipsurile sau regulile indisponibile blochează pasul; nu creează o intenție executabilă parțial |
| `workflow.intent.read` | ID intenție; rezultat: versiune, stare, snapshot și sumarul efectului autorizat | Aprobatorul vede datele fixate, nu o reconstrucție din ciorna curentă. Dacă nu poate vedea datele necesare deciziei, nu poate aproba acel efect |
| `workflow.approval.decide` | ID intenție, versiune așteptată, `snapshot_id`, `decision = approve` sau `reject` și motiv; rezultat: decizie, `approval_id`, versiune/stare nouă a intenției | Motivul refuzului este obligatoriu în propunere. Backend-ul verifică snapshot-ul, aprobatorul și politica S03; o decizie nouă este admisă numai în `pending_approval`, iar deciziile concurente sunt serializate. Înregistrarea deciziei nu emite factura |
| `workflow.intent.execute` | ID intenție, versiune așteptată și `approval_id`; rezultat: operațiune persistentă și status | Se execută numai când politica de aprobare este satisfăcută, sursa este încă admisibilă și efectul poate fi rezervat sigur. Inputul nu poate înlocui cumpărătorul, sumele, sistemul sau payload-ul aprobat |
| `workflow.operation.read`, `workflow.operation.reconcile` | ID operațiune; rezultat: stare și confirmări/referințe autorizate | Citirea nu produce efectul. Reconcilierea verifică aceeași execuție; nu reemite sub alt ID și nu transformă timeout-ul în eșec confirmat |

**Reluarea deliberată pentru reaprobare:** o intenție refuzată, expirată, revocată sau înlocuită nu se redeschide prin repetarea lui `approve`. Propunerea pentru `prepare` permite un succesor numai cu `supersedes_intent_id` care indică ultimul predecesor eligibil și un motiv explicit, păstrând aceeași cerere și identitate a efectului. S07/S08 verifică faptul că nu există efect rezervat, în curs, necunoscut ori emis și serializează înlocuirea intenției curente. Astfel, o aprobare expirată poate fi cerută din nou fără modificări fictive ale datelor. Repetarea idempotentă întoarce înregistrarea existentă, inclusiv starea sa închisă; o cheie nouă fără această tranziție explicită nu creează o altă intenție pentru același efect. Schema stărilor și eligibilitatea exactă se validează în S08 înainte de implementare.

Scrierile folosesc idempotență și versiunea așteptată unde modifică un obiect existent. Inputul furnizat greșit este refuzat, nu salvat ca o ciornă „incompletă” pentru a ocoli validarea. O ciornă anterior corectă poate deveni `invalid` sau `unavailable` dacă sursele, politicile ori regulile se schimbă; această situație este vizibilă. Schema executabilă va fixa limitele de dimensiune, precizie, colecții și câmpuri pentru fiecare contract înainte de implementare.

### 7.2.3 Snapshot-ul aprobat și modificările concurente

La pregătire se păstrează împreună: tenantul și emitentul, ID-ul și versiunea cererii, originea și referința comercială aplicabilă, cumpărătorul, liniile/prețurile/reducerile/taxele, moneda și totalurile, datele documentului, baza comercială și dovezile versionate relevante, profilul de facturare și configurația numerotării, sistemul/mediul de emitere și conexiunea dacă este externă, capabilitatea și versiunile regulilor. Numărul fiscal rezultat nu este inventat pentru aprobare; momentul atribuirii sale aparține regulilor S07. Snapshot-ul nu conține secrete.

`snapshot_id` identifică o înregistrare imuabilă administrată de sistem, cu amprenta și versiunea de canonicalizare păstrate server-side. ID-ul singur nu este o dovadă criptografică și nu acordă autorizare. Aprobarea este legată de înregistrarea exactă și de intenția afișată; serverul nu acceptă alt snapshot doar fiindcă clientul îi trimite ID-ul. Reprezentarea canonică și calculul amprentei rămân de fixat și testat.

Propunerea de control al concurenței este:

1. S07 deține versiunea cererii și o stare durabilă de protecție a execuției. Update-ul datelor și rezervarea pentru emitere trebuie să concureze printr-o verificare atomică asupra aceleiași autorități, nu doar prin citirea versiunii urmată mai târziu de apelul extern.
2. La `prepare`, validarea S07 și persistarea intenției S08 se leagă prin ID, versiune și înregistrarea controlată a asocierii. Dacă versiunea s-a schimbat între pași, intenția nu devine aprobabilă/executabilă. O întrerupere lasă o înregistrare recuperabilă, nu o a doua intenție activă pentru același efect; nu se presupune o tranzacție distribuită implicită.
3. Cât timp nu există o rezervare de execuție, update-ul autorizat poate salva o nouă versiune a cererii. Intenția veche nu mai poate fi executată; pregătirea ulterioară creează o intenție succesoare și cere reaprobare. Sincronizarea stării afișate prin evenimente nu este singura protecție: handlerul verifică autoritatea S07.
4. Dacă execuția a rezervat deja efectul, modificarea datelor de emitere este blocată. Pentru `queued`, `running`, `unknown` și succes confirmat nu se eliberează rezervarea prin simpla expirare a unui lease, prin editare sau prin alegerea altui furnizor. Rezervarea poate fi închisă/redeschisă numai prin tranziții controlate care confirmă rezultatul și, când se permite o nouă încercare, neexecuția anterioară.
5. O aprobare consemnată concomitent cu schimbarea sursei rămâne cel mult o decizie istorică asupra snapshot-ului vechi; nu permite trecerea de verificarea versiunii și rezervării la execuție. Drepturile, mandatul și aprobarea se reverifică înaintea dispatch-ului.
6. Rezervarea S07 și operațiunea S08 se corelează printr-un ID stabil al operațiunii. Dispatch-ul este interzis până când ambele înregistrări durabile sunt confirmate. Dacă procesul se întrerupe între ele, reluarea idempotentă sau reconcilierea internă completează aceeași asociere; absența temporară a uneia dintre înregistrări nu permite generarea altui efect. Alegerea tranzacției locale coordonate ori a protocolului de asociere se validează în P3/S08, inclusiv pentru această fereastră de eroare.

Pentru datele selectate din surse curente, dacă profilul CRM sau prețul de catalog s-a schimbat înainte de pregătire față de versiunea aleasă, sistemul cere rezolvare explicită; nu folosește tacit altă valoare. Această regulă nu înlocuiește prețurile și condițiile unei oferte acceptate cu cele din catalogul curent: originea `accepted_quote` folosește conținutul comercial acceptat și îi verifică valabilitatea și cerințele fiscale aplicabile. După fixarea snapshot-ului, schimbarea profilului curent nu rescrie documentul aprobat. Precondițiile legale, drepturile și validitatea efectului sunt totuși reverificate; dacă nu mai sunt îndeplinite, execuția se oprește fără substituirea datelor aprobate. O factură deja emisă se corectează prin procesul S07, nu prin editarea cererii originale.

### 7.3 Exemple de contract și rezultate

Exemplele folosesc date și identificatori fictivi. Arată forma semantică propusă, nu payload-uri fiscale complete, scheme executabile sau răspunsuri obținute dintr-un backend. Contextul de identitate este separat și verificat server-side. Anvelopa este ilustrativă și independentă de alegerea finală a rutelor HTTP.

#### 7.3.1 Salvarea unei ciorne directe incomplete

Un om autorizat introduce un cumpărător punctual și o linie manuală; lipsesc intenționat date necesare pregătirii. Alegerea RON și a bazei nete este doar exemplificativă, nu validarea suportului fiscal sau monetar al implementării.

```json
{
  "capability": "invoicing.request.create",
  "contract_version": 1,
  "idempotency_key": "demo-draft-001",
  "input": {
    "origin": "direct",
    "buyer": {
      "mode": "one_off",
      "details": { "display_name": "Client demonstrativ SRL" }
    },
    "currency": "RON",
    "lines": [
      {
        "kind": "custom",
        "description": "Serviciu demonstrativ",
        "quantity": "1",
        "unit_code": "demo_unit",
        "pricing": { "source": "manual", "amount": "1200.00", "basis": "net" }
      }
    ],
    "commercial_basis": { "description": "Referinta comerciala demonstrativa" }
  }
}
```

`demo_unit` reprezintă o unitate dintr-un nomenclator fictiv al testului, nu un cod fiscal presupus valid în producție. Un răspuns ilustrativ prescurtat este:

```json
{
  "billing_request_id": "billing_request_demo_001",
  "version": 1,
  "validation": {
    "state": "incomplete",
    "missing_fields": ["billing_profile_ref", "buyer.details.address", "lines[0].tax_treatment_ref"],
    "issues": []
  },
  "calculation": { "state": "incomplete" }
}
```

Lipsurile sunt exemplificative și nu înlocuiesc lista completă derivată din S07. Acest răspuns nu conține `invoice_id`, număr fiscal sau aprobare. Totalul nu este presupus `1200.00` doar pentru că taxele lipsesc. Cumpărătorul și serviciul punctual nu sunt adăugate automat în CRM sau catalog.

#### 7.3.2 Pregătirea intenției și consemnarea aprobării

Presupunem că un update autorizat a completat cererea, versiunea ei este acum `2`, iar regulile S07 și profilul sunt disponibile și validate în mediul de test. Nu deducem aceste condiții din exemplul de ciornă de mai sus.

```json
{
  "capability": "invoicing.intent.prepare",
  "contract_version": 1,
  "idempotency_key": "demo-prepare-001",
  "input": {
    "billing_request_id": "billing_request_demo_001",
    "expected_version": 2
  }
}
```

Rezultat prescurtat după înregistrarea reușită a asocierii cu versiunea cererii:

```json
{
  "intent_id": "intent_demo_001",
  "intent_version": 1,
  "billing_request_id": "billing_request_demo_001",
  "billing_request_version": 2,
  "snapshot_id": "snapshot_demo_001",
  "status": "pending_approval"
}
```

Aprobatorul citește intenția și efectul complet autorizat prin `workflow.intent.read`, apoi consemnează decizia asupra acelui snapshot:

```json
{
  "capability": "workflow.approval.decide",
  "contract_version": 1,
  "idempotency_key": "demo-approve-001",
  "input": {
    "intent_id": "intent_demo_001",
    "expected_version": 1,
    "snapshot_id": "snapshot_demo_001",
    "decision": "approve"
  }
}
```

În exemplu, politica aplicabilă este satisfăcută de această aprobare, rezultatul include `approval_id = approval_demo_001`, `intent_version = 2` și `status = approved`. Acesta nu este un drept universal ca orice utilizator să aprobe sau ca orice politică să fie satisfăcută de o singură decizie. La `reject`, se cere motiv și nu se creează operațiunea de emitere. Identitatea aprobatorului și amprenta snapshot-ului sunt stabilite de sistem, nu declarate liber în input.

#### 7.3.3 Solicitarea execuției și urmărirea rezultatului

După aprobarea validă, clientul cere execuția aceleiași intenții, fără să retransmită sau să înlocuiască datele facturii.

```json
{
  "capability": "workflow.intent.execute",
  "contract_version": 1,
  "idempotency_key": "demo-submit-001",
  "input": {
    "intent_id": "intent_demo_001",
    "expected_version": 2,
    "approval_id": "approval_demo_001"
  }
}
```

Răspunsul de acceptare înseamnă că operațiunea a fost înregistrată durabil, nu că factura a fost emisă:

```json
{
  "operation_id": "operation_demo_001",
  "intent_id": "intent_demo_001",
  "status": "queued",
  "correlation_id": "correlation_demo_001"
}
```

Pentru contractul propus, citirea operațiunii întoarce `operation_id`, `intent_id`, `status`, `updated_at` și `correlation_id`; `result`, `error` și `next_action` sunt prezente după caz. Rezultatul confirmat al emiterii include referința facturii, sistemul emitent și referința confirmării, nu doar un boolean `success`.

| Stare de operațiune | Semnificație și comportament |
|---|---|
| `queued` | Cerere acceptată durabil; efectul nu este confirmat și poate să nu fi început |
| `running` | Încercare în curs; clientul urmărește aceeași operațiune |
| `succeeded` | Efect confirmat și rezultat persistent, cu proveniență |
| `failed` | Eșec confirmat, fără efectul cerut; nu se folosește pentru timeout ambiguu |
| `unknown` | Nu știm dacă efectul s-a produs; reconciliere și fără retry orb |
| `cancelled` | Neexecutarea este confirmată în urma anulării; nu descrie o factură deja emisă |

Așteptarea aprobării, refuzul și expirarea aparțin intenției/aprobării, nu se reduc la aceste stări de execuție. `reconciling` poate fi o activitate/sarcină asociată unei operațiuni `unknown`; UI-ul păstrează vizibilă incertitudinea până la confirmare. S08 definitivează tranzițiile, inclusiv comportamentul încercărilor și al intervenției umane.

Erorile au `code`, `message` sigur pentru destinatar, `field_errors` unde se aplică și o indicație de remediere. Coduri inițiale: `VALIDATION_FAILED`, `ACCESS_DENIED`, `NOT_FOUND`, `VERSION_CONFLICT`, `APPROVAL_REQUIRED`, `APPROVAL_STALE`, `IDEMPOTENCY_CONFLICT`, `CAPABILITY_UNAVAILABLE`, `UNSUPPORTED_OPERATION` și `DEPENDENCY_UNAVAILABLE`. Un răspuns ambiguu după transmiterea unui efect produce starea `unknown`, nu o eroare generică interpretată drept permisiune de reexecuție. S03 stabilește când refuzul se prezintă ca resursă inexistentă pentru a nu divulga informații.

#### 7.3.4 Erori și remediere pentru facturare

Erorile de mai jos completează codurile comune. Răspunsul arată câmpurile afectate numai în limitele accesului; nu include secrete, payload-uri externe brute sau date ale altui tenant. Un cod de eroare nu autorizează singur un retry.

| Cod propus | Când apare | Remediere și efect |
|---|---|---|
| `VALIDATION_FAILED` | Schema, formatul, valoarea furnizată sau o regulă deja evaluabilă este încălcată | Refuză scrierea afectată; corectare explicită. O referință inaccesibilă urmează regulile S03 de refuz/nedivulgare |
| `REQUEST_INCOMPLETE` | Se cere `prepare`, dar lipsesc date necesare | Ciorna rămâne salvată; se întorc lipsurile autorizate, fără intenție pregătită sau emitere |
| `DOMAIN_RULES_UNAVAILABLE` | Calculul ori validarea cerută de scenariul fiscal nu este disponibilă | Fără etichetă `valid` și fără pregătire/emitere; necesită implementare sau restabilirea serviciului verificat |
| `SOURCE_CHANGED` | Datele CRM, catalogul, oferta sau profilul nu mai corespund versiunii selectate pentru pregătire | Actualizare/rezolvare explicită și reevaluare, nu preluarea tacită a altor date |
| `VERSION_CONFLICT` | Cererea ori intenția a avansat față de versiunea așteptată | Recitire autorizată și rezolvarea conflictului, fără suprascriere oarbă |
| `APPROVAL_REQUIRED`, `APPROVAL_STALE` | Lipsește aprobarea necesară ori snapshot-ul, versiunea sau valabilitatea ei nu mai corespund | Nu se execută; se revizuiește intenția și se cere aprobarea corectă |
| `EFFECT_LOCKED` | Se încearcă schimbarea datelor sau o nouă execuție peste un efect rezervat, în curs, necunoscut ori confirmat | Se urmărește operațiunea existentă; reconciliere sau proces de corecție, nu eliberare prin editare |
| `IDEMPOTENCY_CONFLICT` | Aceeași cheie de cerere este reutilizată cu alt input semantic | Nu se execută noul payload. Recuperarea cererii originale și o nouă modificare legitimă se tratează separat |
| `PRECONDITION_FAILED` | La execuție nu mai sunt îndeplinite condiții de domeniu pentru efectul aprobat | Blocare explicită; nu se modifică automat data, taxele, destinatarul sau sistemul pentru a forța emiterea |

`DEPENDENCY_UNAVAILABLE` înainte de orice posibil dispatch nu dovedește același lucru ca un timeout după transmitere. Dacă operațiunea ar putea fi executată, rezultatul rămâne `unknown`, cu ID-ul ei stabil și reconciliere. Nici eroarea de răspuns către UI după acceptarea cererii nu dovedește neexecutarea; clientul repetă idempotent cererea sau citește aceeași operațiune.

### 7.4 Aprobare, idempotență și concurență

1. **Intenție fixată:** aprobarea include tenantul, capabilitatea și versiunea semantică, resursele/reviziile, destinatarii, conținutul și documentele versionate, sumele, moneda, profilul și sistemul de execuție. Pentru extern include identitatea conexiunii și mediul, fără secrete. Reprezentarea canonică și calculul amprentei se fixează și se testează înainte de implementare; un hash singur nu dovedește cine a aprobat.
2. **Revalidare:** executantul verifică aprobarea, expirarea/revocarea, drepturile actuale, mandatul, resursele și precondițiile. Nu substituie date curente diferite în payload-ul aprobat. O schimbare materială cere intenție succesoare și reaprobare.
3. **Deduplicarea cererii:** `(tenant_id, capability, contract_version, idempotency_key)` identifică cererea și amprenta inputului său semantic. Aceeași cheie cu alt input produce conflict; repetarea aceleiași cereri întoarce operațiunea existentă, numai după autorizarea accesului. Identitatea actorului nu permite ocolirea deduplicării prin schimbarea canalului.
4. **Unicitatea efectului:** separat de cheia de transport, domeniul stabilește `business_effect_key`. Propunerea inițială leagă emiterea integrală de `(tenant_id, billing_request_id, effect_kind)`, atât pentru origine `direct`, cât și pentru `accepted_quote`. Crearea cererii este idempotentă; pentru traseul din ofertă, S07 împiedică cereri concurente pentru aceeași facturare legitimă și stabilește cum se păstrează identitatea între revizii/acceptări. S08 rezervă atomic efectul, iar handlerul nativ verifică unicitatea și la persistarea facturii. Intențiile succesoare păstrează cheia; o nouă intenție, o nouă cheie de transport sau alt furnizor nu eliberează un efect în curs, confirmat ori necunoscut.
5. **Ordinea verificărilor:** după autorizare, repetarea exactă a unei cereri deja acceptate poate recupera aceeași operațiune chiar dacă versiunea entității a avansat. O cerere nouă trebuie să treacă verificarea versiunii așteptate. Două execuții concurente folosesc o tranziție/rezervare atomică, nu doar un control în UI.
6. **Încercări și restart:** înainte de efect se persistă intenția, ținta și identitatea încercării. O încercare întreruptă după posibilul dispatch nu se consideră neexecutată doar fiindcă a expirat un lease. Se caută rezultatul nativ sau se reconciliază extern înainte de retrimitere.
7. **Retry controlat:** numai după confirmarea neexecuției și verificarea datelor, drepturilor și aprobării cerute de flux; limite și backoff în S08/S09. Rezultatul necunoscut rămâne vizibil cât timp probele sunt insuficiente. Se interzice fallback-ul automat între Bizix și un furnizor extern.
8. **Retenție și anulare:** fereastra de deduplicare și evidența durabilă a efectelor se stabilesc împreună; expirarea cache-ului nu transformă o factură existentă într-o operațiune nouă. Revocarea sau anularea după dispatch nu pot garanta retragerea efectului; dacă rezultatul nu este cunoscut, urmează reconciliere. O corecție fiscală nu este rollback tehnic.

Pentru facturarea directă, două cereri create deliberat ca operațiuni distincte nu pot fi declarate automat aceeași obligație numai din egalitatea sumelor sau descrierilor. S07 stabilește avertismentele și verificarea suspiciunilor de dublare; nu promitem deduplicare semantică universală. Identitatea cererii trebuie păstrată la dublu clic, restart și reluarea aceleiași facturări, nu regenerată la fiecare încercare.

Garanțiile externe depind de API-ul verificat. Fără idempotență sau interogare sigură la furnizor, poate fi necesară intervenția umană. Nu se promite execuție „exactly once” globală și nu se presupune suport pentru căutare după ID extern sau webhook-uri.

### 7.5 Evenimente și proiecții

Un eveniment descrie un fapt confirmat de producător, nu o comandă implicit autorizată. Anvelopa propusă conține `event_id`, `event_type`, `schema_version`, `tenant_id`, `producer`, `aggregate_type`, `aggregate_id`, `aggregate_version`, `occurred_at`, `recorded_at`, `correlation_id`, `causation_id` și `payload`. `causation_id` poate lipsi pentru o intrare inițială; când există, indică cererea/operațiunea/evenimentul cauzal.

| Eveniment propus, schema v1 | Producător și payload minim | Utilizare |
|---|---|---|
| `crm.customer.created`, `crm.customer.updated` | CRM; referință și versiune, lista câmpurilor schimbate unde este necesar | Proiecții autorizate; fără copierea automată a întregului profil personal |
| `sales.quote.revision.created` | Oferte; ofertă, revizie și oportunitate | Istoric și sarcini; nu înseamnă trimitere |
| `sales.quote.acceptance.recorded` | Oferte; acceptare, revizie și referință la dovadă | Permite propunerea facturării, nu emiterea fără aprobare |
| `workflow.approval.decided` | S08; intenție, aprobare, decizie și versiune | Actualizarea Action Center; handlerul reverifică autorizarea la execuție |
| `workflow.operation.status.changed` | S08; operațiune, stare și referință la rezultat/eroare | Istoric, următoarea acțiune și monitorizarea rezultatelor necunoscute |
| `invoicing.invoice.issued` | Facturare nativă; factură, intenție și confirmare persistentă | Actualizare CRM și pași ulteriori autorizați; nu dovedește livrarea emailului sau acceptarea în SPV |
| `integration.invoice.issue.confirmed` | S09; sistem, mapare externă, intenție și momentul verificării | Proiecție externă și confirmarea operațiunii; nu redenumește documentul drept nativ |
| `notification.delivery.status.changed` | S10; comunicare, status, sursa raportării și moment | Distinge cererea acceptată, trimiterea, livrarea și alte confirmări efectiv disponibile |

Contractul de publicare leagă durabil modificarea și outbox-ul. Livrarea se proiectează pentru redelivery, nu pentru absența duplicatelor. Consumatorul deduplică prin `(tenant_id, consumer_id, event_id)`; marcarea procesării și efectul local sunt atomice sau folosesc un mecanism echivalent verificat. Orice efect extern ulterior trece prin propria intenție durabilă, nu printr-un simplu callback care trimite din nou la fiecare eveniment.

Nu există ordine globală. Versiunea agregatului permite detectarea informației vechi; o proiecție care necesită ordine folosește o secvență declarată a fluxului și/sau recitește sursa. O versiune de agregat nu este presupusă secvență fără goluri a tuturor evenimentelor consumate. Mesajele neprocesabile intră într-o stare de eroare observabilă, cu responsabil și reluare controlată în S08.

Canalul autentifică producătorul și limitează consumatorii per tenant și tip de date. Publicarea unui eveniment nu acordă dreptul de a citi fișierul referit sau de a executa o acțiune. Evenimentele de business, logurile tehnice, auditul și dovezile S14 au scopuri și retenții distincte. Event sourcing nu este o cerință implicită.

### 7.6 Evoluția contractelor

**Cerință confirmată S02-D06 / RC-008:** upgrade-urile nu schimbă tacit contractele sau operațiunile în curs. Propunerea de aplicare folosește versiuni explicite și teste de compatibilitate producător–consumator. Adăugarea unui câmp opțional este compatibilă numai dacă cititorii o tolerează și sensul rămâne neschimbat. Eliminarea sau redenumirea câmpurilor, schimbarea unităților, efectelor, regulilor de aprobare ori semnificației statusurilor cere versiune nouă sau migrare explicită. Valorile enum necunoscute nu se interpretează ca succes.

Manifestul modulului declară ID-ul, versiunea artefactului, capabilitățile și evenimentele oferite/consumate, compatibilitățile de runtime/configurare, migrările și permisiunile solicitate. Nu conține secrete și nu acordă singur drepturile solicitate. S05 implementează verificarea instalărilor; S12/S13 detaliază pachetul publicabil.

Procesele în curs păstrează versiunile contractelor, configurațiilor și handlerelor relevante. Un upgrade nu schimbă în tăcere payload-ul aprobat sau ținta unei operațiuni necunoscute. Versiunea veche se păstrează până la închiderea proceselor ori se aplică o migrare verificată, cu reaprobare dacă efectul se schimbă. Perioada de coexistență se stabilește din inventarul consumatorilor și politica de livrare, nu printr-un număr arbitrar de zile.

## 8. Securitate, politici și AI

S03 este proprietarul politicii de acces și al modelului de amenințări. [S03 v0.3](./03_IDENTITATE_ACCES_IZOLARE.md), capitolele 6–9, detaliază drepturile și deciziile confirmate RC-013–RC-020: operare, autoritate, niveluri de acces, desemnare inițială automatizată, transfer reciproc și recuperare prin factori proprii/asistență verificată. Aplicarea nivelului se verifică pentru fiecare drept; citirea tuturor documentelor nu extinde implicit dreptul de aprobare. Dovezile de identitate/reprezentare, repartizările și tranzițiile administrative necesită contracte și probe; aprobarea comercială nu autorizează schimbarea titularului. Schemele comerciale din capitolul 7 nu constituie contracte implementate de onboarding sau recuperare. Matricea de aplicare rămâne în revizuire; domeniul complet de identitate și izolare nu este închis. S02 fixează punctele contractuale unde trebuie aplicate controalele:

- context verificat de tenant, identitate și delegare pentru fiecare citire și acțiune, inclusiv joburi, căutare, proiecții, cache, fișiere și memorie AI;
- acces minim la obiecte și câmpuri; un identificator cunoscut ori o referință de document nu este o permisiune;
- verificarea separată a inițiatorului, aprobatorului, solicitantului execuției și executantului tehnic; RC-014 permite aceleiași persoane să pregătească și să aprobe cu drepturi explicite, fără a confunda pașii sau a pretinde control independent;
- mandate și bugete aplicate server-side, inclusiv după pauză/restart; indisponibilitatea AI nu blochează funcțiile deterministe autorizate;
- conținutul din emailuri, documente și răspunsuri de model este tratat ca date neîncrezute, nu ca instrucțiuni de schimbare a politicii sau confirmare de business;
- credențiale numai prin referințe server-side; fără secrete în manifest, evenimente, loguri sau prompturi;
- AI Constructor fără acces implicit la datele și secretele de producție; AI Operator fără SQL/shell arbitrar ori drept de publicare a codului;
- audit corelat pentru cerere, decizie, refuz, tentativă și rezultat; erorile nu divulgă datele altei firme și nu copiază necontrolat payload-uri personale;
- RLS, scheme separate și containere sunt opțiuni de control, nu dovezi suficiente de izolare. Se testează și rolurile de bază de date, workerii privilegiați și restaurarea.

Politicile de date, licențierea, cerințele fiscale și limitele dovezilor se verifică cu responsabilii S03/S07/S14. O aprobare funcțională sau un hash pe blockchain nu înlocuiește aceste verificări.

## 9. Alternative și decizii

**Sursă și limită a confirmării:** răspunsurile explicite ale inițiatorului din 2026-09-14: revizuirea capitolelor 6 și 9 a fost consemnată în S02 v0.2, iar alegerile pentru cumpărător, linii și ciorne, la detalierea capitolului 7, în S02 v0.3. D01 este o direcție arhitecturală; D02 o prioritate de evaluare, nu alegerea definitivă a bazei de date; D03–D07 sunt cerințe de proiectare; D08–D12 delimitează prima versiune de produs. Corespondențele transversale sunt păstrate în registrul planului general.

| ID local / general | Problemă și alternative | Direcție sau cerință confirmată și motiv | Probe și condiție de reevaluare | Statut la 2026-09-14 |
|---|---|---|---|---|
| S02-D01 / RC-003 | Nucleu de business modular versus microserviciu per modul | Nucleu modular pentru funcțiile noi; separări fizice justificate prin securitate sau probe; mai puține dependențe operaționale inițiale | Flux complet, granițe, deployment și încărcare; separare dacă limitele măsurate o cer; fără rescriere automată a serviciilor existente | Direcție confirmată de inițiator; implementare neverificată |
| S02-D02 / RC-004 | PostgreSQL versus MongoDB sau altă opțiune pentru datele noi | PostgreSQL este candidatul principal de evaluat pentru relații, tranzacții și constrângeri; core-ul MongoDB nu este migrat automat | Tranzacții, izolare, concurență, migrări, restaurare și cost; dacă nu trece probele, evaluăm alternativele; strategia de izolare și persistență per tenant se decide cu S03/S04 | Prioritate de evaluare confirmată; tehnologie încă nevalidată |
| S02-D03 / RC-005 | Reguli comune versus implementări divergente pentru UI/AI/API | Aceleași contracte și reguli de business, cu drepturile evaluate pentru fiecare actor | Paritate între canale, cost de integrare, licențe și mentenanță; OpenAPI, Node.js/TypeScript, bibliotecile și manifestul rămân candidați de verificat | Cerință confirmată; soluție tehnică de verificat |
| S02-D04 / RC-006 | Aprobare asupra unor date mutabile versus legătură stabilă cu efectul | Aprobarea privește conținutul și efectul concret; modificările relevante cer reaprobare | Payload schimbat, revocare, expirare și concurență; modelul de intenție, canonicalizarea și mecanismul amprentei se validează în S02/S03/S08 | Cerință confirmată; mecanism de verificat |
| S02-D05 / RC-007 | Date curente folosite retroactiv versus snapshot-uri și proveniență | Profilul curent nu rescrie documentele istorice; sursa documentului este păstrată | Modificare contact, sincronizare întârziată și schimbare furnizor; corecțiile și retenția se detaliază cu S03/S07/S09 | Cerință confirmată; model detaliat de verificat |
| S02-D06 / RC-008 | Upgrade tacit versus compatibilitate și migrare explicită | Upgrade-urile nu schimbă tacit contractele sau operațiunile în curs | Consumator vechi/producător nou, aprobare în așteptare, export/restaurare; strategia și fereastra de coexistență se stabilesc pe implementările reale | Cerință confirmată; strategie de verificat |
| S02-D07 / RC-009 | Retry generic versus identitate stabilă și reconciliere | Repetarea aceleiași facturări nu autorizează efecte suplimentare; rezultatul necunoscut cere reconciliere, fără fallback automat | Dublu clic, canale diferite, restart și timeout pentru ambele origini; `BillingRequest` și cheile sunt propuneri, nu garanție de deduplicare semantică universală | Cerință confirmată; mecanism de verificat |
| S02-D08 / RC-001 | Un tenant per entitate juridică versus grup juridic într-un tenant | Un tenant reprezintă o singură entitate juridică; utilizatorul poate avea apartenențe separate în mai multe firme | Selectarea firmei, autorizare și izolare; extinderea la grupuri juridice necesită cerere de produs și revizuire S01/S03/S07 | Model confirmat pentru prima versiune |
| S02-D09 / RC-002 | Facturare numai din ofertă versus și emitere directă | Prima versiune include ambele trasee; fără oportunități sau acceptări fictive pentru factura directă | Aceleași validări, aprobări și protecții în ambele trasee; UX de proiectat; avansurile și facturarea parțială rămân de delimitat în S07 | Scope confirmat pentru prima versiune |
| S02-D10 / RC-010 | Client reutilizabil obligatoriu versus și cumpărător punctual | Facturarea directă acceptă client existent sau cumpărător punctual; CRM-ul se creează/actualizează numai explicit | Salvare/pregătire/emitere fără scriere implicită în CRM; referințe autorizate și snapshot separat; drepturile și câmpurile fiscale se validează în S03/S07 | Cerință de produs confirmată |
| S02-D11 / RC-011 | Numai catalog versus catalog plus linii punctuale | Omul autorizat poate folosi catalogul sau linii punctuale; AI-ul rămâne limitat la surse și prețuri autorizate | Teste de proveniență, drepturi și preț fals declarat ca fiind din catalog; limitele negocierii și schema catalogului se stabilesc în S03/S07 | Cerință de produs confirmată |
| S02-D12 / RC-012 | Salvare numai completă versus ciorne incomplete | Ciornele incomplete se salvează; pregătirea pentru aprobare cere date complete și valide | Lipsuri vizibile fără totaluri inventate, refuz pentru schema invalidă și pentru pregătirea incompletă; rezervarea și concurența se verifică în S07/S08 | Cerință de produs confirmată |

Confirmările nu reprezintă aprobarea integrală a capitolelor sau a tuturor detaliilor tehnice adăugate pentru aplicarea lor. Structura catalogului, `BillingRequest`, schemele, codurile de eroare și mecanismele de rezervare rămân propuneri. Schimbarea unei direcții confirmate necesită motiv, impact asupra secțiunilor dependente și o nouă decizie explicită; o probă eșuată nu este ascunsă prin păstrarea etichetei „confirmat”.

## 10. Plan de implementare pentru livrabil

În această etapă aprobarea privește arhitectura și contractele necesare, nu implementarea întregii platforme. Pașii tehnici de mai jos necesită o etapă explicită ulterioară.

| Pas | Rezultat observabil | Dependențe | Verificare | Responsabil |
|---|---|---|---|---|
| P1 — Inventar și revizuire M0 | Cod/medii identificate, proprietari de date revizuiți, alternative și riscuri asumate | Acces la implementări, S01/S03/S04 | Inventar pe revizii concrete; fiecare reutilizare are probe sau rămâne condiționată | Tehnic și produs, de desemnat |
| P2 — Contractele fluxului și emiterii directe | Scenarii și date fictive pentru ofertă, facturare directă, aprobări și excepții | S01/S06/S07/S08 | Fiecare pas are autoritate, input, rezultat și următoarea acțiune | Domeniu și verificare, de desemnat |
| P3 — Scheme și teste de contract | Scheme executabile, manifest minim, erori, validatori și teste inițial eșuate pentru comportamentele încă neimplementate | Deciziile relevante S02 și politica S03 | Exemple valide/invalide, același contract pentru UI și adaptorul AI | Tehnic, de desemnat |
| P4 — Primul segment M1 | Contact → oportunitate → ofertă, executabil manual; intenții, aprobări și audit minime | Baza S03–S08/S10 și mediul S04 | Flux real cu persistență, izolare și restart; nu doar ecrane demonstrative | Echipa livrabilului, de desemnat |
| P5 — Emitere și servicii M1/M2 | Facturare nativă validată din ofertă și directă; conector extern numai dacă este inclus explicit | S07/S08/S10; S09 opțional | Confirmare, dubluri, timeout și reconciliere; fără dependență FGO pentru nativ | Domeniu și operare, de desemnat |
| P6 — Evoluție și acceptare | Compatibilitate, proces în curs, export și restaurare demonstrate | S04/S05/S12/S16 pentru funcțiile livrate | Probe reproductibile; riscurile reziduale și limitele rămân vizibile | Verificare și operare, de desemnat |

Extensii ulterioare: alte procese comerciale și industrii, publicare extinsă de API-uri, automatizare pe mandate preaprobate, personalizări și catalog. Contractele lor se adaugă când există un livrabil justificat; nu blocăm fluxul inițial pe un SDK universal sau pe un marketplace.

## 11. Teste și criterii de acceptare

Criteriile de mai jos sunt propuse; nu există rezultate tehnice obținute în această redactare. Pentru acceptarea documentului se cere revizuirea proprietarilor, exemplelor și deciziilor relevante. Pentru acceptarea implementării se cer probele efective, pe versiuni și medii identificate.

| ID și scenariu | Rezultat așteptat | Metodă și mediu | Dovadă necesară |
|---|---|---|---|
| T01 — Acoperirea fluxului | Fiecare pas S01/S06 are capabilitate, sursă de date, politică și excepții | Revizuire documentară comună S02/S03/S06/S07/S08 | Matrice revizuită și decizii asumate; neimplementările sunt marcate |
| T02 — Paritate UI/AI/API | Aceleași validări de business și refuzuri pentru același context autorizat | Teste de contract și integrare cu adaptoare | Rezultate reproductibile pe același set de cazuri |
| T03 — Două firme | Citiri, scrieri, referințe, fișiere, evenimente, cache și operațiuni fără acces încrucișat | Teste negative în mediu izolat | Nicio divulgare sau modificare neautorizată, inclusiv la restaurare |
| T04 — Aprobare și concurență | Datele schimbate, aprobarea expirată sau drepturile revocate blochează efectul încă neexecutat; două execuții nu dublează efectul | Teste cu revizii și intercalări controlate | Auditul deciziei și lipsa efectului neautorizat |
| T05 — Deduplicare | Aceeași cerere întoarce operațiunea existentă; alt payload cu aceeași cheie este conflict; altă cheie nu dublează aceeași facturare | Teste prin UI/API/worker, inclusiv după expirarea cache-ului | O evidență coerentă a efectului și rezultate autorizate |
| T06 — Restart după dispatch | Efectul posibil executat nu este retrimis doar după expirarea lease-ului | Oprire controlată între dispatch, răspuns și persistarea rezultatului | Rezultat recuperat sau `unknown` și reconciliere fără duplicare |
| T07 — Evenimente | Redelivery, întârzieri și ordine diferită nu dublează efecte și nu regresează proiecțiile | Teste outbox/consumatori cu erori injectate | Persistență atomică locală, deduplicare și stare de eroare observabilă |
| T08 — Independență nativă | Facturarea Bizix validată funcționează fără cont, credențiale sau apeluri FGO | Mediu nativ cu integrarea externă neconfigurată | Flux și confirmare native; criteriile fiscale S07 verificate separat |
| T09 — Extern opțional și timeout | Proveniență externă; rezultat necunoscut fără reemitere sau fallback; funcțiile nesuportate sunt refuzate | Adaptor simulat, apoi sandbox autorizat dacă se oferă conectorul | Urme ale încercării și reconcilierii; simularea singură nu validează API-ul real |
| T10 — Snapshot și tranziție | Contactul modificat și schimbarea sistemului nu rescriu documentele sau intențiile în curs | Teste de domeniu și integrare | Versiuni, referințe și proveniență păstrate |
| T11 — Upgrade | Consumatorii și procesele în curs rămân compatibile ori sunt migrate explicit | Producător/consumator din versiuni diferite, aprobare în așteptare | Matrice de compatibilitate și probă de migrare fără schimbarea tacită a efectului |
| T12 — Export și restaurare | Datele, relațiile, fișierele, configurațiile și operațiunile sunt recuperabile fără amestecarea firmelor | Mediu separat, servicii externe inițial dezactivate | Export verificat, restaurare și reconciliere înainte de reactivarea workerilor |
| T13 — Actualitate și comunicări | Cerere acceptată, emis, trimis, livrat și status fiscal sunt distincte; proiecțiile întârziate sunt vizibile | Teste de integrare și acceptare UX | Statusuri bazate pe confirmări reale, nu deducții din alte etape |
| T14 — Operabilitate și cost | Praguri stabilite înainte de acceptare; consum și întârzieri măsurabile pe flux | Probe S04/S15/S16 pe profilul de încărcare convenit | Măsurători, alerte, responsabil și limite documentate |
| T15 — Facturare directă | Emiterea fără oportunitate/ofertă este posibilă cu date valide și aprobare; ciorna incompletă se salvează, dar pregătirea ei pentru aprobare este refuzată; fără acceptare fictivă | Teste de domeniu, contract și flux nativ; aceeași matrice de aprobare, concurență, restart și rezultat necunoscut ca pentru traseul din ofertă | Identitate de facturare păstrată între încercări și intenții succesoare; niciun efect suplimentar prin regenerarea cheii de transport |
| T16 — Un utilizator în două firme | Apartenențele permit selectarea firmei, nu amestecarea datelor; un tenant are un singur emitent juridic | Teste S03/S05/S07 cu contexte și configurații distincte | Facturi, conexiuni și aprobări legate de firma corectă; referințele încrucișate sunt refuzate |
| T17 — Mai multe revizii sau acceptări | Repetarea consemnării sau revizuirea aceleiași oferte nu autorizează automat o a doua facturare integrală a aceluiași angajament | Teste S07/S08 cu două cereri concurente, versiuni și intenții succesoare | Identitatea comercială și maparea la facturare sunt validate înaintea emiterii; ambiguitatea produce conflict, nu factură nouă |
| T18 — Cumpărător punctual | Salvarea, pregătirea și emiterea nu creează/modifică implicit profilul CRM; salvarea ca client este separată și autorizată | Teste S07/CRM cu cumpărător punctual, existent și referință inaccesibilă | Snapshot și proveniență corecte; zero scrieri CRM necerute, fără divulgare între tenants |
| T19 — Ciornă incompletă și reguli indisponibile | Lipsurile sunt salvate și afișate; `prepare` le refuză; schema invalidă și scrierile neautorizate nu sunt acceptate ca lipsuri; reguli indisponibile nu produc `valid` sau totaluri inventate | Teste de contract/domeniu cu lipsuri, taxe neprecizate și validator indisponibil | ID persistent fără factură/aprobare; `REQUEST_INCOMPLETE` sau `DOMAIN_RULES_UNAVAILABLE`, după caz |
| T20 — Linii și proveniența prețului | Omul autorizat folosește catalog sau linie punctuală; AI-ul nu falsifică prețul prin `source = manual`, ID-uri sau override-uri de catalog | Aceleași cazuri prin UI, API și adaptor AI, cu drepturi distincte | Prețul și autorul provin din surse verificate; linia punctuală nu creează implicit un articol în catalog |
| T21 — Editare versus rezervare | Update-ul și rezervarea concurente nu permit execuția unei versiuni vechi; rezervarea activă/necunoscută blochează schimbarea datelor | Intercalări controlate S07/S08, inclusiv aprobare concomitentă și oprire între rezervare și operațiune | Un singur rezultat admisibil, refuz explicit al concurentului și asociere recuperabilă înainte de dispatch |
| T22 — Schimbarea sursei și a precondițiilor | Schimbarea surselor curente selectate cere rezolvare explicită; catalogul nou nu rescrie oferta acceptată; după aprobare nu se substituie datele, iar precondițiile neîndeplinite blochează emiterea | Teste cu versiuni de sursă diferite, ofertă acceptată, dată devenită nepermisă și snapshot modificat în cererea de aprobare | Refuz explicit unde este cazul, fără alt payload executat și fără rescrierea condițiilor acceptate sau a documentelor istorice |
| T23 — Reaprobare după închiderea intenției | Repetarea deciziei nu redeschide intenția; succesorul cere predecesor eligibil și motiv, fără editări fictive sau schimbarea identității efectului | Teste cu refuz, expirare, revocare, două cereri de succesor concurente și efect `unknown` | Un singur succesor admis când este sigur; tranziție refuzată pentru efect rezervat, necunoscut sau emis; istoricul rămâne intact |

Comenzile de test/build se preiau din proiectele de implementare când acestea sunt inspectate. Nu există aici o comandă presupusă care să demonstreze funcționarea backend-ului. Cazurile de acceptare necesită revizuire independentă de generatorul implementării.

## 12. Livrare și operare

S04 stabilește mediile, artefactele și procedurile; S08/S09 gestionează recuperarea proceselor și reconcilierea. Contractul comun trebuie să permită:

- corelarea unei probleme de la UI la intenție, aprobare, încercare, handler și rezultat, fără logarea secretelor;
- monitorizarea latenței capabilităților, vechimii aprobărilor, operațiunilor `unknown`, restanțelor outbox, proiecțiilor întârziate și conflictelor de idempotență;
- limite de concurență și consum per tenant/conexiune, fără a publica aici SLA-uri sau bugete numerice neverificate;
- oprirea controlată a execuțiilor cu efecte externe la incidente, păstrând citirea și preluarea manuală unde sunt sigure;
- deployment de cod și migrări cu compatibilități declarate, verificări post-livrare și plan de revenire potrivit stării datelor;
- backup și restaurare pentru date, fișiere, configurări și evidența efectelor, cu obiective validate în S04/S16.

Restaurarea nu retrage facturi sau mesaje deja produse în afara bazei restaurate. Într-un mediu restaurat, workerii cu efecte externe rămân opriți până la verificarea mediului, credențialelor, identităților efectelor și reconcilierii. Rollback-ul codului nu garantează rollback-ul datelor sau al obligațiilor de business.

## 13. Personalizare, reutilizare și drepturi

Pentru pilot, preferăm configurări validate: câmpuri suplimentare delimitate, șabloane de document și parametri ai regulilor aprobate. Câmpurile suplimentare au namespace, tip, versiune și reguli de acces/export; nu pot suprascrie tenantul, sumele validate, aprobarea sau sursa documentului printr-un obiect JSON arbitrar.

Extensiile de cod folosesc puncte publicate de extensie și contracte versionate. Nu importă modelele de persistență private și nu rulează implicit în procesul privilegiat comun. Schimbările de politică, calcul sau efect sunt revizuite în secțiunea responsabilă; nu se ascund în prompturi ori configurări netestate.

S12 deține ciclul de publicare și compatibilitatea; S13 verifică generalizarea, distribuirea, drepturile și mentenanța. Datele, configurările private, documentele și know-how-ul unei firme nu intră automat într-un pachet reutilizabil. Testul ulterior cu două firme verifică upgrade-ul bazei comune și exportul fără divulgarea datelor private, nu doar instalarea aceleiași interfețe.

## 14. Migrare și continuitate

Absența clienților externi în producție, confirmată în S01, reduce unele obligații de coexistență, dar nu autorizează eliminarea datelor sau infrastructurii interne. Inventarul stabilește ce trebuie păstrat și ce poate fi înlocuit.

Pentru fiecare tranziție propusă se cer:

1. inventar pe sursă, tenant, tip de date, versiune și responsabil; fără presupunerea compatibilității schemelor vechi;
2. mapări de identificatori, câmpuri, monede, statusuri, fișiere și proveniență, cu excepțiile neconvertibile păstrate vizibil;
3. export și restaurare de probă într-un mediu separat, plus reconciliere a numărului de obiecte, totalurilor relevante și relațiilor;
4. autoritate unică de scriere în fiecare etapă; coexistență prin API/adaptor, nu sincronizare bidirecțională necontrolată;
5. tratarea intențiilor și efectelor în curs înainte de cutover; fără redirect automat al operațiunilor vechi către noul furnizor;
6. aprobare explicită a cutover-ului și, separat, a retragerii sau ștergerii componentelor/datelor vechi, cu limitele recuperării cunoscute.

Facturile externe istorice pot fi păstrate ca referințe sau importate ca proiecții dacă API-ul și drepturile permit. Nu se reemit și nu se redenumesc ca facturi native Bizix. Dacă datele efectului nu pot fi reconciliate, tranziția afectată rămâne blocată, iar cazul este preluat de un responsabil.

## 15. Riscuri și întrebări deschise

| Risc sau necunoscută | Impact | Responsabil | Probă ori răspuns necesar | Blochează ce livrabil? |
|---|---|---|---|---|
| Codul și deployment-urile existente nu sunt inventariate | Reutilizare nepotrivită sau pierderea configurațiilor interne | Tehnic/S04, de desemnat | Repository-uri, revizii, medii, teste și obligații reale | Decizia de reutilizare și orice schimbare a sistemelor existente |
| Nu există pilot comercial | Modelul poate rata modul real de ofertare și facturare | Produs/S01, de desemnat | Interlocutor reprezentativ, documente și excepții anonimizate | Validarea produsului; nu blochează contractele de explorare |
| Minimul fiscal și comercial al facturării nu este stabilit | Contract incomplet sau emitere incorectă | S07 și responsabil fiscal, de desemnat | Jurisdicție, emitent, serii, calcul, corecții și obligații aplicabile | Emiterea reală; detaliile modelului fiscal |
| Aplicarea modelului confirmat un tenant = o entitate juridică | Emitent greșit sau acces încrucișat la schimbarea firmei active | S03/S05/S07, de desemnat | RC-001 stabilește limita; rămân de verificat apartenențele, configurarea emitentului și izolarea | Acceptarea implementării identității și facturării |
| Modelul concret al facturării directe și catalogului minim | Date incomplete, dependență artificială de ofertă ori autorități concurente asupra prețurilor | S02/S06/S07, de desemnat | RC-002 și RC-010–RC-012 confirmă scope-ul și comportamentele; câmpurile fiscale, limitele, autorizarea, schema executabilă și UX-ul trebuie validate pe contractele propuse în capitolul 7 | Contractele executabile pentru facturare directă |
| Reprezentarea canonică a aprobării este încă de fixat | Un efect diferit ar putea părea identic sau invers | S02/S03/S08, de desemnat | Scheme complete, canonicalizare, hash și exemple inter-limbaj | Aprobări executabile |
| Unicitatea efectului pentru facturare directă, revizii de ofertă sau intenții succesoare; ulterior tranșe/corecții | Dublare sau blocare a unei facturări legitime | S07/S08, de desemnat | Identitate stabilă, mapări unice, rezervare și închidere a efectului; limitele detectării cererilor distincte semantic duplicate | Ambele trasee de emitere înainte de implementare; tranșele/corecțiile numai dacă sunt incluse |
| Izolarea și recuperarea per tenant nu sunt demonstrate | Divulgare, corupere sau restaurare incompletă | S03/S04/S16, de desemnat | Probe negative și restaurare cu stocarea aleasă | Acceptarea runtime-ului comun |
| Capabilitățile reale ale furnizorului sunt neconfirmate | Reconciliere imposibilă sau promisiuni false de idempotență | S09, de desemnat | Contract API și teste în mediu autorizat; procedură manuală pentru incertitudine | Numai integrarea externă oferită, nu facturarea nativă |
| Retenția și drepturile de export nu sunt definite | Păstrare excesivă ori ștergere nepermisă | S03/S07/S13/S16, de desemnat | Politici aplicabile fiecărei categorii de date și licențe | Operarea cu date reale și distribuirea soluției |
| Proiecțiile sau evenimentele devin autoritate paralelă | Statusuri greșite și efecte declanșate din date vechi | S02/S05/S08, de desemnat | Contracte de actualitate, recitirea sursei și teste de redelivery | Acceptarea fluxului integrat |
| Responsabilii, profilul de încărcare și costurile lipsesc | Arhitectură fără posibilitate de operare asumată | S01/S04/S15/S16, de desemnat | Responsabilități, bugete și probe pe volume explicite | Oferta operabilă și lansarea; nu redactarea inițială |

Direcțiile, cerințele și limitele confirmate sunt în capitolul 9. Capitolul 7 propune datele și contractele pentru salvare, pregătire, aprobare și execuție. Urmează revizuirea schemelor și limitelor cu S03/S07/S08, definirea câmpurilor fiscale și a politicilor aplicabile, apoi probele S03/S04/S07/S08; exemplele JSON nu înlocuiesc aceste verificări. PostgreSQL este evaluat prioritar, nu considerat validat. Alegerea motorului de workflow, forma izolării și mecanismele de implementare rămân condiționate de probe; avansurile și facturarea parțială se delimitează în S07.

## 16. Checklist de aprobare

Lista privește aprobarea integrală a planului pentru un livrabil delimitat și rămâne nebifată. Confirmările parțiale ale inițiatorului sunt consemnate în capitolul 9; ele nu confirmă suficiența schemelor, existența probelor sau aprobarea întregului document.

- [ ] Obiectivul, actorii și fluxul delimitat sunt confirmate pentru livrabilul curent.
- [ ] Ipotezele comerciale și informațiile istorice sunt separate de starea tehnică verificată.
- [ ] Granițele modulelor și proprietarii datelor sunt acceptați de secțiunile dependente.
- [ ] Modelul minim, identificatorii, snapshot-urile și sursele autoritative sunt suficiente pentru flux.
- [ ] Capabilitățile, erorile, evenimentele și regulile de evoluție au un contract explicit și un responsabil.
- [ ] Politica S03 și legătura aprobării cu efectul concret sunt definite pentru implementarea vizată.
- [ ] Concurența, duplicatele, restartul și rezultatele necunoscute au probe și proceduri de recuperare.
- [ ] Facturarea nativă rămâne independentă de FGO; limitele fiscale și ale integrărilor sunt explicite.
- [ ] Există criterii de acceptare, operare, cost, export și restaurare pentru livrabilul propus.
- [ ] Riscurile care blochează implementarea au răspuns sau condiție de rezolvare și responsabil.
- [ ] Drepturile de reutilizare, licențele și datele interne existente sunt tratate unde se aplică.
- [ ] Planul general și secțiunile afectate reflectă deciziile transversale aprobate.
- [ ] Aprobarea identifică persoana, versiunea S02 și livrabilul delimitat.

Aprobarea acestui plan nu autorizează deployment, emitere de facturi, transmitere de mesaje, migrare sau retragerea infrastructurii. Acestea necesită probe și autorizări separate.
