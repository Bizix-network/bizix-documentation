# BiziX — Plan general de reconstrucție a platformei

> **Versiune:** 0.2 — plan de lucru, mini-brief S01 inițiat
> **Data:** 2026-09-13
> **Nivel:** Plan general al programului; planurile detaliate se elaborează separat, pe secțiuni.
> **Stadiu:** Documentare și decizii. Nu reprezintă implementare, audit tehnic sau angajament de calendar.

## 1. Mandat și obiectiv

Reconstrucția pornește de la valoarea pentru client, nu de la obligația de a implementa whitepaper-ul existent. Putem păstra, adapta, înlocui sau elimina componente și promisiuni de produs dacă rezultatul este o platformă mai coerentă, utilă și sustenabilă.

> **Avantajul Bizix nu ar trebui să fie că poate genera software, ci că poate transforma acel software într-un sistem de business coerent, sigur și întreținut în timp.**

Ținta este un spațiu operațional unic pentru firme mici și medii, cu aplicații native, servicii comune, angajați AI și integrare cu platformele externe pe care clientul dorește să le păstreze.

**„Totul într-un singur loc” este o experiență unificată, nu o obligație de a folosi exclusiv software Bizix.** CRM-ul poate fi Bizix, facturarea poate rămâne FGO, iar utilizatorul trebuie să înțeleagă ce este conectat, ce date sunt actuale și ce acțiuni poate executa.

Planul este complet la nivel de domenii, responsabilități, dependențe și verificări. Nu încearcă să definească anticipat toate funcțiile tuturor industriilor. Detaliile și alegerile tehnologice se fixează după verificarea ipotezelor relevante.

### Relația cu documentele existente

- [Analiza de direcție](../ANALIZA_DIRECTIE_AI_APPS.md) păstrează argumentele și principiile discutate, inclusiv cazul FGO și sursele consultate.
- [Whitepaper-ul](../../README.md) și [roadmap-ul istoric](../IMPLEMENTATION_ROADMAP.md) sunt referințe, nu criterii obligatorii de succes pentru reconstrucție.
- [Documentația tehnică istorică](../TECH_INDEX.md) este punct de pornire pentru inventar, nu dovadă a stării actuale a codului.
- Acest plan nu modifică automat documentele vechi, deployment-urile, contractele comerciale sau datele clienților. Obligațiile existente trebuie verificate separat înainte de schimbarea ofertei sau migrare.

## 2. Cum organizăm această zonă

Fișierele acestei etape sunt:

```text
docs/reconstructie/
├── README.md
└── TEMPLATE_PLAN_SECTIUNE.md
```

Acest README este planul general și punctul de intrare. [Template-ul de secțiune](./TEMPLATE_PLAN_SECTIUNE.md) definește formatul comun pentru aprofundare.

Planurile detaliate vor fi create gradual în subdirectorul `sectiuni/`, când abordăm fiecare domeniu. Numele lor sunt rezervate în catalogul de mai jos, dar fișierele și subdirectorul nu sunt create în această etapă. Nu generăm documente goale prezentate drept planuri finalizate.

### Reguli de lucru

1. Validăm direcția și granițele planului general.
2. Elaborăm o secțiune, discutăm alternativele și deciziile încă deschise.
3. Verificăm dependențele și actualizăm planurile afectate, fără a copia contradictoriu aceleași reguli în mai multe locuri.
4. Aprobăm secțiunea pentru un livrabil delimitat, nu pentru întregul domeniu pe termen nelimitat.
5. Trecem la implementare numai printr-o etapă explicită ulterioară; aprobarea unui document nu autorizează operațiuni de producție.
6. Validăm livrabilul prin probe și folosirea reală, apoi revizuim planul.

Starea documentului și starea implementării sunt distincte. Un plan poate fi aprobat fără ca funcția să fie implementată; o componentă veche poate exista fără să fie acceptată pentru noua platformă.

## 3. Ce schimbăm în mod deliberat

Următoarele sunt recomandări de reconstrucție, supuse validării planului general:

