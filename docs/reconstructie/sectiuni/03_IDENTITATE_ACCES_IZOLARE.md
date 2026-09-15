# S03 — Identitate, acces și izolare: drepturile minime ale fluxului comercial

> **Versiune:** 0.3 — desemnare inițială automatizată, transfer reciproc și recuperarea accesului
> **Data:** 2026-09-15
> **Stare:** În revizuire; opt decizii de produs confirmate conform capitolului 9. Nu reprezintă aprobarea integrală a S03 sau autorizarea implementării.
> **Implementare:** Neverificată; au fost consultate documentele, nu codul, politicile active sau deployment-urile.
> **Legături:** [Plan general](../README.md) · [Mini-brief S01](./01_PRODUS_ECONOMIE_PILOT.md) · [Contracte S02](./02_ARHITECTURA_CONTRACTE_DATE.md) · [Hartă UX S06](./06_SPATIU_DE_LUCRU_UX.md) · [Template](../TEMPLATE_PLAN_SECTIUNE.md)

Acest livrabil stabilește cine poate introduce prețuri manuale, pregăti o intenție, aproba și solicita execuția în fluxul ofertă → facturare și în facturarea directă. Este prima parte a S03, nu proiectarea completă a identității, izolării, administrării sau protecției datelor. Matricea și mecanismele de aplicare sunt propuneri construite pe deciziile confirmate; nu sunt roluri sau API-uri deja implementate.

## Identificare și stare

| Câmp | Valoare |
|---|---|
| ID și titlu | S03 — Identitate, acces și izolare |
| Versiune și data revizuirii | 0.3 / 2026-09-15 |
| Stare document | În revizuire pentru drepturile minime ale primului flux |
| Stare implementare | Neverificată în proiectele de implementare |
| Livrabil și etapă generală | M0: politica minimă pentru operarea manuală M1/M2 și limitele asistentului comercial M3 |
| Responsabil de produs | De desemnat; alegerile inițiatorului sunt consemnate separat |
| Responsabil tehnic | De desemnat |
| Responsabil de securitate, verificare și operare | De desemnat împreună cu S04/S16 |
| Intrări de referință | S01 v0.5, contractele comerciale S02 v0.3 preluate în v0.6, S06 v0.4 și planul general v0.10 |
| Secțiuni dependente | S02/S05/S06/S07/S08/S09/S11; S04/S16 pentru aplicare și probe |

**Confirmări ale inițiatorului la 2026-09-15:** în v0.1 au fost consemnate prețurile manuale numai pentru aprobator, cumulul explicit pregătire/aprobare și solicitarea execuției de către un om desemnat. În v0.2 au fost confirmate responsabilul firmei ca autoritate de acordare/retragere a rolurilor sensibile, inclusiv atribuirea explicită a propriilor roluri operaționale, fără administratori delegați în prima versiune, și cele două niveluri de acces: documente atribuite implicit sau întregul domeniu acordat explicit. În v0.3 au fost confirmate verificarea automatizată pentru prima desemnare, transferul voluntar cu confirmare reciprocă și recuperarea prin factori proprii plus asistență verificată. Confirmările RC-013–RC-020 nu aleg furnizorul sau dovezile acceptate pentru verificarea automată, mecanismul de autorizare, praguri valorice sau persoane concrete și nu aprobă toate detaliile de mai jos.

## 1. Obiectiv și valoare

Firma trebuie să poată lucra cu puțini oameni fără a transforma orice utilizator, administrator sau angajat AI într-un actor cu drepturi nelimitate. Pregătirea muncii, asumarea angajamentului și pornirea efectului sunt responsabilități distincte, chiar când aceeași persoană le cumulează.

Rezultatul urmărit: fiecare pas are un actor eligibil, un domeniu și resurse autorizate, condiții de refuz și o urmă de audit. Succesul se verifică prin scenariile din capitolul 11, nu prin existența unor etichete de rol sau a unor butoane ascunse. Fluxul manual rămâne independent de disponibilitatea AI.

## 2. Scope și granițe

| Inclus în livrabilul curent | Exclus sau de detaliat separat | Responsabilitate |
|---|---|---|
| Drepturi minime pentru ciorne, prețuri manuale, intenții, aprobări și solicitarea execuției | Toate rolurile și procesele firmei | Extinderi S03, pe baza S01/S07 |
| Separarea oamenilor, AI-ului și identității tehnice de execuție | Implementarea autentificării, MFA, recuperării contului și administrării complete | S03/S04/S05 |
| Tenant, domeniu, mandat și două niveluri de acces: documente atribuite / întregul domeniu | Alegerea definitivă a motorului de politici și a izolării fizice | S03/S04, prin probe |
| Responsabilul firmei acordă/retrage rolurile sensibile și stabilește întinderea accesului | Delegarea administrării în prima versiune | S03/S05 |
| Politica pentru prima desemnare automatizată, transfer reciproc și recuperare prin factori proprii/asistență | Alegerea și validarea mecanismului de verificare, dovezile acceptate, contractele administrative și procedurile operaționale complete | S03/S04/S05/S08/S16; S09 dacă se folosește un serviciu extern |
| Legătura drepturilor cu contractele și snapshot-ul S02 | Schemele executabile, calculul fiscal, catalogul și regulile de negociere | S02/S07 |
| Reverificare, revocare și cerințe minime de audit | Motorul durabil, retry, deduplicare și reconciliere | S05/S08/S09 |
| Cerințe pentru prezentarea drepturilor și blocajelor | Modificarea diagramelor, wireframe-uri sau prototip | S06; harta existentă nu este actualizată prin acest document |
| Riscuri de acces ale fluxului și probe negative | Modelul complet de amenințări și politicile de retenție/export/ștergere | Restul S03, împreună cu S04/S07/S16 |

Facturarea directă și cea din ofertă folosesc aceeași politică pentru emitere. Bizix este sistemul implicit; activarea unui conector extern nu acordă drepturi suplimentare și nu ocolește aprobarea. S03 decide accesul, nu corectitudinea fiscală sau existența acceptării comerciale.

## 3. Starea actuală și reutilizarea

| Element | Sursă verificată la 2026-09-15 | Constatare și limită |
|---|---|---|
| Flux, oameni și asistent comercial | S01 v0.5 | Document de lucru; fără pilot identificat și fără clienți externi în producție declarați |
| Ciorne, intenții, aprobări și operațiuni | S02, capitolele 6–8 | Contracte propuse; separarea pașilor și autoritatea backend-ului sunt intrări pentru S03 |
| Roluri în experiența utilizatorului | S06 v0.4 | Persoana care aprobă poate fi și responsabilul comercial; harta nu acordă drepturi |
| Keycloak/OIDC și autorizare backend | [Analiza de direcție](../../ANALIZA_DIRECTIE_AI_APPS.md), secțiunile despre identitate și securitate | Referințe documentare, nu verificare a configurației sau a codului; SSO nu înlocuiește autorizarea de business |

Nu preluăm automat rolurile istorice de administrator în noua matrice. Reutilizarea identității existente necesită inventar, mapare de drepturi și probe; redactarea politicii nu autorizează schimbarea conturilor sau a permisiunilor active.

## 4. Dependențe și contracte cu alte secțiuni

| Secțiune | Intrare sau ieșire necesară | Condiție pentru livrabilul dependent |
|---|---|---|
| S02 | Actor/tenant/mandat verificați, contracte și snapshot-uri versionate | Drepturile se aplică fără schimbarea tacită a sensului contractelor |
| S05 | Apartenențe, atribuiri de drepturi, runtime și audit | Instalarea unui modul sau publicarea unei capabilități nu acordă acces |
| S06 | Acțiuni distincte, firma activă, rolul și motivul blocării | UI-ul consumă eligibilitatea verificată; lipsa accesului nu divulgă date ascunse |
| S07 | Proveniența prețurilor, date complete, reguli comerciale/fiscale și efectul autorizat | Backend-ul verifică valorile și limitele; dreptul de aprobare nu poate suprascrie validările |
| S08 | Intenție, decizie umană, solicitant și executant tehnic distincte | Persistență și reverificare înainte de efect, inclusiv după restart |
| S09 | Conexiunea, mediul și ținta fixate în intenție | Dreptul de utilizare a unei conexiuni nu permite citirea secretelor sau administrarea ei |
| S11 | Identitate AI, mandat și responsabil uman | AI-ul poate pregăti numai în mandat; nu primește aprobarea sau solicitarea execuției în prima versiune |
| S04/S16 | Autentificare, aplicarea izolării, verificări și operare | Probe cu cel puțin două firme, drepturi revocate și canale diferite înainte de acceptare |

