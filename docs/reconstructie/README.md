# BiziX — Plan general de reconstrucție a platformei

> **Versiune:** 0.10 — desemnare automatizată, transfer și recuperarea responsabilului de acces în S03
> **Data:** 2026-09-15
> **Nivel:** Plan general al programului; planurile detaliate se elaborează separat, pe secțiuni.
> **Stadiu:** Documentare și decizii. Nu reprezintă implementare, audit tehnic sau angajament de calendar.

## 1. Mandat și obiectiv

Reconstrucția pornește de la valoarea pentru client, nu de la obligația de a implementa whitepaper-ul existent. Putem păstra, adapta, înlocui sau elimina componente și promisiuni de produs dacă rezultatul este o platformă mai coerentă, utilă și sustenabilă.

> **Avantajul Bizix nu ar trebui să fie că poate genera software, ci că poate transforma acel software într-un sistem de business coerent, sigur și întreținut în timp.**

Ținta este un spațiu operațional unic pentru firme mici și medii, cu aplicații native, servicii comune, angajați AI și integrare cu platformele externe pe care clientul dorește să le păstreze.

**Facturarea nativă Bizix este soluția principală și implicită a produsului țintă.** FGO și alte platforme sunt integrări complet opționale pentru clienții care au nevoie să păstreze un program existent. Obiectivul este adoptarea în timp a facturării Bizix prin valoarea oferită, nu dependența de FGO.

„Totul într-un singur loc” rămâne o experiență unificată, nu o obligație de migrare imediată. Facturarea Bizix trebuie să funcționeze fără cont FGO. Schimbarea sistemului se face controlat pentru documentele noi, după reconcilierea operațiunilor în curs; facturile istorice își păstrează proveniența.

Planul este complet la nivel de domenii, responsabilități, dependențe și verificări. Nu încearcă să definească anticipat toate funcțiile tuturor industriilor. Detaliile și alegerile tehnologice se fixează după verificarea ipotezelor relevante.

### Relația cu documentele existente

- [Analiza de direcție](../ANALIZA_DIRECTIE_AI_APPS.md) păstrează argumentele și principiile discutate, inclusiv cazul FGO și sursele consultate.
- [Whitepaper-ul](../../README.md) și [roadmap-ul istoric](../IMPLEMENTATION_ROADMAP.md) sunt referințe, nu criterii obligatorii de succes pentru reconstrucție.
- [Documentația tehnică istorică](../TECH_INDEX.md) este punct de pornire pentru inventar, nu dovadă a stării actuale a codului.
- Acest plan nu modifică automat documentele vechi, deployment-urile, contractele comerciale sau datele clienților. Obligațiile existente trebuie verificate separat înainte de schimbarea ofertei sau migrare.

## 2. Cum organizăm această zonă

Fișierele curente sunt:

```text
docs/reconstructie/
├── README.md
├── TEMPLATE_PLAN_SECTIUNE.md
└── sectiuni/
    ├── 01_PRODUS_ECONOMIE_PILOT.md
    ├── 02_ARHITECTURA_CONTRACTE_DATE.md
    ├── 03_IDENTITATE_ACCES_IZOLARE.md
    └── 06_SPATIU_DE_LUCRU_UX.md
```

Acest README este planul general și punctul de intrare. [Template-ul de secțiune](./TEMPLATE_PLAN_SECTIUNE.md) definește formatul comun pentru aprofundare.

[Mini-brief-ul S01](./sectiuni/01_PRODUS_ECONOMIE_PILOT.md) este redactat ca document de lucru, cu repere confirmate și ipoteze separate. [S02](./sectiuni/02_ARHITECTURA_CONTRACTE_DATE.md) este în revizuire după discutarea capitolelor 6 și 9 și detalierea contractelor din capitolul 7: direcțiile și cerințele confirmate sunt separate de propunerile de model, contracte și mecanisme care încă necesită probe. [S03](./sectiuni/03_IDENTITATE_ACCES_IZOLARE.md) delimitează drepturile minime ale fluxului comercial, cu opt decizii confirmate despre operare, autoritate, niveluri de acces, desemnare automatizată, transfer și recuperare, plus propuneri pentru aplicarea lor; nu acoperă încă întregul domeniu al identității și izolării. [S06](./sectiuni/06_SPATIU_DE_LUCRU_UX.md) conține prima hartă UX creată în FigJam, sursele diagramelor și intrările pentru wireframe-uri. S01 și S06 nu sunt încă planuri detaliate complete; S02 și livrabilul delimitat S03 necesită revizuire și probe din implementări. Niciunul dintre aceste livrabile nu reprezintă aprobarea pentru implementare. Celelalte planuri vor fi create gradual în `sectiuni/`, fără documente goale prezentate drept planuri finalizate.

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