| Abordare de reconsiderat | Direcție recomandată | Valoare urmărită |
|---|---|---|
| Alinierea procentuală cu whitepaper-ul | Rezolvarea unor procese reale, măsurarea adoptării și a costului complet | Priorități determinate de client, nu de document |
| Catalog de aplicații independente | Module native pe contracte comune, plus conectori externi | Integrare și mentenanță coerente |
| Câte un proiect divergent pentru fiecare client | Compoziție, configurări și extensii versionate | Personalizare care supraviețuiește upgrade-urilor |
| Multe portale separate de la început | Un spațiu principal, cu roluri și zone dedicate; separare ulterioară doar când este justificată | Mai puțină duplicare și experiență consecventă |
| VM privată ca model implicit pentru orice client | Izolare verificată și medii potrivite încărcării; variantă dedicată unde este necesară | Echilibru între control, cost și operare |
| AI ca chat adăugat peste aplicații | Operator al acelorași capabilități folosite de oameni | Muncă executabilă, controlată și auditabilă |
| Blockchain și token ca precondiție pentru orice funcție | Trust Service cu rol demonstrabil; mecanisme economice on-chain distincte | Încredere fără fricțiune inutilă |
| Integrare promisă ulterior | Interoperabilitate inclusă în contractele inițiale și verificată devreme | Adoptare graduală, fără migrare forțată |
| Marketplace public înainte de validarea produsului | Catalog intern verificat, apoi distribuire controlată | Calitate, drepturi și mentenanță înainte de volum |
| Securitate, backup și teste la final | Cerințe și verificări încă din primul proces | Evitarea unei platforme rapide de demonstrat, dar greu de operat |

Nu păstrăm un element doar fiindcă există și nu îl rescriem doar fiindcă este vechi. Pentru fiecare componentă evaluăm utilitatea, securitatea, costul adaptării, testabilitatea, licența și dependențele.

### Ce nu este obiectivul primei versiuni

- Un ERP complet, un editor universal de software și toate aplicațiile din vechiul catalog.
- Un limbaj proprietar în care trebuie exprimat orice proces posibil.
- Personalizare arbitrară cu mentenanță nelimitată inclusă implicit.
- O rețea de agenți care discută între ei pentru fiecare operațiune simplă.
- Rescrierea protocoalelor SMTP/SMPP, a identității sau a bazelor de date.
- Eliminarea tuturor furnizorilor externi ori migrarea obligatorie a facturării clienților în Bizix.
- Lansarea unui token, a unui marketplace public sau a tuturor portalelor ca precondiție pentru primul client.

## 4. Modelul țintă al platformei

```text
                 Spațiul de lucru al firmei
          Interfețe | Oameni | Angajați AI | Workflow-uri
                              |
          Capabilități versionate + identitate + politici
                              |
           Module native și adaptoare pentru servicii externe
                              |
     Core | Procese durabile | Notificări | Fișiere | AI | Trust
                              |
        Infrastructură, livrare, observabilitate și recuperare

AI Constructor → specificații / cod / migrări → verificare → publicare
AI Operator    → capabilități publicate → mandat verificat → execuție
```

Acestea sunt granițe logice, nu o cerință de a crea câte un microserviciu, repository sau echipă pentru fiecare componentă.

### Reguli structurale de păstrat în toate secțiunile

1. Compania, actorul și mandatul sunt explicite pentru fiecare operațiune. AI-ul nu are o cale de acces privilegiată care ocolește regulile aplicației.
2. Fiecare categorie de date are o sursă autoritativă. Identificatorii, proveniența și regulile de conflict sunt stabilite înaintea sincronizării.
3. Modulele expun contracte de acțiuni și evenimente, nu acces liber la tabelele celorlalte module.
4. Procesele, aprobările și intențiile de acțiune sunt persistente și pot fi reluate sau preluate de oameni.
5. AI Constructor este separat de AI Operator prin identități, instrumente și medii de execuție.
6. Configurările și extensiile clienților sunt separate de baza comună; fiecare versiune are migrări și compatibilități declarate.
7. Conectorii, modelele AI și backend-ul de trust sunt înlocuibili prin adaptoare, fără a pretinde că toți furnizorii au aceleași capabilități.
8. Publicarea și descoperirea unei capabilități nu acordă automat permisiunea de utilizare.
9. Auditul, costurile, izolarea, backup-ul și portabilitatea sunt parte din produs, nu servicii de adăugat la final.
10. Rezultatele necunoscute sau neconfirmate sunt afișate ca atare. Un timeout nu este dovada că o operațiune externă nu s-a executat.