## 5. Scenarii de utilizare

| Scenariu | Parcurs și rezultat urmărit |
|---|---|
| SC-01 — Operator fără drept de aprobare | Salvează ciorna din surse autorizate și pregătește intenția dacă datele sunt complete. Nu introduce preț manual și nu aprobă. Poate solicita execuția numai dacă primește desemnarea separată și există o aprobare validă |
| SC-02 — Preț punctual necesar | Operatorul cere intervenția aprobatorului. Aprobatorul cu drept de editare a ciornei introduce prețul, cu proveniență; un operator autorizat pregătește noua versiune, apoi urmează aprobarea explicită |
| SC-03 — Antreprenor cu roluri cumulate | Are explicit operator + aprobator + solicitant al execuției în firma și domeniul relevante. Poate parcurge toți pașii, dar editarea sau pregătirea nu țin locul aprobării |
| SC-04 — Aprobare și operare separate | Aprobatorul verifică snapshot-ul și aprobă. Alt om desemnat solicită execuția aceleiași intenții, fără să modifice payload-ul și fără să aibă obligatoriu drept de aprobare |
| SC-05 — Asistent comercial | Pregătește ciorne și intenții în mandat, inclusiv pe baza unor prețuri deja introduse autorizat de om. Nu creează preț manual, nu aprobă și nu solicită execuția; omul preia pasul necesar |
| SC-06 — Date schimbate sau acces revocat | Schimbarea materială cere intenție succesoare și reaprobare. Drepturile ori mandatul nevalide blochează efectul încă neexecutat; după posibilul dispatch se păstrează rezultatul necunoscut și se reconciliază |
| SC-07 — Firmă sau domeniu diferit | Aprobatorul de oferte din firma A nu devine aprobator de facturare în A și nici în firma B. Referințele și listele sunt filtrate prin contextul autorizat |

În lipsa unui aprobator ori solicitant eligibil, cazul rămâne în așteptare cu responsabil și cale de remediere. Nu se promovează automat operatorul, AI-ul sau administratorul pentru a închide fluxul.

## 6. Structură și date

### 6.1 Actori și atribuiri

Un tenant reprezintă o singură entitate juridică, conform RC-001. Aceeași identitate umană poate avea apartenențe și drepturi diferite în mai multe firme. Titlul profesional, emailul, CUI-ul, cunoașterea unui ID sau deținerea contului de infrastructură nu acordă drepturi asupra unui document.

Modelul minim propus păstrează:

- identitatea actorului și tipul său verificat: om, AI sau serviciu tehnic;
- apartenența activă la tenant, desemnarea responsabilului de acces și atribuirile de drepturi, cu domeniu, nivel de acces, emitentul atribuirii, versiune și stare;
- repartizările autorizate ale documentelor/cazurilor, separate de atribuirea rolului și de simpla asignare a unei sarcini; sursa, actorul și versiunea repartizării sunt verificabile;
- mandatul AI: identitate proprie, responsabil uman, acțiuni și resurse permise, limite și valabilitate; fără moștenirea întregului rol al responsabilului;
- politica aplicabilă, cu versiune și condiții; o schimbare de politică nu rescrie auditul deciziilor anterioare;
- proveniența prețului și a modificărilor, inițiatorul intenției, aprobatorul, solicitantul execuției și identitatea workerului, legate de obiectele S02.

Acestea sunt cerințe logice, nu tabele finale sau o cerință de microserviciu separat. Detaliile de retenție și administrare rămân de stabilit înainte de operarea cu date reale.

### 6.2 Regula de autorizare

Propunerea combină roluri cumulabile cu verificări per acțiune, tenant, domeniu și resursă. Lipsa unei acordări explicite înseamnă refuz. Acordarea unui drept nu elimină restricțiile de tip de actor, mandat, validare sau stare.

Accesul efectiv trebuie să satisfacă împreună identitatea verificată, apartenența activă, dreptul cerut pentru resursa respectivă, politica și mandatul când se aplică. Pentru execuție se adaugă aprobarea validă asupra efectului exact și precondițiile S02/S07/S08. Nu este suficient un rol generic `admin` sau accesul la endpoint-ul generic de workflow.

### 6.3 Autoritatea de acordare a drepturilor

**Decizie confirmată S03-D07 / RC-016:** în prima versiune, numai responsabilul firmei desemnat explicit poate acorda și retrage rolurile de aprobator și solicitant al execuției. Poate atribui explicit roluri operaționale și propriei identități, cu audit; nu le primește automat prin autoritatea administrativă. Delegarea administrării către alți administratori nu intră în prima versiune.

În acest document, denumirea **responsabil de acces al firmei** desemnează o identitate umană căreia i-a fost acordată această autoritate în produs, nu responsabilul de produs al reconstrucției și nu o calitate juridică dedusă din titlul profesional. Se verifică desemnarea pentru tenantul activ. Crearea unui cont, cunoașterea CUI-ului sau simpla declarare a unui nume de firmă nu dovedesc această autoritate. RC-018–RC-020 stabilesc direcțiile pentru prima desemnare, transfer și recuperare în capitolele 6.5–6.7; dovezile și mecanismele lor rămân de validat.

Propunerea de aplicare folosește aceeași autoritate pentru atribuirea rolurilor operaționale din matrice și, conform RC-017, pentru alegerea întinderii lor. Acordarea identifică destinatarul, firma, rolul/drepturile, domeniul, nivelul de acces și actorul care o dispune. Schimbarea nivelului ori retragerea sunt acțiuni explicite, versionate și auditate, nu simple modificări ale profilului sau promptului. Dreptul administrativ este distinct de drepturile pe care le poate acorda: responsabilul nu trebuie să fie deja aprobator ca să desemneze un aprobator.

Propriile atribuiri nu ocolesc restricțiile platformei: fără preț manual în afara drepturilor aplicabile, aprobare implicită, execuție înainte de aprobare sau acces la alt tenant. AI-ul, serviciile tehnice și suportul Bizix nu pot primi această autoritate printr-un rol comercial. Desemnarea altui responsabil de acces nu este o acordare obișnuită de rol operațional: urmează transferul sau înlocuirea verificată din capitolele 6.6–6.7, fără activarea implicită a unui administrator suplimentar.

### 6.4 Întinderea accesului la documente

**Decizie confirmată S03-D08 / RC-017:** pentru operator, aprobator și solicitant al execuției există două niveluri. Implicit se folosesc documentele atribuite autorizat; responsabilul poate acorda explicit întregul domeniu. Lipsa unui rol acordat nu oferă nici măcar accesul implicit la documente atribuite.

| Nivel | Întindere | Ce nu acordă |
|---|---|---|
| Documente atribuite — implicit la acordarea rolului | Numai documentele/cazurile repartizate autorizat persoanei, în tenantul și domeniul acordate | Nu permite consultarea documentelor nerepartizate, autoatribuirea unor documente existente sau o acțiune absentă din rol |
| Întregul domeniu — acordare explicită | Toate documentele domeniului acordat din acel tenant, pentru drepturile indicate | Nu acordă alt domeniu, alt tenant, toate tipurile de acțiuni sau acces global la CRM, fișiere ori configurații |