**Status la 2026-09-15:** S01 are un mini-brief actualizat, S02 este în revizuire după confirmarea direcțiilor și cerințelor din capitolul 9, S03 are un prim livrabil documentar pentru drepturile minime, iar S06 păstrează prima hartă UX în FigJam și sursele în repository. S03 confirmă drepturile minime, autoritatea responsabilului firmei și cele două niveluri de acces, plus prima desemnare prin verificare automatizată, transferul reciproc și recuperarea prin factori proprii/asistență. Matricea, mecanismul automat de verificare și procedurile de aplicare rămân de validat. Un tenant reprezintă o singură entitate juridică, iar facturarea directă este inclusă în prima versiune, fără a fi încă desenată în hartă. S02 nu este aprobat integral; schemele și mecanismele detaliate rămân propuse, iar implementarea este neverificată. Firma mică de servicii B2B este ipoteza aleasă pentru explorare, nu un profil validat comercial; firma pilot și oferta rămân de stabilit. Celelalte planuri sunt de elaborat, iar wireframe-urile și prototipul interactiv sunt încă necreate. Starea implementărilor vechi este de verificat. Numerotarea reprezintă domenii de lucru, nu o ordine rigidă de implementare integrală.

| ID | Fișier existent sau planificat în `sectiuni/` | Responsabilitate și livrabil principal |
|---|---|---|
| S01 | [01_PRODUS_ECONOMIE_PILOT.md](./sectiuni/01_PRODUS_ECONOMIE_PILOT.md) | Mini-brief redactat: vânzare până la facturare și asistent comercial; ipoteză de client, economie, limite și criterii de valoare |
| S02 | [02_ARHITECTURA_CONTRACTE_DATE.md](./sectiuni/02_ARHITECTURA_CONTRACTE_DATE.md) | În revizuire: direcții și cerințe confirmate; model și contracte propuse pentru fluxul din ofertă și facturare directă, evoluție și criterii de acceptare |
| S03 | [03_IDENTITATE_ACCES_IZOLARE.md](./sectiuni/03_IDENTITATE_ACCES_IZOLARE.md) | În revizuire: drepturi minime, niveluri de acces, desemnare automatizată, transfer și recuperare; dovezile, mecanismele, izolarea și politicile complete rămân de validat |
| S04 | `04_INFRASTRUCTURA_LIVRARE_OPERARE.md` | Medii, CI/CD, artefacte, secrete, deployment, observabilitate, backup, recuperare și cost operațional |
| S05 | `05_CORE_PLATFORM_RUNTIME.md` | Registrul modulelor/capabilităților, instalări, configurări, versiuni, rutare, audit comun și ciclul de viață al aplicațiilor |
| S06 | [06_SPATIU_DE_LUCRU_UX.md](./sectiuni/06_SPATIU_DE_LUCRU_UX.md) | Prima hartă logică UX creată; navigare, Action Center și transparența AI. Wireframe-urile, prototipul și design system-ul urmează |
| S07 | `07_MODULE_BUSINESS_NATIVE.md` | Aplicații native, inclusiv facturarea Bizix: minim funcțional, cerințe fiscale, reguli de domeniu și validare înainte de producție |
| S08 | `08_WORKFLOW_SARCINI_EVENIMENTE.md` | Procese durabile, sarcini pentru oameni/AI, aprobări, outbox, deduplicare, retry, compensare și reconciliere |
| S09 | `09_INTEGRARI_API_CONECTORI.md` | API-uri expuse și consumate, conexiuni per companie și conectori opționali; FGO este un exemplu de integrare, nu o condiție pentru facturarea Bizix |
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
| S06 | Mini-brief-ul S01 pentru harta UX și wireframe-uri; implementarea UI se aliniază ulterior contractelor S02/S03/S05 |
| S07, S08 | S01 și contractele minime S02/S03/S05; se dezvoltă împreună cu implementarea S06 pentru primul proces |
| S09, S10 | Contractele comune și procesele S08; infrastructura și securitatea aferente |
| S11 | Capabilități funcționale S07/S08 și S09/S10 acolo unde mandatul agentului le folosește |
| S12 | Baza stabilă a modulelor, izolarea, artefactele și pipeline-ul S02–S05/S07 |
| S13 | Personalizare verificată S12, drepturi comerciale S01 și registrul S05; S15 înainte de monetizare automată |
| S14 | Model de încredere S02/S03, infrastructură S04 și audit/execuție S05/S08 |
| S15 | Oferta S01, identitate S03 și evenimentele de consum/procesele S05/S08 |
| S16 | Activitate transversală; criterii din S01, probe și acceptare pentru toate domeniile livrate |