## 5. Catalogul planurilor detaliate

**Status inițial:** toate cele 16 planuri de mai jos sunt de elaborat. Starea implementărilor vechi este de verificat. Numerotarea reprezintă domenii de lucru, nu o ordine rigidă de implementare integrală.

| ID | Fișier planificat în `sectiuni/` | Responsabilitate și livrabil principal |
|---|---|---|
| S01 | `01_PRODUS_ECONOMIE_PILOT.md` | Segmentul de client, problema, procesul pilot, oferta, costurile, criteriile de valoare și excluderile |
| S02 | `02_ARHITECTURA_CONTRACTE_DATE.md` | Granițe de module, model minim de date, proprietatea datelor, contracte de capabilități/evenimente și reguli de evoluție |
| S03 | `03_IDENTITATE_ACCES_IZOLARE.md` | Companii, utilizatori, identități AI, delegare, autorizare, izolare, politici de date și model de amenințări |
| S04 | `04_INFRASTRUCTURA_LIVRARE_OPERARE.md` | Medii, CI/CD, artefacte, secrete, deployment, observabilitate, backup, recuperare și cost operațional |
| S05 | `05_CORE_PLATFORM_RUNTIME.md` | Registrul modulelor/capabilităților, instalări, configurări, versiuni, rutare, audit comun și ciclul de viață al aplicațiilor |
| S06 | `06_SPATIU_DE_LUCRU_UX.md` | Portal comun, design system, navigare, căutare autorizată, inbox, Action Center, administrare și transparența AI |
| S07 | `07_MODULE_BUSINESS_NATIVE.md` | Aplicațiile native ale pilotului, regulile de domeniu și funcționarea manuală completă; familiile ulterioare de module |
| S08 | `08_WORKFLOW_SARCINI_EVENIMENTE.md` | Procese durabile, sarcini pentru oameni/AI, aprobări, outbox, deduplicare, retry, compensare și reconciliere |
| S09 | `09_INTEGRARI_API_CONECTORI.md` | API-uri expuse și consumate, conexiuni per companie, adaptoare, mapări, capabilități externe și pilotul FGO |
| S10 | `10_SERVICII_COMUNE.md` | Notification Hub, email, SMS, fișiere, documente și semnare; limitele dintre serviciul Bizix și transport/furnizor |
| S11 | `11_ANGAJATI_AI.md` | AI Gateway operațional, competențe, mandate, instrumente, cunoaștere, memorie, bugete, evaluări și escaladare |
| S12 | `12_APP_STUDIO_PERSONALIZARE.md` | AI Constructor, App Spec, SDK, configurări, extensii, preview, migrări și publicare controlată |
| S13 | `13_REUTILIZARE_CATALOG_MARKETPLACE.md` | Generalizarea personalizărilor, drepturi, pachete verticale, competențe reutilizabile, review și distribuire |
| S14 | `14_TRUST_BLOCKCHAIN.md` | Dovezi, proveniență, semnare tehnică, backend blockchain, verificabilitate independentă și politici pentru chei |
| S15 | `15_BILLING_CONSUM_ADMINISTRARE.md` | Abonamentul Bizix, consum măsurat, limite, drepturi comerciale, reconciliere și instrumente administrative |
| S16 | `16_ACCEPTARE_MIGRARE_LANSARE.md` | Strategia de verificare transversală, pilot, migrare/coexistență, acceptare, lansare controlată, suport și retragerea controlată a componentelor vechi |