**Regula de aplicare propusă:** nivelul se evaluează pentru fiecare drept cerut, nu se combină liber nivelul unui rol cu acțiunile altuia. Un utilizator cu citire pentru întregul domeniu și aprobare numai pentru documente atribuite poate vedea toate documentele domeniului, dar aprobă numai cele atribuite. Un document repartizat fără rolul de aprobare nu devine aprobabil de acea persoană.

Exemplu fictiv: Maria este operator de facturare numai pentru cazurile atribuite; Andrei este aprobator pentru întregul domeniu facturare. Andrei poate verifica o intenție pregătită de Maria fără o repartizare individuală suplimentară, dar nu poate solicita execuția fără dreptul separat. Niciunul nu primește implicit drepturi asupra ofertelor sau altei firme.

Detalii de repartizare propuse, încă de validat în S02/S05/S07/S08:

- Modificarea repartizării care acordă acces este controlată de responsabilul de acces în varianta minimă propusă. Operatorul nu își poate atribui un document existent doar prin editarea unui câmp de responsabil; atribuirea unei sarcini în Action Center nu este singură o acordare de acces.
- Crearea unei ciorne cere un drept explicit de creare în domeniu, chiar în modul „documente atribuite”. Propunere: odată cu crearea autorizată, sistemul înregistrează durabil repartizarea către creator; acesta nu poate selecta arbitrar alte persoane sau resurse inaccesibile. Pentru AI rămân obligatorii mandatul și sursele autorizate, fără atribuirea automată a documentului tuturor persoanelor asociate agentului.
- Intenția, aprobarea, operațiunea și documentul rezultat se leagă de cazul sursă prin relații verificate server-side. Accesul se evaluează folosind dreptul specific și repartizarea/nivelul curente; legătura nu acordă orice acțiune și nici acces la întregul CRM sau la toate fișierele referite. Regulile exacte de propagare între ofertă, cerere de facturare și factură se fixează în contracte, fără a transfera implicit drepturi între domenii.
- Retragerea repartizării sau restrângerea de la întregul domeniu la documente atribuite se aplică și citirilor, listelor, căutării, fișierelor, rezultatelor memorate și pasului de execuție încă neefectuat. Istoricul aprobării rămâne, dar accesul ori autorizarea curentă pierdute nu sunt păstrate printr-o copie în job sau cache. Dacă există o altă acordare valabilă pentru aceeași acțiune și resursă, aceasta se evaluează explicit, nu se presupune.

Cele două niveluri nu reprezintă un editor universal de reguli pe echipe, sucursale sau câmpuri. Excepțiile de confidențialitate și drepturile de citire a surselor trebuie concretizate pentru pilot; mecanismul și granularitatea fizică nu sunt validate prin alegerea acestui model.

### 6.5 Prima desemnare prin verificare automatizată

**Decizie confirmată S03-D09 / RC-018:** prima desemnare a responsabilului, înainte de activarea spațiului cu date și operațiuni reale, se bazează pe un mecanism automat verificabil de identitate și reprezentare a firmei. Inițiatorul a ales această variantă în locul onboarding-ului asistat sau al unui flux demonstrativ separat. Nu sunt încă alese furnizorul, sursele de date, dovezile acceptate ori tratarea completă a excepțiilor.

Verificarea trebuie să distingă **cine este persoana** de **ce autoritate are să solicite administrarea firmei**. Autentificarea, MFA, verificarea emailului, controlul unui domeniu sau identificarea firmei după CUI nu dovedesc singure reprezentarea. Nici verificarea identității printr-un certificat sau serviciu de identificare nu dovedește automat un mandat pentru companie. Sursele și regulile aplicabile reprezentantului ori persoanei mandatate se evaluează înainte de alegerea tehnică; nu declarăm aici o metodă universal suficientă juridic.

Propunerea minimă de aplicare este:

1. Cererea de desemnare identifică persoana, contul, tenantul și entitatea juridică vizată. Înregistrarea cererii nu acordă autoritatea și nu deschide datele unei firme existente. „Am introdus primul CUI-ul” nu rezervă accesul asupra acelei firme.
2. Backend-ul verifică autenticitatea, actualitatea și rezultatul dovezilor din sursele acceptate și legătura lor cu persoana, firma și cererea concretă. Un document încărcat, un răspuns furnizat de client sau concluzia liberă a unui model AI nu sunt singure rezultatul verificării automate.
3. Persoana acceptă explicit rolul din contul verificat. Propunere de securitate pentru activare: MFA configurat și confirmare recentă de autentificare; mecanismul exact se validează în S03/S04. Prima desemnare și acceptarea se leagă durabil; rolurile comerciale se acordă ulterior explicit, conform RC-016.
4. Lipsa dovezilor, neconcordanțele, expirarea, indisponibilitatea verificatorului sau rezultatul neconcludent păstrează cererea neactivată, cu stare și remediere explicite. Nu se acordă autoritatea prin timeout, lipsa unui răspuns sau un scor presupus suficient. Asistența poate explica ori remedia dosarul, nu înlocuiește tacit verificarea automată cu o aprobare manuală de onboarding.
5. Cererile concurente, repetarea rezultatului verificării și reluarea după restart nu creează doi responsabili activi pentru același tenant. Pentru un tenant deja administrat se folosesc transferul sau recuperarea, nu un nou onboarding care înlocuiește titularul. Tratarea cererilor distincte care indică aceeași entitate juridică necesită politică de conflict și nedivulgare înainte de activare, fără unirea automată a tenanturilor.

Alegerea automatizării este o cerință pentru produsul inițial, nu dovada existenței unui furnizor care acoperă scenariul. Dacă verificarea identității și reprezentării nu poate fi demonstrată, activarea reală prin acest flux rămâne blocată. O excepție de onboarding manual necesită o decizie ulterioară explicită; recuperarea asistată confirmată în RC-020 nu devine o cale alternativă de primă desemnare.

### 6.6 Transferul voluntar al autorității

**Decizie confirmată S03-D10 / RC-019:** responsabilul actual inițiază transferul după reautentificare, iar destinatarul acceptă din propriul cont verificat, cu MFA. Autoritatea se mută numai la finalizarea sigură; o invitație neacceptată nu îl elimină pe titular. Acest flux presupune că responsabilul actual încă are acces; indisponibilitatea sa se tratează separat în RC-020.

Propuneri pentru contractul de transfer:

- Cererea fixează tenantul, identitățile actuală și destinatară, versiunea desemnării curente, autorul și momentul inițierii. Destinatarul vede firma și responsabilitatea acceptată; un link redirecționat nu permite altei identități să accepte.
- Până la acceptare și finalizare, destinatarul nu poate administra firma. Refuzul, expirarea ori anularea cererii păstrează autoritatea actuală; o schimbare concurentă de titular, o revocare relevantă sau autentificarea nevalidă blochează finalizarea vechii cereri.
- Finalizarea reverifică drepturile și identitățile și înlocuiește desemnarea într-o tranziție atomică/versionată: un singur responsabil activ, fără interval administrativ gol. Două acceptări, două transferuri concurente sau restartul nu produc doi titulari. Accesul administrativ al fostului titular încetează la această tranziție, inclusiv pentru cereri ulterioare cu sesiuni/tokenuri vechi.
- Rolurile comerciale, mandatele și aprobările nu se copiază noului responsabil și nu își schimbă autorul. Înainte de finalizare se prezintă separat ce roluri operaționale ale fostului titular sunt păstrate sau retrase printr-o acțiune autorizată; transferul administrativ nu este un offboarding implicit și nu rescrie aprobările istorice. Efectele încă neexecutate își reverifică lanțul de autorizare conform S02/S08.
- Ambele persoane sunt notificate prin canalele verificate, iar inițierea, acceptarea, anularea și rezultatul sunt auditate. Notificarea nu ține loc de acceptare, iar datele de autentificare nu se transferă între persoane.