Dependențele cer contractele și funcțiile necesare livrabilului curent, nu finalizarea întregii secțiuni. Altfel am bloca primul proces până la construirea întregii platforme.

Ordinea recomandată pentru aprofundare este:

1. Mini-brief S01 — repere confirmate, ipoteza de client, procesul și limitele inițiale.
2. Explorare S06 — hartă globală a experienței și prototip schematic al fluxului pilot; feedback în S01.
3. S02 — granițe, date, acțiuni și interfețe de integrare, rafinate împreună cu S01/S06.
4. S03 + baza S04/S16 — acces, izolare, livrare și probe de siguranță.
5. S05/S06/S07/S08 — implementarea unui proces complet utilizabil manual.
6. S09/S10 — integrare externă și servicii efective.
7. S11 și aplicarea S14 — operare AI și dovezi pentru cazul ales.
8. S12/S13 — personalizare, reutilizare și actualizare comună.
9. Extinderea S15/S16 — oferta operabilă, migrarea și lansarea; elementele lor minime sunt pregătite anterior.

Explorarea UX începe fără a aștepta închiderea exhaustivă a S01 sau implementarea core-ului. Harta acoperă viziunea globală, iar prototipul aprofundează primul flux. Schițele folosesc ipoteze și date fictive; nu constituie validare comercială sau dovadă a funcționării tehnice.

## 7. Etape de livrare și condiții de trecere

Etapele sunt propuneri bazate pe rezultate, nu termene calendaristice. Fiecare produce un flux demonstrabil și un set de probe. Dacă o condiție esențială nu este îndeplinită, restrângem sau reproiectăm înainte de extindere.

### M0 — Produsul pilot și adevărul tehnic

**Secțiuni principale:** S01, S02, explorarea S06 și baza S03/S04/S14/S16.

Livrabile:

- mini-brief cu procesul ales, ipoteza de client și limitele inițiale;
- hartă globală UX și prototip schematic al fluxului ales, cu feedback în brief;
- identificarea unui interlocutor reprezentativ și verificarea problemei și valorii urmărite;
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
- primul segment nativ al fluxului ales: contact → oportunitate → ofertă; facturarea Bizix este ținta principală, cu minimul și etapa de livrare stabilite în S07;
- aceleași reguli implementate în capabilități, nu numai în UI;
- sarcini, evenimente și audit persistente;
- build/deploy repetabil, observabilitate și restaurare de bază verificată;
- execuție manuală fără dependență de modelul AI.

**Poarta de acceptare:** procesul este utilizabil cap-coadă și controalele de acces, consistența și recuperarea funcționează în scenariile definite. Nu este suficient un set de ecrane cu date demonstrative.

### M2 — Facturare și servicii comune, cu integrări opționale

**Secțiuni principale:** S07, S10, extinderea S06/S08/S15 și probe S16; S09 pentru integrările efectiv incluse.

Livrabile:

- validarea facturării native Bizix din ofertă și directe, fără oportunitate/ofertă obligatorie pentru traseul direct și fără dependență de conturi sau apeluri FGO;
- stabilirea în S07 a minimului funcțional, cerințelor fiscale aplicabile și repartizării livrării între etape, înainte de emiterea reală;
- notificări prin serviciile comune, audit, statusuri și evidența consumului;
- dacă un client alege o integrare externă: conectare autorizată, mapări, proveniență, limite și reconciliere;
- FGO poate fi verificat ca exemplu opțional într-un mediu de test autorizat, fără a condiționa utilizarea facturării native;
- reguli de tranziție către Bizix pentru documente noi, cu păstrarea istoricului și evitarea dublării.