### Granițe care evită suprapunerea

- **S02 definește contractele**, iar secțiunile funcționale definesc implementarea și probele lor. Nu proiectăm câte un model incompatibil pentru UI, AI și conectori.
- **S03 definește politica de acces și protecția datelor**; S04, S05 și fiecare modul trebuie să o aplice și să demonstreze acest lucru.
- **S05 aplică disponibilitatea funcțiilor și instalărilor**, în timp ce S15 gestionează abonamentul, consumul și regulile comerciale care o influențează.
- **S06 deține experiența Action Center**, iar S08 deține starea și execuția aprobărilor. S03 controlează cine poate aproba.
- **S07 deține regulile de business**, iar S09 adaptează sistemele externe fără să creeze o a doua autoritate implicită asupra datelor.
- **S10 deține serviciul de notificări/documente**, iar S04 infrastructura pe care rulează. Nu confundăm trimiterea emailurilor cu un produs complet de căsuțe poștale.
- **S11 operează aplicațiile**, iar S12 le modifică. Nu împart implicit privilegii sau acces la producție.
- **S12 livrează personalizarea privată**, iar S13 decide cum devine produs reutilizabil și cine îl întreține.
- **S14 implementează dovada**, nu autorizează retrospectiv acțiunea și nu certifică singur corectitudinea ei.
- **S15 facturează serviciile Bizix**; facturile emise de firma clientului aparțin S07/S09. Sunt domenii distincte.
- **S16 începe de la prima etapă**, definind criteriile și probele. Nu este o fază târzie în care testăm pentru prima dată platforma.

## 6. Dependențe și ordinea de aprofundare

| Domeniu | Intrări principale necesare |
|---|---|
| S01 | Informații despre clienți, procese, constrângeri, echipă și ofertă |
| S02 | Rezultatele S01 și inventarul componentelor existente |
| S03, S04 | Contractele și granițele inițiale S02; S04 preia controalele de securitate din S03 |
| S05 | Contractele S02, identitatea/politicile S03 și mediul minim S04 |
| S06, S07, S08 | S01 și contractele minime S02/S03/S05; se dezvoltă împreună pentru primul proces |
| S09, S10 | Contractele comune și procesele S08; infrastructura și securitatea aferente |
| S11 | Capabilități funcționale S07/S08 și S09/S10 acolo unde mandatul agentului le folosește |
| S12 | Baza stabilă a modulelor, izolarea, artefactele și pipeline-ul S02–S05/S07 |
| S13 | Personalizare verificată S12, drepturi comerciale S01 și registrul S05; S15 înainte de monetizare automată |
| S14 | Model de încredere S02/S03, infrastructură S04 și audit/execuție S05/S08 |
| S15 | Oferta S01, identitate S03 și evenimentele de consum/procesele S05/S08 |
| S16 | Activitate transversală; criterii din S01, probe și acceptare pentru toate domeniile livrate |

Dependențele cer contractele și funcțiile necesare livrabilului curent, nu finalizarea întregii secțiuni. Altfel am bloca primul proces până la construirea întregii platforme.

Ordinea recomandată pentru discuțiile detaliate este:

1. S01 — ce problemă și ce client validăm.
2. S02 — granițe, date, acțiuni și interfețe de integrare.
3. S03 + baza S04/S16 — acces, izolare, livrare și probe de siguranță.
4. S05/S06/S07/S08 — un proces complet utilizabil manual.
5. S09/S10 — integrare externă și servicii efective.
6. S11 și aplicarea S14 — operare AI și dovezi pentru cazul ales.
7. S12/S13 — personalizare, reutilizare și actualizare comună.
8. Extinderea S15/S16 — oferta operabilă, migrarea și lansarea; elementele lor minime sunt pregătite anterior.

## 7. Etape de livrare și condiții de trecere

Etapele sunt propuneri bazate pe rezultate, nu termene calendaristice. Fiecare produce un flux demonstrabil și un set de probe. Dacă o condiție esențială nu este îndeplinită, restrângem sau reproiectăm înainte de extindere.