Durata cererii, posibilitatea anulării în cursul acceptării, tratamentul schimbărilor de reprezentare și regulile de revizuire a rolurilor operaționale se fixează înainte de implementare. Un transfer finalizat nu se inversează prin ștergerea auditului; orice schimbare ulterioară are propriul flux autorizat.

### 6.7 Recuperarea contului și înlocuirea titularului indisponibil

**Decizie confirmată S03-D11 / RC-020:** pentru același titular se folosesc mai întâi factori alternativi sau coduri de recuperare pregătite anterior. În lipsa acestora există o cale asistată, cu dovezi, notificări și audit. Înlocuirea unui titular indisponibil este un caz distinct, nu o resetare de parolă și nu impersonarea contului său.

| Situație | Rezultat permis | Limită |
|---|---|---|
| Titularul are un factor alternativ sau cod de recuperare valid | Recuperarea aceleiași identități prin mecanismul de autentificare | Nu schimbă desemnarea firmei și nu reintroduce roluri ori apartenențe revocate |
| Titularul a pierdut factorii necesari | Caz asistat pentru verificarea identității și restabilirea sigură a accesului | Nu se acceptă doar un email, CUI-ul, date publice sau schimbarea adresei contului la cererea unui terț |
| Firma cere înlocuirea responsabilului care nu mai poate îndeplini rolul | Caz asistat distinct, cu verificarea autorității solicitantului asupra firmei și acceptarea noului titular din contul propriu verificat | Nu se cere simularea acceptării fostului titular; lipsa răspunsului său nu este singură dovadă suficientă pentru înlocuire |

Propuneri de protecție pentru recuperare:

1. Factorii și codurile sunt înrolați înainte de incident printr-un flux autentificat. Codurile sunt de unică folosință, protejate la stocare și invalidate la consum/regenerare; nu apar în audit, loguri sau conversații cu suportul. Două cereri concurente nu pot consuma cu succes același cod. Factorii acceptați, stocarea și limitele încercărilor se validează tehnic, fără parole temporare transmise liber.
2. Resetarea parolei nu ocolește singură MFA. După recuperare se revocă sesiunile și factorii compromiși ori înlocuiți, se reconfigurează protecția contului și se notifică titularul pe canalele verificate conform procedurii. Identitatea care aparține mai multor firme nu primește prin recuperare noi apartenențe sau drepturi.
3. Un caz asistat are scop, solicitant, probe, verificator autorizat, decizie motivată și rezultat auditat. Propunere: modificările privilegiate asistate cer și o verificare independentă de persoana care operează schimbarea. Aceasta este o măsură operațională de validat, nu un administrator delegat al firmei și nu un acces permanent al suportului la documentele comerciale.
4. Pentru înlocuirea titularului se verifică dovezi actuale de autoritate asupra firmei și noua identitate; probele de simplă identitate sau de posesie a unei căsuțe poștale nu substituie reprezentarea. Se notifică persoanele/canalele relevante conform procedurii. Contestarea, dovezile insuficiente sau verificările neconcludente mențin cazul nerezolvat, fără mutarea autorității pe baza tăcerii.
5. Înlocuirea verificată folosește aceleași protecții de unicitate, concurență și revocare a autorității vechi ca transferul. Nu recreează tenantul, nu copiază aprobările pe noul actor și nu reexecută operațiuni comerciale. Suspendările necesare pentru un incident și închiderea sesiunilor se tratează explicit, nu prin ștergerea utilizatorului sau istoricului.

Dovezile acceptate, canalele de notificare și contestare, ferestrele de așteptare, verificarea independentă și responsabilii BiziX trebuie definiți înainte de oferirea asistenței reale. Recuperarea asistată este o procedură privilegiată delimitată, nu un drept general al suportului de a desemna administratori.

Pentru cele trei fluxuri administrative se păstrează propus o cerere persistentă cu ID, tenant, tip, identități, versiunea autorității, starea verificării, referințe protejate la dovezi, acceptări/decizii și rezultat. Retenția, minimizarea și accesul la dovezi se definesc separat; datele de identificare nu sunt copiate integral în audit sau pe blockchain. Contractele administrative se detaliază în S02/S05/S08 și nu reutilizează automat aprobarea comercială drept autorizare a unei schimbări de titular.

## 7. Capabilități și matricea drepturilor minime

### 7.1 Cele patru drepturi nu sunt echivalente

| Acțiune | Cine este eligibil | Contract și limită |
|---|---|---|
| Introducere/modificare preț manual | Numai un om cu drept de aprobator în domeniul respectiv și drept de editare a ciornei | Prin scrierea autorizată a ciornei; fără modificare implicită a catalogului și fără aprobare implicită |
| Pregătire intenție | Operator uman autorizat sau AI cu mandat explicit de pregătire | `sales.quote.send.prepare` ori `invoicing.intent.prepare`; date complete, snapshot fixat, fără efect de trimitere/emitere |
| Aprobare/refuz | Numai aprobator uman pentru efectul respectiv | `workflow.approval.decide`; citirea snapshot-ului necesar, drept specific domeniului și stare admisibilă |
| Solicitare execuție | Numai om desemnat explicit pentru efectul respectiv | `workflow.intent.execute`; drept separat, aprobare validă și payload nemodificabil |

Salvarea ciornei este un drept de lucru distinct de pregătire; ciornele incomplete rămân permise conform RC-012. Execuția efectivă aparține workerului/handlerului autorizat, nu modelului AI și nu browserului. Acceptarea cererii în coadă nu confirmă emiterea sau trimiterea.

### 7.2 Roluri logice propuse

Rolurile sunt pachete de drepturi, nu un număr obligatoriu de angajați. Domeniile `oferte` și `facturare` se acordă explicit, cu nivelul de acces din capitolul 6.4; un „Da” din matrice este valabil numai în acel perimetru. Responsabilul de acces desemnează aprobatorii și solicitanții conform RC-016; nu se creează un rol de administrator delegat în prima versiune. Denumirile tehnice finale ale permisiunilor se fixează cu S02/S05.

| Rol / identitate | Ciornă și surse autorizate | Preț manual | Pregătire intenție | Aprobare/refuz | Solicitare execuție |
|---|---|---|---|---|---|
| Operator comercial / facturare | Citire și scriere în domeniul și resursele atribuite | Nu, prin acest rol | Da, în domeniul atribuit | Nu | Numai dacă primește și desemnarea separată |
| Aprobator de oferte | Citire pentru decizie; editarea ciornei cere dreptul de scriere | Da, numai pentru oferte și cu drept de editare | Numai dacă are și drept de pregătire | Numai trimiterea ofertelor autorizate | Numai dacă primește și desemnarea separată |
| Aprobator de facturare | Citire pentru decizie; editarea ciornei cere dreptul de scriere | Da, numai pentru cererile de facturare și cu drept de editare | Numai dacă are și drept de pregătire | Numai emiterea facturilor autorizate | Numai dacă primește și desemnarea separată |
| Solicitant al execuției | Citirea intenției aprobate și a statusului, în limitele necesare | Nu, prin acest rol | Nu, prin acest rol | Nu, prin acest rol | Da, numai pentru tipurile de efect și resursele atribuite |
| Responsabil de acces al firmei | Gestionează atribuiri, nu primește implicit citirea documentelor comerciale | Numai după atribuirea explicită a drepturilor necesare | Numai după atribuirea explicită a dreptului | Numai după atribuirea explicită a rolului | Numai după desemnarea explicită |
| Asistent comercial AI | Numai prin capabilitățile și sursele din mandat | Nu; poate reutiliza valori umane autorizate fără schimbare sau falsificarea provenienței | Da, dacă mandatul include pasul | Nu | Nu în prima versiune |
| Worker / handler tehnic | Numai datele necesare operațiunii autorizate | Nu decide prețuri | Nu inițiază discreționar operațiuni comerciale | Nu | Nu creează cereri comerciale proprii; execută cererea autorizată existentă |