**Poarta de acceptare:** facturarea nativă și orice integrare oferită au probele necesare pentru funcțiile declarate. Erorile nu mută automat emiterea între Bizix și un furnizor extern. O capabilitate încă neimplementată sau neverificată fiscal nu este prezentată drept disponibilă în producție. FGO nu este o condiție obligatorie de lansare.

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

[S02 v0.6](./sectiuni/02_ARHITECTURA_CONTRACTE_DATE.md) detaliază aceste teme pentru fluxul comercial și facturarea directă. S02-D01–D12 au confirmări delimitate în registrul RC-001–RC-012: direcție arhitecturală, prioritate de evaluare, cerințe sau scope de produs. PostgreSQL este candidatul principal de evaluat, nu o tehnologie deja validată. Confirmările nu aprobă schemele detaliate, nu închid alegerile tehnice de mai jos și nu confirmă reutilizarea codului existent.

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
| Procesul vânzare până la facturare și primul AI asistent comercial | Alegeri confirmate de inițiator la 2026-09-13, documentate în S01 |
| Firmă mică de servicii B2B pentru prima hartă UX | Ipoteză de lucru aleasă de inițiator, până la validarea cu un interlocutor real |
| Facturare Bizix principală și implicită; FGO complet opțional | Direcție confirmată de inițiator; minimul funcțional și disponibilitatea în producție se stabilesc în S07 |
| Validarea profilului, firma pilot și oferta comercială | Necesită cercetare și probe în S01; nu rezultă automat din alegerea ipotezei UX |

### Confirmări din revizuirea S02 la 2026-09-14

Inițiatorul a confirmat explicit deciziile de mai jos pentru prima versiune și proiectarea ei. Alternativele, motivele, probele și condițiile de reevaluare sunt detaliate în capitolul 9 din S02 v0.3. Confirmarea direcției sau a cerinței nu reprezintă aprobarea integrală a S02, validarea unei implementări ori autorizarea operațiunilor de producție.

| ID general | Decizie și limită | Referință locală și impact |
|---|---|---|
| RC-001 | Un tenant = o singură entitate juridică; utilizatorul poate avea apartenențe separate în mai multe firme | S02-D08; S01/S03/S05/S06/S07; grupurile juridice într-un singur tenant nu intră în prima versiune |
| RC-002 | Prima versiune include facturare din ofertă și directă, fără oportunitate/ofertă fictivă; aceleași validări și aprobări | S02-D09; S01/S06/S07/S08; UX-ul direct este de proiectat, avansurile și facturarea parțială se delimitează în S07 |
| RC-003 | Nucleu modular pentru funcțiile noi; separări fizice justificate prin securitate și probe | S02-D01; S04/S05/S07/S08; fără rescriere automată a microserviciilor existente |
| RC-004 | PostgreSQL este candidatul principal de evaluat pentru datele noi, nu alegerea definitivă | S02-D02; S03/S04; probe de izolare, concurență, migrare și restaurare; fără migrare automată a core-ului MongoDB |
| RC-005 | Aceleași contracte și reguli de business pentru UI, AI și API, cu autorizare per actor | S02-D03; S03/S05/S06/S07/S09/S11; OpenAPI, limbajul și bibliotecile rămân de verificat |
| RC-006 | Aprobarea privește conținutul și efectul concret; schimbările relevante cer reaprobare | S02-D04; S03/S06/S07/S08/S11; reprezentarea și mecanismul amprentei nu sunt încă validate |
| RC-007 | Datele curente nu rescriu documentele istorice; snapshot-urile și proveniența se păstrează | S02-D05; S03/S07/S09/S10/S16; corecțiile și retenția se detaliază separat |
| RC-008 | Upgrade-urile nu schimbă tacit contractele sau operațiunile în curs | S02-D06; S04/S05/S08/S12/S16; compatibilitatea și migrarea se demonstrează prin probe |
| RC-009 | Repetarea aceleiași facturări nu autorizează efecte suplimentare; rezultatul necunoscut cere reconciliere, fără fallback automat | S02-D07; S07/S08/S09; aplicabil ambelor origini, fără promisiune de deduplicare semantică universală |
| RC-010 | Facturarea directă permite client existent sau cumpărător punctual; crearea/actualizarea CRM este explicită și autorizată | S02-D10; S01/S03/S06/S07; datele documentului și profilul reutilizabil au cicluri de viață distincte |
| RC-011 | Omul autorizat poate folosi catalog și linii punctuale; AI-ul rămâne limitat la surse/prețuri autorizate | S02-D11; S01/S03/S06/S07/S11; structura catalogului și limitele negocierii rămân de verificat |
| RC-012 | Ciornele incomplete se pot salva; pregătirea pentru aprobare cere date complete și valide | S02-D12; S01/S06/S07/S08; salvarea nu aprobă, nu emite și nu permite date structurale invalide sau scrieri neautorizate |