### M0 — Produsul pilot și adevărul tehnic

**Secțiuni principale:** S01, S02, baza S03/S04/S14/S16.

Livrabile:

- alegerea unei categorii de firme și a unui proces cu valoare verificabilă;
- inventarul codului, deployment-urilor, clienților și obligațiilor existente;
- clasificarea componentelor: reutilizare, adaptare, înlocuire sau retragere controlată;
- arhitectură de referință, surse autoritative și contracte inițiale;
- model de amenințări, politici de date și model de încredere pentru dovezi;
- criterii de acceptare, bugete, profil de încărcare și obiective de recuperare propuse pe baze explicite;
- responsabilități de produs, tehnice și operaționale asumate.

**Poarta de acceptare:** există un proces pilot delimitat, date suficiente pentru deciziile ireversibile și un plan pentru riscurile critice. Lipsa accesului la cod sau necunoașterea clienților activi nu se maschează cu procente istorice de progres.

### M1 — Un proces nativ complet, executabil manual

**Secțiuni principale:** minimum funcțional din S03–S08, infrastructura necesară S10 și probe S16.

Livrabile:

- acces la spațiul firmei și verificarea izolării în cel puțin două contexte de tenant;
- primul flux de business, de exemplu contact → programare sau oportunitate → ofertă;
- aceleași reguli implementate în capabilități, nu numai în UI;
- sarcini, evenimente și audit persistente;
- build/deploy repetabil, observabilitate și restaurare de bază verificată;
- execuție manuală fără dependență de modelul AI.

**Poarta de acceptare:** procesul este utilizabil cap-coadă și controalele de acces, consistența și recuperarea funcționează în scenariile definite. Nu este suficient un set de ecrane cu date demonstrative.

### M2 — Ecosistem deschis și servicii reale

**Secțiuni principale:** S09, S10, extinderea S06/S08/S15 și probe S16.

Livrabile:

- conectare autorizată per companie la platforma externă aleasă;
- cazul CRM Bizix + FGO ca referință, validat inițial într-un mediu de test autorizat;
- proprietatea datelor, mapări, actualitate și conflicte vizibile;
- notificări prin serviciile comune și evidența consumului;
- limite, revocare, timeout, rezultat necunoscut și reconciliere;
- acces la funcțiile externe disponibile din spațiul Bizix, fără promisiunea acoperirii întregului produs extern.

**Poarta de acceptare:** erorile și retry-urile nu produc efecte suplimentare necontrolate. Indisponibilitatea FGO nu declanșează emiterea aceleiași facturi în alt sistem. Funcțiile neacoperite de API sunt declarate, nu simulate ca succes.

### M3 — Primul angajat AI și încredere demonstrabilă

**Secțiuni principale:** S11, livrabilul de trust S14 pentru cazul ales, S03/S08/S15/S16.

Livrabile:

- un rol restrâns, cu identitate, responsabil uman, mandat și buget;
- acces exclusiv prin capabilitățile publicate și autorizate;
- început cu citire/ciorne, apoi operațiuni standard preautorizate după evaluare;
- predare către om și reluare după întreruperi;
- verificarea izolării contextului și a rezistenței la instrucțiuni malițioase din documente/emailuri;
- dovezi legate de acțiuni și versiuni, verificabile conform modelului de încredere ales.

Auditul începe în M1, iar modelul de trust în M0. M3 nu este începutul preocupării pentru securitate. Dacă oferim ancorare blockchain în pilot, o verificăm efectiv aici; dacă nu este încă disponibilă, funcția și afirmațiile comerciale aferente rămân explicit neactivate.

**Poarta de acceptare:** agentul economisește muncă pe procesul ales fără privilegii suplimentare față de mandat, respectă bugetul, iar erorile pot fi detectate și preluate de oameni. Dovezile nu sunt prezentate ca mai puternice decât implementarea verificată.

### M4 — Personalizare, reutilizare și upgrade