Un antreprenor nu primește drepturi fiindcă are acest titlu: configurația sa poate cumula explicit operator, aprobator și solicitant. Un contabil sau colaborator extern primește atribuiri în aceeași matrice, nu acces global implicit. Administratorul platformei și suportul Bizix nu sunt aprobatori ai firmei prin poziția lor tehnică.

### 7.3 Aplicare în contractele S02

| Contract sau etapă | Verificare de acces propusă |
|---|---|
| `sales.quote.create` / `sales.quote.revise` și `invoicing.request.create` / `invoicing.request.update` | Drept de scriere a ciornei în domeniu; la o valoare manuală nouă/modificată, actor uman și eligibilitate de aprobator în același domeniu |
| `sales.quote.send.prepare` / `invoicing.intent.prepare` | Drept specific de pregătire și acces la surse; nu cere obligatoriu ca preparatorul să poată aproba sau introduce prețuri manuale |
| `workflow.intent.read` | Acces la intenția și datele necesare rolului; ciorna curentă nu înlocuiește snapshot-ul |
| `workflow.approval.decide` | Identitate umană, drept de aprobare pentru `sales.quote.send` ori `invoicing.issue`, acces la datele necesare și politica validă |
| `workflow.intent.execute` | Identitate umană desemnată pentru solicitare, drept asupra efectului specific, acces la intenție și aprobarea validă; dreptul generic de workflow nu permite orice emitere |
| `sales.quote.send` / `invoicing.issue` | Handler tehnic autorizat, context persistent și aceeași intenție aprobată; fără endpoint intern sau credențială de serviciu care să ocolească controalele |
| Citire factură/operațiune și reconciliere | Citire autorizată separat; reconcilierea necesită drept operațional dedicat și nu acordă drept de reemitere |

Drepturile pentru CRM, consemnarea acceptării clientului, editarea catalogului, configurarea emitentului/conexiunilor și trimiterea facturii către client rămân distincte. Aprobarea emiterii nu le acordă implicit. Nu se introduc aici endpoint-uri noi sau scheme diferite de contractele S02.

## 8. Securitate, politici și AI

### 8.1 Prețuri manuale și proveniență

**Decizie confirmată S03-D01 / RC-013:** numai aprobatorul poate introduce sau modifica un preț manual. Aplicarea propusă limitează dreptul la domeniul și resursele pentru care persoana este aprobator și cere drept de editare a ciornei. Faptul că o valoare este încă în ciornă nu permite ocolirea acestei reguli.

Backend-ul stabilește autorul și sursa valorii. Operatorul fără drept de aprobare și AI-ul pot folosi un preț deja introdus autorizat, dacă au acces la ciornă și drepturile pasului curent; nu trebuie să primească dreptul de preț manual doar pentru a pregăti intenția. Reutilizarea verifică înregistrarea și versiunea autorizate, nu acceptă simpla afirmație a clientului că valoarea a fost introdusă anterior de un aprobator.

La înlocuirea listei `lines` conform S02, păstrarea neschimbată a unei linii manuale existente se distinge de introducerea, copierea ca linie nouă sau schimbarea prețului. Referințele, valorile și proveniența sunt verificate server-side; forma tehnică a transferului între documente se fixează în S02/S07, fără a autoriza payload-uri manuale arbitrare.

O abatere de la prețul de catalog nu se prezintă ca preț de catalog nemodificat. Discounturile, cantitățile, moneda și condițiile au propriile validări; nu pot masca o modificare de preț neautorizată. Până la definirea unor reguli de discount/negociere în S07, nu se acordă operatorului sau AI-ului libertatea de a introduce reduceri discreționare. Un preț introdus manual nu devine automat articol/preț reutilizabil de catalog și nici angajament comercial aprobat.

Pentru `accepted_quote`, conținutul comercial rămâne cel din revizia acceptată. Nici aprobatorul de facturare nu poate rescrie liber prețul acceptat: diferențele cer rezolvarea de domeniu și, unde este necesar, o nouă acceptare a clientului. Dreptul de preț manual nu înlocuiește această condiție.

### 8.2 Cumulul rolurilor și aprobarea explicită

**Decizie confirmată S03-D02 / RC-014:** aceeași persoană poate pregăti și aproba propria intenție dacă are explicit ambele drepturi. Poate solicita și execuția numai dacă este desemnată separat. Prima versiune nu impune universal două persoane, dar nici nu pretinde o verificare independentă când identitățile coincid.

Introducerea unui preț de către aprobator, salvarea sau pregătirea nu constituie aprobare. Persoana verifică snapshot-ul fixat și consemnează decizia prin pasul dedicat. Aprobarea trimiterii ofertei, acceptarea clientului comercial și aprobarea emiterii sunt fapte distincte. Aprobatorul trebuie să vadă datele necesare deciziei; accesul la un sumar insuficient nu îi permite să aprobe un efect ascuns.

### 8.3 Solicitantul și executantul tehnic

**Decizie confirmată S03-D03 / RC-015:** numai un om desemnat solicită execuția în prima versiune. Desemnarea se poate acorda operatorului, aprobatorului sau altui om autorizat; nu cere ca acesta să fie persoana care a aprobat. Aprobarea nu pornește automat execuția și nu acordă solicitantului dreptul de a modifica efectul.

AI-ul nu poate folosi drepturile responsabilului uman pentru a apela `workflow.intent.execute`, nici când există o aprobare validă. Un workflow sau un client API automat nu se poate declara om printr-un câmp de input. Workerul consumă cererea umană autorizată prin propria identitate tehnică, păstrând solicitantul, inițiatorul și aprobatorul în audit. Reluarea tehnică sigură a aceleiași operațiuni în S08 nu este o nouă decizie comercială a AI-ului.

Solicitarea execuției de către AI sau automatizarea imediată după aprobare necesită o decizie ulterioară explicită, cu mandat, limite și probe; nu rezultă din simpla activare a asistentului.

### 8.4 Reverificare, revocare și acces administrativ

Propuneri de aplicare, încă de validat tehnic:

1. Autorizarea se verifică la fiecare citire/scriere și tranziție, la solicitarea execuției și înainte de dispatch, inclusiv după coadă sau restart. Se verifică drepturile actuale ale actorilor și mandatul pe care se bazează operațiunea, aprobatorul și solicitantul, nu numai validitatea tokenului inițial.
2. Retragerea apartenenței/dreptului relevant, expirarea/revocarea mandatului sau a aprobării blochează efectul încă neexecutat. Lipsa serviciului de politici nu permite continuarea pe baza presupunerii că drepturile nu s-au schimbat. Mecanismul de propagare și fereastra concurentă cu dispatch-ul trebuie probate.
3. Istoricul rămâne intact; preluarea de către alt om se consemnează explicit, fără rescrierea autorului. Dacă aprobarea nu mai este validă, se folosește traseul de reaprobare S02/S08; reatribuirea unei sarcini nu validează singură aprobarea veche.
4. După posibilul dispatch, revocarea nu garantează anularea efectului. Se păstrează rezervarea, rezultatul necunoscut și reconcilierea; nu se reemite și nu se schimbă automat furnizorul.
5. RC-016 rezervă responsabilului de acces desemnarea aprobatorilor și solicitanților. Acesta își poate atribui explicit roluri operaționale, în limitele politicii, cu audit; rolul generic de administrator, promptul sau impersonarea nu țin locul atribuirii. Nu există administratori delegați în prima versiune. RC-018–RC-020 definesc direcțiile pentru prima desemnare, transfer și recuperare; mecanismele din capitolele 6.5–6.7 necesită probe. Nu sunt permise schimbări care lasă firma fără acces administrativ recuperabil printr-un flux verificat.
6. Auditul leagă acordările/revocările, schimbarea prețului, pregătirea, decizia, solicitarea, refuzurile și efectul de actor, tenant, resursă, versiune și politică. Nu copiază secrete sau payload-uri personale complete în loguri ori pe blockchain.