### Confirmări din elaborarea S03 la 2026-09-15

Inițiatorul a ales explicit cele opt reguli de mai jos pentru prima versiune: RC-013–RC-015 în S03 v0.1, RC-016/RC-017 în v0.2 și RC-018–RC-020 în v0.3. [S03 v0.3](./sectiuni/03_IDENTITATE_ACCES_IZOLARE.md), capitolul 9, păstrează alternativele, limitele și probele necesare. Confirmările nu aprobă integral matricea de aplicare, nu desemnează persoane reale și nu autorizează modificarea drepturilor într-un sistem activ.

| ID general | Decizie și limită | Referință locală și impact |
|---|---|---|
| RC-013 | Numai aprobatorul poate introduce sau modifica prețuri manuale; editarea ciornei nu reprezintă aprobare și nu modifică implicit catalogul | S03-D01; S02/S06/S07/S11; domeniul, resursele și verificarea provenienței se concretizează în matricea S03 și contractele S07 |
| RC-014 | Aceeași persoană poate pregăti și aproba propria intenție dacă are explicit ambele drepturi; aprobarea rămâne un pas distinct | S03-D02; S02/S06/S08; nu impunem universal două persoane și nu pretindem control independent când identitățile coincid |
| RC-015 | Solicitarea execuției aparține unui om desemnat, prin drept separat acordabil operatorului sau aprobatorului; AI-ul nu solicită execuția în prima versiune | S03-D03; S02/S05/S06/S08/S11; numai după aprobarea validă, cu worker tehnic distinct și reverificare; automatizarea ulterioară cere altă decizie |
| RC-016 | Numai responsabilul firmei desemnat explicit acordă/retrage rolurile de aprobator și solicitant; își poate atribui explicit roluri operaționale, cu audit, fără moștenire automată; administrarea delegată se amână | S03-D07; S02/S05/S06/S16; autoritate în produs, nu calitate juridică presupusă; RC-018–RC-020 detaliază direcțiile pentru desemnare, transfer și recuperare, fără validarea mecanismelor |
| RC-017 | Două niveluri de acces: documente atribuite implicit sau întregul domeniu acordat explicit de responsabil; vizibilitatea și acțiunile se verifică separat | S03-D08; S02/S05/S06/S07/S08/S11; fără transfer de drepturi între acțiuni, domenii sau firme; repartizarea și propagarea pe obiectele procesului necesită contracte și probe |
| RC-018 | Prima desemnare înainte de activarea reală folosește verificare automatizată de identitate și reprezentare; emailul/CUI-ul singure nu acordă autoritate | S03-D09; S02/S04/S05/S09/S16; furnizorul/sursele, dovezile și excepțiile sunt de evaluat; fără rezultat verificabil, activarea este blocată, nu aprobată manual implicit |
| RC-019 | Transfer voluntar cu confirmare reciprocă: titularul inițiază după reautentificare, destinatarul acceptă din contul propriu verificat cu MFA; mutare numai la finalizare sigură | S03-D10; S02/S04/S05/S06/S08; fără eliminarea titularului prin invitație neacceptată, fără copierea rolurilor/aprobărilor comerciale; concurența și revocarea autorității vechi cer probe |
| RC-020 | Recuperare prin factori alternativi/coduri pregătite anterior, cu asistență verificată când lipsesc; înlocuirea titularului indisponibil este distinctă de recuperarea contului | S03-D11; S02/S04/S05/S06/S08/S16; dovezi, notificări, audit și procedură de contestare; nu autorizează impersonare, onboarding manual implicit sau acces comercial permanent pentru suport |

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

## 13. Stadiu curent: mini-brief S01, contracte S02, drepturi minime S03 și harta UX

[Mini-brief-ul S01](./sectiuni/01_PRODUS_ECONOMIE_PILOT.md) consemnează răspunsurile inițiatorului din 2026-09-13:

- Bizix este în dezvoltare internă, fără clienți externi în producție.
- Nu există încă o firmă pilot identificată.
- Primul proces ales este vânzare până la facturare.
- Primul rol AI ales este asistent comercial.
- Firma mică de servicii B2B este ipoteza de lucru aleasă pentru explorarea UX, nevalidată cu un interlocutor real.

[Prima hartă UX este creată în FigJam](https://www.figma.com/board/8eaisIfYFnggwsgt0ME29P). [S06](./sectiuni/06_SPATIU_DE_LUCRU_UX.md) păstrează vederea globală, detaliul fluxului pilot, sursele Mermaid, regulile și zonele candidate pentru wireframe-uri.

Facturarea nativă Bizix este direcția principală. FGO rămâne o integrare complet opțională pentru retenție și adoptare graduală, nu traseul implicit sau ținta finală a produsului. Minimul nativ, oferta, prețurile și bugetele sunt de validat; datele și configurațiile interne rămân de inventariat înainte de schimbări tehnice.

La 2026-09-14, inițiatorul a confirmat un tenant per entitate juridică și includerea facturării directe, alături de traseul din ofertă. [S01 v0.5](./sectiuni/01_PRODUS_ECONOMIE_PILOT.md) consemnează aceste completări. [S06 v0.4](./sectiuni/06_SPATIU_DE_LUCRU_UX.md) precizează diferența dintre scope-ul nou și diagramele v0.2, nemodificate: traseul direct nu este încă desenat.

[S02 v0.6](./sectiuni/02_ARHITECTURA_CONTRACTE_DATE.md) separă direcțiile și cerințele confirmate în RC-001–RC-012 de propunerile de contracte și mecanisme. Capitolul 7 detaliază datele ciornei, pregătirea intenției, aprobarea și execuția, cu exemple JSON, erori și reguli de concurență. La această detaliere au fost confirmate cumpărătorul existent sau punctual, catalogul plus liniile punctuale autorizate și salvarea ciornelor incomplete. Documentul include criterii de acceptare propuse, nu rezultate de teste; schemele executabile și câmpurile fiscale rămân de validat. Codul și deployment-urile nu au fost inspectate în această revizuire.

[S03 v0.3](./sectiuni/03_IDENTITATE_ACCES_IZOLARE.md) consemnează deciziile RC-013–RC-020 din 2026-09-15 și propune aplicarea lor pentru oameni, AI și worker. Pe lângă drepturile și nivelurile de acces, sunt confirmate prima desemnare automatizată, transferul reciproc și recuperarea prin factori proprii/asistență verificată. Livrabilul nu închide întregul S03: dovezile de identitate/reprezentare, mecanismul automat, repartizarea documentelor și procedurile administrative necesită validare. S02 v0.6 actualizează legătura cu politica, fără schimbarea schemelor comerciale propuse în v0.3. Harta S06 nu a fost modificată în această etapă.

Pașii următori:

1. Revizuim harta: rolurile, aprobările, așteptările, erorile și următoarea acțiune a utilizatorului.
2. Stabilim în S03 dovezile de identitate/reprezentare acceptate și evaluăm mecanismul automat pentru RC-018; concretizăm procedurile de transfer/recuperare și repartizarea documentelor. Aliniem contractele comerciale din capitolul 7 din S02 cu politica S03 și regulile fiscale S07 și definim contractele administrative necesare; fixăm schemele executabile și probele S08. Inventariem implementările și evaluăm prioritar PostgreSQL înainte de alegerea tehnică definitivă.
3. Identificăm un interlocutor real pentru verificarea ipotezei și a procesului său actual.
4. Proiectăm intrarea prin facturare directă și pregătim wireframe-uri pentru ea și fluxul vânzare → ofertă → facturare, apoi interacțiunile prototipului.
5. Clarificăm instrumentele firmei, volumele, costurile, responsabilitățile și disponibilitatea de plată.
6. Rafinăm S01 și contractele S02 pe baza observațiilor; detaliem celelalte secțiuni când dependențele lor sunt clare.

Absența unui pilot nu blochează schițele și contractele de explorare, dar nu permite declararea produsului sau a UX-ului ca validate de piață. S01, S02, S03 și S06 sunt documente de lucru, neaprobate pentru implementare; schemele executabile, detaliile politicilor și probele tehnice rămân de elaborat în etapa corespunzătoare. Harta logică este creată; wireframe-urile, prototipul interactiv și designul final nu sunt încă create.