**Secțiuni principale:** S12, S13 și probe comune S02/S03/S04/S07/S16.

Livrabile:

- cerere în limbaj natural → specificație → configurare/extensie → preview → aprobare → artefact publicat;
- personalizare pentru clientul A fără copierea întregului proiect;
- transformarea părții generalizabile într-un modul cu drepturi și responsabilitate de mentenanță clare;
- utilizare la clientul B, cu alte configurări și fără datele private ale clientului A;
- actualizarea bazei comune și a modulului, păstrând personalizările și datele ambilor;
- export și restaurare în afara controlului operațional Bizix, cu dependențele și drepturile necesare.

**Poarta de acceptare:** nu este necesară reconstruirea manuală integrală a fiecărei instalări la upgrade. Dacă apar fork-uri permanente sau reutilizarea nu reduce costul total, se revizuiește modelul înainte de deschiderea catalogului.

### M5 — Ofertă operabilă și lansare controlată

**Secțiuni principale:** S15, S16 și controalele operaționale ale tuturor domeniilor livrate.

Livrabile:

- ofertă și limite comerciale, cost complet măsurat și evidența consumului;
- onboarding, suport, alerte și proceduri de intervenție;
- verificarea securității și a drepturilor de licențiere pentru configurația livrată;
- testarea obiectivelor de disponibilitate, recuperare și încărcare stabilite;
- migrare sau coexistență verificată, dacă există clienți actuali;
- lansare într-un grup controlat, colectarea rezultatelor și decizie de extindere;
- alinierea comunicării și documentelor comerciale cu funcțiile și garanțiile reale.

**Poarta de acceptare:** produsul poate fi operat și întreținut, nu doar demonstrat. Defectele critice cunoscute ale funcțiilor lansate sunt rezolvate; riscurile reziduale sunt documentate și asumate, iar suportul și continuitatea au responsabili.

După M5, extinderea pe industrii, competențe AI, conectori și marketplace este ghidată de utilizare și economie. Nu revenim automat la lista integrală de funcționalități din whitepaper.

## 8. Decizii de arhitectură: ce fixăm și ce testăm

### De fixat înainte de implementările care depind de ele

- definiția companiei/tenantului, apartenența utilizatorilor și identitățile tehnice;
- proprietatea datelor, identificatorii, contextul și regulile de autorizare;
- forma contractelor, versionarea și gestionarea efectelor externe;
- separarea AI Constructor de AI Operator și a codului comun de extensiile clientului;
- reprezentarea sarcinilor, aprobărilor, rezultatului necunoscut și auditului;
- separarea facturării platformei de facturarea afacerii clientului;
- politica de confidențialitate și drepturile de reutilizare;
- limitele garanțiilor de trust și cerințele de recuperare.

### De ales prin probe delimitate

| Alegere | Punct de plecare, nu concluzie definitivă | Probă necesară |
|---|---|---|
| Stack aplicații | TypeScript/Node.js, React/Next.js și componente reutilizabile | Un flux real, testabil și operabil; costul adaptării core-ului |
| Framework intern | SDK și convenții subțiri peste tehnologii mature | Aceeași personalizare în alternativele viabile, inclusiv upgrade și migrare |
| Persistență și izolare | PostgreSQL pentru date noi; core-ul existent poate rămâne separat | Izolare, concurență, restaurare per tenant și evoluția schemelor |
| Workflow engine | Motor durabil existent, integrat cu joburile potrivite | Restart, aprobare întârziată, retry și rezultat extern necunoscut |
| Deployment | Reutilizarea infrastructurii potrivite și medii containerizate | Livrare repetabilă, observabilitate, securitate și cost |
| Email/SMS | Servicii Bizix proprii peste transporturi/adaptoare mature | Livrabilitate, acorduri A2P, rate limits, licențe și operare |
| Modele AI | Gateway și politici independente de un singur model | Evaluări pe sarcinile pilotului, confidențialitate, cost și fallback uman |
| Blockchain | Trust Service cu backend înlocuibil; evaluarea BiziX Chain și a alternativelor | Verificabilitate independentă, securitate, compatibilitate și cost complet |