Izolarea se aplică și listelor, căutării, fișierelor, cache-urilor, evenimentelor, rezultatelor memorate și memoriei AI. Propunere de nedivulgare: resursele din afara accesului se prezintă ca indisponibile/inexistente, fără confirmarea existenței lor; lipsa dreptului pentru o acțiune asupra unui obiect vizibil poate avea un refuz explicativ. Codurile și răspunsurile uniforme se definitivează în contractele S02.

## 9. Alternative și decizii

**Sursa confirmărilor:** răspunsurile explicite ale inițiatorului din 2026-09-15: trei alegeri în v0.1, două despre autoritate și niveluri în v0.2 și trei despre desemnarea automatizată, transfer și recuperare în v0.3. D01–D03 și D07–D11 sunt decizii pentru prima versiune. D04–D06 păstrează propunerile de mecanism și indică limitele clarificate prin confirmări. Registrul general păstrează corespondențele RC, nu o a doua matrice de permisiuni.

| ID local / general | Problemă și alternative | Alegere sau recomandare și motiv | Probe / condiție de reevaluare | Statut |
|---|---|---|---|---|
| S03-D01 / RC-013 | Preț manual pentru orice operator, prin drept independent sau numai aprobator | Numai aprobatorul; controlul valorilor punctuale rămâne la omul desemnat să își asume angajamentul | Refuz pentru operator/AI, proveniență verificată; limitele pe domeniu și scrierea ciornei sunt detalii propuse; reevaluare dacă pilotul cere delegare mai fină | Confirmat de inițiator la 2026-09-15 |
| S03-D02 / RC-014 | Două persoane obligatorii versus cumul explicit | Aceeași persoană poate pregăti și aproba cu ambele drepturi; nu blocăm firma mică, dar păstrăm decizia explicită | Flux cu roluri cumulate și audit; fără pretenție de control independent; reguli de separare mai stricte necesită decizie ulterioară | Confirmat de inițiator la 2026-09-15 |
| S03-D03 / RC-015 | Solicitare numai de aprobator, de om desemnat sau și de AI | Om desemnat, drept separat acordabil operatorului ori aprobatorului; AI-ul nu solicită execuția în prima versiune | Refuz AI/API tehnic, cerere umană după aprobare și worker distinct; autonomie extinsă numai prin decizie și probe ulterioare | Confirmat de inițiator la 2026-09-15 |
| S03-D04 | Roluri globale versus roluri limitate la tenant/domeniu/resurse | Roluri cumulabile cu verificări contextuale și refuz implicit; aplicarea celor două niveluri din D08 nu transferă drepturi între acțiuni | Matrice de teste, repartizarea resurselor și cost de administrare; motorul și schema finală rămân deschise | Nivelurile sunt confirmate prin D08; mecanismul rămâne propus |
| S03-D05 | Administrator universal versus administrare distinctă de operare | Fără drepturi comerciale automate; D07 permite responsabilului desemnat atribuirea explicită a propriilor roluri | Probe de escaladare și flux explicit de acordare; direcțiile pentru desemnare/transfer/recuperare sunt confirmate prin D09–D11, mecanismele rămân de validat | Autoritatea este confirmată prin D07; mecanismul rămâne propus |
| S03-D06 | Verificare doar la autentificare versus și la fiecare efect | Reverificare server-side, revocare și audit conform capitolului 8 | Cozi, cache, restart, drept retras și concurență cu dispatch-ul; limite măsurate înainte de acceptare | Propus ca mecanism de aplicare a cerințelor S02 |
| S03-D07 / RC-016 | Acordare numai de responsabil, prin administratori delegați sau cu dublă confirmare | Numai responsabilul firmei desemnat explicit acordă/retrage rolurile sensibile; își poate atribui explicit roluri operaționale, cu audit; delegarea administrării se amână | Teste de acordare/retragere, propriile atribuiri și refuz pentru alte identități; D09–D11 stabilesc direcțiile pentru continuitatea autorității, fără validarea mecanismelor; reevaluare dacă firma pilot cere administrare delegată | Confirmat de inițiator la 2026-09-15, în v0.2 |
| S03-D08 / RC-017 | Toate documentele domeniului, numai documente atribuite sau ambele niveluri | Documente atribuite implicit; întregul domeniu numai prin acordare explicită a responsabilului; vizibilitatea și acțiunile sunt distincte | Teste per drept, resursă și tenant, restrângere de nivel și repartizare; relațiile și mecanismele din capitolul 6.4 rămân propuse; reevaluare pentru nevoi reale de echipe/câmpuri confidențiale | Confirmat de inițiator la 2026-09-15, în v0.2 |
| S03-D09 / RC-018 | Onboarding asistat, explorare separată cu activare verificată sau verificare automatizată | Verificare automatizată de identitate și reprezentare înaintea primei activări reale; fără autoritate acordată doar prin email/CUI | Furnizor/surse, dovezi acceptate, autenticitate, actualitate, reprezentare, acoperire și excepții; fără rezultat verificabil, activarea rămâne blocată; o alternativă manuală de onboarding cere altă decizie | Confirmat de inițiator la 2026-09-15, în v0.3; mecanism încă neales |
| S03-D10 / RC-019 | Transfer voluntar cu confirmare reciprocă versus transfer asistat de fiecare dată | Titularul inițiază după reautentificare, destinatarul acceptă din contul propriu verificat cu MFA; autoritatea se mută numai la finalizare sigură | Probe de acceptare, expirare/anulare, concurență, restart și revocarea accesului administrativ vechi; fără copierea rolurilor comerciale; cazul titularului indisponibil aparține D11 | Confirmat de inițiator la 2026-09-15, în v0.3 |
| S03-D11 / RC-020 | Factori proprii plus asistență versus numai recuperare asistată | Factori alternativi/coduri pregătite anterior, apoi asistență verificată când lipsesc; înlocuirea titularului este distinctă de recuperarea contului | Probe de unică folosință, MFA, identitate/reprezentare, notificări, audit și contestare; dovezile, verificatorii și ferestrele procedurii sunt de stabilit înainte de oferirea asistenței | Confirmat de inițiator la 2026-09-15, în v0.3 |

Confirmările nu stabilesc persoane reale, praguri financiare, durate de aprobare, reguli fiscale sau un produs IAM. Schimbarea lor cere o nouă decizie explicită și actualizarea secțiunilor afectate.

## 10. Plan de implementare pentru livrabil

Pașii de mai jos sunt pentru etapa tehnică ulterioară, neautorizată prin redactarea acestui document.

| Pas | Rezultat observabil | Dependențe și verificare | Responsabil |
|---|---|---|---|
| P1 — Revizuire și atribuiri | Aplicarea RC-016–RC-020: repartizare, dovezi și mecanism de verificare automatizată evaluate, transfer și recuperare delimitate | S01/S02/S04/S05/S07/S16; S09 pentru verificator extern; exemple cu o persoană, roluri separate, dovezi neconcludente și titular indisponibil | Produs/securitate, de desemnat |
| P2 — Contracte și teste negative | Permisiuni, contexte și erori în schemele executabile; teste inițial eșuate pentru comportamente neimplementate | S02/S07/S08/S11; cazurile din capitolul 11 | Tehnic/verificare, de desemnat |
| P3 — Aplicare end-to-end | Backend, worker și adaptor AI folosesc aceeași autoritate de politici | S04/S05/S08; fără ocolire prin apel direct sau job vechi | Tehnic, de desemnat |
| P4 — Acceptare delimitată | Probe de izolare, revocare, audit și flux manual cu configurația livrată | S06/S16; rolurile și blocajele sunt inteligibile, iar backend-ul le impune | Verificare/operare, de desemnat |

Separarea obligatorie între persoane, pragurile de aprobare, delegările temporare și solicitarea execuției de către AI sunt extensii posibile, nu funcții declarate livrate sau blocaje obligatorii pentru explorarea curentă.

## 11. Teste și criterii de acceptare

Cazurile sunt cerințe de verificat, nu rezultate obținute. Se aplică ofertelor și ambelor origini ale facturării, după relevanță, prin UI, API, adaptor AI și worker.