Aceste alegeri nu sunt amânate până după lansare: sunt testate înainte de livrabilul care se bazează pe ele. Nu fixăm anticipat un framework universal, un singur model AI sau o infrastructură complexă de orchestrare doar pentru o presupusă scară viitoare.

## 9. Reutilizarea și tranziția din platforma actuală

M0 produce un inventar verificat pentru identitate, portal, comenzi, provisioning, cozi, infrastructură și blockchain. Pentru fiecare element se notează codul/deployment-ul examinat, funcțiile reutilizabile, defectele, testele și decizia propusă.

| Situație constatată | Abordare |
|---|---|
| Nu există clienți sau date de producție de păstrat | Validare în medii separate; trecere la noul model fără obligații de compatibilitate inventate |
| Există piloți | Migrare/coexistență negociată, export și verificarea datelor înainte de schimbare |
| Există clienți în producție | Plan per tip de aplicație/date, sursă autoritativă clară și acceptare explicită a tranziției |
| O componentă existentă trece verificările | Reutilizare prin contractele noii platforme |
| O componentă nu este potrivită | Înlocuire incrementală; retragere numai după validarea alternativei și aprobarea acțiunii |

Nu presupunem rescriere totală, compatibilitate totală sau migrare automată universală. Nu sincronizăm două sisteme ca surse principale concurente fără reguli de proprietate și reconciliere.

Cutover-ul, conversiile ireversibile și retragerea infrastructurii sunt acțiuni distincte, care cer autorizare explicită, backup și probe de recuperare. Planificarea lor nu autorizează executarea.

## 10. Calitate, securitate și cost: reguli transversale

Fiecare livrabil trebuie să includă, după relevanță:

- teste de domeniu și contract, plus acceptare funcțională definită independent de generator;
- teste negative între companii și pentru drepturi revocate sau delegare invalidă;
- verificarea aprobării asupra datelor și efectului concret executat;
- concurență, duplicate, retry, restart, date întârziate și indisponibilitatea dependențelor;
- verificări de securitate pentru cod, dependențe, secrete, date și extensii;
- migrări, compatibilitate și restaurare, nu doar un buton de rollback al codului;
- observabilitate, consum și responsabil pentru intervenții;
- documentarea limitelor, a licenței și a modului de export/deconectare.