| ID | Scenariu și rezultat așteptat | Metodă / dovadă necesară |
|---|---|---|
| T01 | Operator fără aprobare introduce sau schimbă un preț manual: refuz inclusiv în ciornă incompletă; aprobatorul cu acces la scriere reușește fără efect de emitere | Teste de contract/domeniu, valoare și autor persistate corect |
| T02 | AI încearcă să introducă/schimbe un preț prin `source = manual`, autor uman fictiv sau preț de catalog fals: refuz; responsabilul uman aprobator nu transferă dreptul AI-ului | Teste de identitate și proveniență prin adaptor și API; păstrarea neschimbată a valorii umane autorizate se verifică separat în T03 |
| T03 | Operatorul/AI-ul pregătește o ciornă cu preț introdus anterior de aprobator: permis în mandat, fără schimbarea prețului/provenienței; prețul arbitrar cu aceeași etichetă este refuzat | Teste cu versiuni, linii păstrate, înlocuite sau copiate și referințe neautorizate |
| T04 | Ciornă incompletă: salvare permisă, pregătire refuzată; dreptul de pregătire nu conferă aprobare sau solicitare | Teste de contract cu zero operațiuni de emitere/trimitere |
| T05 | Aprobatorul de oferte nu aprobă emiterea și nu introduce prețuri în facturare fără atribuirea acelui domeniu; drepturile firmei A nu se propagă în B | Teste negative pe domeniu, resursă și tenant |
| T06 | Aceeași persoană cu drepturi cumulate pregătește, aprobă explicit și solicită execuția; lipsa oricăruia dintre drepturi blochează pasul respectiv | Flux end-to-end și trei acțiuni distincte în audit; fără aprobare dedusă din editare |
| T07 | Un om desemnat diferit de aprobator poate solicita efectul aprobat; aprobatorul fără desemnare nu poate solicita; desemnarea fără aprobare nu permite efectul | Teste cu identități separate și snapshot identic |
| T08 | AI și serviciul API nu solicită execuția, nici cu aprobare validă; workerul execută numai cererea umană autorizată și nu acceptă un apel direct neautorizat | Teste de integrare și audit al lanțului complet |
| T09 | Preț, cumpărător, document sau sistem schimbat după aprobare: intenția veche nu se execută; prețul ofertei acceptate nu este înlocuit liber de aprobator | Teste S02/S07/S08 cu succesor și reaprobare unde sunt necesare |
| T10 | Drept/mandat/aprobare revocate ori expirate în coadă sau înainte de dispatch: efectul încă neexecutat este blocat; politica indisponibilă nu acordă acces | Teste cu restart, cache și intercalări controlate; măsurarea propagării revocării |
| T11 | Dublu clic, doi solicitanți sau repetare după timeout: aceeași operațiune, nu al doilea efect; după posibilul dispatch, revocarea nu simulează anularea | Teste de concurență/reconciliere S08/S09 și evidența rezervării |
| T12 | Aprobare fără acces la datele necesare: refuz; listele, fișierele, cache-urile, memoria AI și rezultatele memorate nu divulgă resurse neautorizate | Teste negative în cel puțin două tenants, cu răspunsuri de nedivulgare |
| T13 | Administratorul, suportul sau instalarea unui modul nu acordă implicit preț manual, aprobare sau solicitare; acordările nepermise sunt refuzate | Teste pe fluxul de administrare definit în P1, audit al acordării/revocării |
| T14 | Reducere sau alt câmp folosit pentru a masca un preț neautorizat: refuz conform regulilor S07; emiterea nu trimite implicit factura și nu scrie în catalog/CRM | Teste de domeniu și verificarea efectelor secundare absente |
| T15 | Responsabilul desemnat acordă/retrage roluri sensibile și își atribuie explicit roluri operaționale; fără atribuirea comercială nu poate citi, aproba sau solicita execuția doar fiindcă administrează accesul | Teste de autorizare și audit cu rolul administrativ separat de cel comercial, fără autoacordare tacită |
| T16 | Operator, aprobator, administrator generic, AI sau suport fără autoritatea RC-016 încearcă să acorde roluri ori să extindă nivelul: refuz; responsabilul firmei A nu administrează firma B | Teste negative prin UI/API; nicio cale de administrator delegat activată implicit |
| T17 | Rol nou cu nivel implicit: acces numai la documente atribuite; atribuirea documentului fără rolul necesar nu permite aprobarea, iar simpla sarcină sau cunoașterea ID-ului nu acordă acces | Teste cu doi utilizatori, document repartizat/nerepartizat și sarcină atribuită fără drepturi |
| T18 | Citire pe întregul domeniu + aprobare numai pe documente atribuite: vede domeniul, dar nu aprobă un document nerepartizat; aprobarea pe întregul domeniu nu acordă solicitarea execuției sau alt domeniu | Teste de combinare a acordărilor, fără combinarea incorectă a acțiunii unui rol cu nivelul altuia |
| T19 | Retragerea repartizării ori restrângerea nivelului blochează accesul pierdut și efectul încă neexecutat inclusiv după cache/coadă/restart; auditul rămâne, fără reemitere dacă dispatch-ul este posibil | Teste S03/S05/S08 cu verificare și a altor acordări încă valabile, fără păstrarea tacită a nivelului vechi |
| T20 | Ciornă creată autorizat cu repartizare server-side către creator: acces în limitele rolului; schimbarea câmpului de responsabil nu permite preluarea unui document străin; intenția și rezultatul nu divulgă surse neautorizate | Teste pe propunerea de repartizare și relațiile S02/S07/S08, inclusiv AI în mandat și ambele origini ale facturării |
| T21 | Prima desemnare cere identitatea și reprezentarea verificate automat, legate de cerere/firmă, plus acceptarea persoanei; email, CUI, MFA ori identitate verificată fără reprezentare nu activează autoritatea | Teste cu probe valide/invalide, expirate, pentru altă persoană/firmă și răspuns de verificator falsificat; apoi integrare cu mecanismul ales |
| T22 | Verificator indisponibil, dovezi insuficiente sau rezultat neconcludent: fără autoritate activată; suportul nu aprobă manual onboarding-ul folosind procedura de recuperare | Teste de indisponibilitate și stări incomplete; niciun fallback privilegiat sau verdict liber AI |
| T23 | Două cereri, replay al rezultatului, restart sau onboarding pentru tenant deja administrat nu creează doi titulari și nu înlocuiesc titularul existent | Teste de unicitate, legare a dovezii de cerere și concurență; conflictul nu divulgă datele firmei |
| T24 | Transfer inițiat fără reautentificare, acceptare de alt cont ori fără MFA: refuz; invitația în așteptare/refuzată/expirată/anulată nu mută autoritatea | Teste pe identitățile fixate și stările cererii; numai confirmarea reciprocă validă permite finalizarea |
| T25 | Acceptări sau transferuri concurente și restart la finalizare: un singur titular, fără interval gol; vechiul titular nu administrează prin token/cache vechi | Teste de tranziție atomică/versionată, idempotență și reverificare server-side |
| T26 | Transferul nu copiază aprobări, mandate sau roluri comerciale pe noul actor; tratarea rolurilor fostului titular este explicită, iar operațiunile în curs nu sunt relansate | Teste de audit, revocare selectată și continuitate S02/S08 |
| T27 | Recuperarea cu factor/cod valid păstrează aceeași identitate și drepturile curente; codul consumat, concurent sau invalidat nu se reutilizează; resetarea parolei nu ocolește MFA | Teste ale mecanismului de identitate, consum atomic, sesiuni/factori înlocuiți și apartenențe revocate în mai mulți tenants |
| T28 | Recuperarea asistată a contului verifică titularul; înlocuirea responsabilului cere separat autoritatea solicitantului asupra firmei și acceptarea noii identități; lipsa răspunsului fostului titular nu este suficientă | Cazuri controlate de dovezi insuficiente/contestate și înlocuire validă; fără impersonare, recreare de tenant sau drepturi comerciale copiate |
| T29 | Asistența operează numai cazul autorizat, cu probe protejate, decizie, notificări și audit; codurile/factorii nu ajung la suport sau în loguri și cazul nu acordă acces comercial permanent | Revizuire și teste ale procedurii privilegiate, inclusiv verificarea independentă propusă și accesul la dovezi |