Scenariile de detaliu sunt dezvoltate în S16 folosind [matricea din analiza de direcție](../ANALIZA_DIRECTIE_AI_APPS.md#152-scenarii-obligatorii-de-verificare). Testarea este continuă, nu exclusiv o condiție a M5.

### Indicatori de program

- rezultate utile și muncă economisită pe procesul pilot, comparate cu modul actual de lucru;
- cost total pe client, modificare acceptată și sarcină AI finalizată corect;
- intervenții umane, incidente, suport și restanțe de reconciliere;
- reutilizare și compatibilitate la upgrade, nu doar numărul modulelor generate;
- adoptare, disponibilitate de plată și retenție;
- performanță și recuperare pe profilul de încărcare stabilit.

Pragurile sunt stabilite în S01/S04/S16 înainte de acceptarea pilotului. Nu declarăm arbitrar capacitate, SLA sau marjă pozitivă fără măsurători și asumarea costurilor de operare.

## 11. Guvernanța planului și a deciziilor

Rolurile necesare sunt responsabilități, nu un număr obligatoriu de angajați: responsabil de produs, responsabil tehnic, responsabil de securitate/date, responsabil de operare și responsabil comercial. Aceeași persoană poate cumula roluri, dar limitele și verificările trebuie asumate explicit.

### Cum înregistrăm o decizie

O decizie transversală primește un ID `RC-nnn`; o decizie locală unei secțiuni poate folosi `Sxx-Dnn`. Se păstrează problema, alternativele, recomandarea, motivul, dovezile, impactul, aprobarea și condițiile de reevaluare.

| Decizie inițială de lucru | Statut |
|---|---|
| Whitepaper-ul nu impune scope-ul reconstrucției | Mandat exprimat de inițiator; obligațiile reale existente rămân de verificat |
| Aplicații native, ecosistem deschis, personalizări reutilizabile și angajați AI cu mandat | Principii asumate în discuția de direcție |
| Cele 16 secțiuni, etapele M0–M5 și recomandările de simplificare | Propunere a acestui plan general, pentru validare |
| Framework, schemă fizică de date, motoare și furnizori | De evaluat în secțiunile responsabile |
| Primul proces pilot și prima ofertă | De decis în S01 |

Înainte de schimbarea unei decizii verificăm impactul asupra contractelor, datelor, clienților, costurilor și secțiunilor dependente. O modificare a unui prompt nu poate schimba implicit politica de acces sau oferta comercială.

Nu folosim un procent global de tip „platforma este 85% gata”. Progresul se urmărește prin documente aprobate, livrabile implementate, probe trecute și funcții efectiv operate, cu limitele lor.

## 12. Riscuri de program și condiții de oprire

| Risc | Semnal de avertizare | Acțiune |
|---|---|---|
| Scope prea larg | Nu putem livra un proces complet fără a construi aproape toate secțiunile | Reducerea pilotului și a interfețelor inițiale |
| Framework-ul devine produsul principal | Multă infrastructură de generare, puțină muncă reală rezolvată | Revenire la procesul nativ și extragerea componentelor demonstrat comune |
| Personalizări care nu se actualizează | Fiecare client necesită fork și remedieri separate | Reproiectarea extensiilor înainte de catalog public |
| Agent cu acces excesiv | Are nevoie de cont administrator sau de SQL/shell arbitrar pentru activitatea obișnuită | Redefinirea mandatului și capabilităților |
| Date fără autoritate clară | CRM și facturarea externă se suprascriu reciproc | Oprirea sincronizării afectate și clarificarea proprietății datelor |
| Efecte externe duplicate | Retry sau schimbarea furnizorului poate dubla facturi ori mesaje | Tratarea rezultatului necunoscut și reconciliere înainte de reluare |
| Economie nesustenabilă | Costurile de suport, AI sau comunicare depășesc oferta validată | Restrângerea scope-ului, limitelor și revizuirea prețului |
| Trust doar declarativ | Dovezile depind exclusiv de afirmațiile operatorului, deși se promite verificare independentă | Îmbunătățirea dovezii sau limitarea explicită a promisiunii |
| Lipsă de responsabilitate operațională | Există deploy, dar nu cine intervine și recuperează datele | Nu se extinde lansarea până la asumarea operării |
| Migrare nesigură | Nu există export validat, reconciliere sau plan de recuperare | Nu se face cutover și nu se retrage sistemul vechi |

Planul reduce riscul de blocaj prin probe timpurii și decizii reversibile; nu garantează absența blocajelor. Riscurile nerezolvate se păstrează vizibile, cu responsabil și condiție de rezolvare.

## 13. Următorul pas: S01, apoi S02

Primul plan detaliat recomandat este **S01 — Produs, economie și pilot**. Înainte de a decide multe componente tehnice, trebuie să clarificăm:

1. Există clienți activi și date de producție care trebuie păstrate?
2. Ce categorie de firme putem implica direct în validare?
3. Ce proces zilnic merită rezolvat primul și cum funcționează astăzi?
4. Ce aplicații externe trebuie păstrate, inclusiv rolul concret al FGO?
5. Ce acțiuni ar trebui să execute primul angajat AI și ce rămâne la om?
6. Ce personalizare probabilă ar putea fi reutilizată de un al doilea client?
7. Cine construiește, verifică, operează și oferă suport?
8. Ce costuri, constrângeri de date și niveluri de serviciu putem susține?

Rezultatul S01 va fi un pilot delimitat, criterii de succes și excluderi explicite. S02 îl transformă apoi în contracte și granițe arhitecturale. Planurile detaliate nu sunt create sau considerate aprobate prin publicarea acestui plan general.