Testele de cod și comenzile se preiau din proiectele de implementare după inspectarea lor. Verificarea documentară a matricei nu demonstrează izolarea tehnică, eficacitatea revocării sau corectitudinea fiscală.

## 12. Livrare și operare

Înainte de livrare se stabilesc cu S04/S05/S08/S16 autentificarea pentru acțiuni sensibile, propagarea revocării, auditul protejat, alertele pentru refuzuri repetate și responsabilul pentru aprobări blocate. Sesiunile, tokenurile și cache-urile nu pot păstra nelimitat drepturi retrase.

Operarea trebuie să permită oprirea efectelor la incident și preluarea autorizată a cazurilor fără ștergerea istoricului. După restaurare se reverifică atribuirile și operațiunile înainte de reactivarea workerilor; un backup vechi nu reautorizează tacit drepturi revocate. Pragurile, costul verificării politicilor, obiectivele de recuperare și procedurile de acces excepțional rămân de definit și probat, nu SLA-uri declarate aici.

## 13. Personalizare, reutilizare și drepturi

Firma poate primi atribuiri și configurații de acces adaptate în limitele funcțiilor livrate; un șablon de rol nu include implicit membrii, datele sau mandatele altei firme. Personalizarea ori instalarea unui modul poate solicita drepturi, nu le poate acorda singură.

Nu se permite ocolirea aprobării, a restricției pentru prețuri manuale sau a cererii umane prin prompt, extensie, import ori configurație. Politici mai stricte și praguri pe sume pot fi evaluate ulterior cu S01/S07/S12; nu presupunem că sunt implementate în prima versiune.

## 14. Migrare și continuitate

Rolurile și conturile existente se inventariază înainte de mapare. Un rol istoric de administrator nu devine automat aprobator sau solicitant pentru toate firmele. Se păstrează proveniența datelor și deciziilor vechi; aprobările în curs fără identitate, snapshot sau politică verificabile nu se declară valide prin migrare.

Tranziția cere export, mapări verificate, procedură de recuperare a accesului și acceptare separată. Nu sunt autorizate aici modificări ale conturilor, revocări reale, ștergeri, schimbări de politici active sau operațiuni de producție.

## 15. Riscuri și întrebări deschise

| Risc / întrebare | Impact și condiție de rezolvare | Responsabil | Blochează |
|---|---|---|---|
| Ce mecanism automat și ce dovezi verifică identitatea și reprezentarea firmei? | RC-018 alege automatizarea, nu furnizorul sau suficiența probelor; trebuie validate autenticitatea, actualitatea, mandatul/reprezentarea, acoperirea, protecția datelor și costul; fără rezultat demonstrabil, activarea rămâne blocată | S03/S04/S05/S09 și responsabil juridic/date, de desemnat | Prima desemnare și activarea cu date/operațiuni reale |
| Cum se finalizează sigur transferul și cum se tratează rolurile fostului titular? | RC-019 fixează confirmarea reciprocă; tranziția atomică, expirarea/anularea, verificarea identităților, revocarea administrativă și revizuirea rolurilor comerciale necesită contracte și probe | S02/S03/S05/S08, de desemnat | Transferul real al autorității |
| Ce dovezi și verificatori autorizează recuperarea asistată și înlocuirea titularului? | RC-020 fixează cele două căi, nu procedura completă; sunt necesare factori acceptați, o decizie asupra verificării independente propuse, notificare/contestare, retenție, ferestre și responsabili; fără acestea nu se promite asistență operabilă | S03/S04/S05/S16 și responsabil juridic/date, de desemnat | Recuperarea și continuitatea administrării într-un sistem real |
| Cum sunt repartizate documentele și legate intențiile/rezultatele de accesul lor? | RC-017 confirmă cele două niveluri; repartizarea, accesul la surse, combinarea acordărilor și propagarea/revocarea pe obiectele S02 cer scheme și probe; detaliile din capitolul 6.4 rămân propuse | S02/S03/S05/S07/S08, de desemnat | Contractele executabile și acceptarea controlului accesului la documente |
| Durata aprobării/mandatului și limitele valorice | Fără praguri sau durate inventate; se cere o politică explicită adecvată firmei și scenariului | S03/S07/S11, de desemnat | Activarea politicii cu date reale |
| Discounturi, cantități și transferul prețurilor autorizate între documente | Pot ocoli controlul prețului ori pierde proveniența; reguli de domeniu și contract de reutilizare verificabile | S02/S03/S07, de desemnat | Contractele executabile pentru prețuri și negociere |
| Revocare și dispatch concurent | O decizie veche poate supraviețui într-un cache/job; mecanismul și limita ferestrei de revocare trebuie probate | S03/S04/S08, de desemnat | Acceptarea execuției durabile |
| Autentificare, izolare și acces de suport încă nedetaliate | Matricea funcțională singură nu asigură protecția datelor; sunt necesare restul modelului de amenințări și probele de izolare | S03/S04/S16, de desemnat | Operarea cu date reale |
| Retenția, exportul și protecția auditului | Nu există încă politici complete pentru fiecare categorie de date și obligație aplicabilă | S03/S07/S16, de desemnat | Operarea și migrarea datelor reale |
| Fără pilot și fără inventar tehnic verificat | Nu putem declara matricea validată comercial sau controalele implementate | S01/tehnic, de desemnat | Acceptarea produsului și reutilizarea, nu redactarea curentă |

Răspunsurile confirmate sunt în capitolul 9. RC-016/RC-017 fixează autoritatea și nivelurile de acces, iar RC-018–RC-020 direcțiile pentru prima desemnare, transfer și recuperare; mecanismele de aplicare nu sunt validate prin aceste alegeri. Următorul pas este stabilirea dovezilor acceptabile pentru identitate și reprezentare și evaluarea mecanismului automat care le poate verifica. Procedura asistată de recuperare și repartizarea documentelor se rafinează separat; S07 trebuie să delimiteze în paralel regulile comerciale și fiscale.

## 16. Checklist de aprobare

Lista privește aprobarea livrabilului documentar delimitat și rămâne nebifată; cele opt confirmări de produs sunt consemnate separat.

- [ ] Matricea, nivelurile pe drept, repartizarea documentelor și accesul la surse sunt suficient detaliate pentru livrabil.
- [ ] Dovezile și mecanismul pentru desemnarea automatizată, transferul și recuperarea autorității au verificări și responsabili expliciți; administrarea delegată rămâne exclusă din prima versiune.
- [ ] Recuperarea contului și înlocuirea titularului au proceduri distincte, fără onboarding manual implicit sau acces comercial permanent pentru suport.
- [ ] Proveniența prețurilor și regulile S07 împiedică ocolirea drepturilor prin alte câmpuri sau canale.
- [ ] Cumulul drepturilor păstrează aprobarea și solicitarea execuției ca pași distincți.
- [ ] Contractele S02/S08 și mandatul S11 aplică restricțiile pentru oameni, AI și worker.
- [ ] Revocarea, izolarea, nedivulgarea și auditul au mecanisme și criterii de probă suficiente pentru implementarea vizată.
- [ ] Riscurile și întrebările blocante au decizie sau condiție de rezolvare și responsabil.
- [ ] Planul general și secțiunile dependente reflectă deciziile fără matrice concurente.
- [ ] Aprobarea identifică persoana, versiunea S03 și livrabilul delimitat.

Aprobarea documentului nu înseamnă acceptarea implementării sau autorizarea emiterii de facturi, trimiterii de mesaje ori modificării drepturilor într-un mediu real.
