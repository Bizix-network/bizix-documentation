# Template — Plan detaliat de secțiune Bizix

Acesta este formatul comun pentru planurile din [planul general de reconstrucție](./README.md). Nu este un plan funcțional aprobat și nu indică existența unei implementări.

## Utilizare

- Se creează un plan numai când abordăm secțiunea, folosind ID-ul și numele rezervate în catalogul general.
- Fișierul rezultat se plasează în `sectiuni/`. Se actualizează linkurile relative pentru noua locație; legătura către planul general va fi `../README.md`.
- Se completează informațiile verificate și se marchează explicit ipotezele. Nu se inventează starea codului, costuri, volume, comenzi sau rezultate de teste.
- Pentru un capitol nerelevant se notează „Nu se aplică” și motivul, fără a-l trata ca problemă rezolvată implicit.
- Se documentează livrabilul curent și extensiile prevăzute, fără a încerca proiectarea exhaustivă a întregului domeniu.

---

## Identificare și stare

| Câmp | De completat |
|---|---|
| ID și titlu | Sxx — titlul secțiunii |
| Versiune și data revizuirii | Versiunea documentului, nu versiunea unui produs presupus implementat |
| Stare document | De elaborat / În redactare / În revizuire / Aprobat pentru livrabilul delimitat |
| Stare implementare | Neverificată / Neîncepută / În lucru / Verificată prin probe / Operată |
| Livrabil și etapă generală | Legătura cu M0–M5 și limita concretă a aprobării |
| Responsabil de produs | De desemnat |
| Responsabil tehnic | De desemnat |
| Responsabil de verificare și operare | De desemnat; pot fi persoane sau roluri distincte |
| Secțiuni dependente | ID-uri, contracte și versiuni de referință |

## 1. Obiectiv și valoare

- Problema rezolvată și utilizatorii afectați.
- Rezultatul de business sau operațional urmărit.
- Cum funcționează procesul astăzi și ce dovezi avem.
- Indicatorii de succes, metoda de măsurare și pragurile de stabilit înainte de acceptare.
- De ce această secțiune este necesară pentru livrabilul curent.

## 2. Scope și granițe

| Inclus în livrabilul curent | Exclus sau amânat | Secțiunea responsabilă pentru elementele externe |
|---|---|---|
| De completat | De completat, cu motiv | ID și responsabilitate |

Se delimitează responsabilitatea față de core, UI, module, workflow-uri, identitate, conectori, AI Constructor, AI Operator și servicii comune. Se evită redefinirea regulilor deținute de altă secțiune.

## 3. Starea actuală și reutilizarea

| Element existent | Sursă și data verificării | Stare constatată | Reutilizăm / adaptăm / înlocuim / retragem controlat |
|---|---|---|---|
| De completat | Cod, teste, deployment sau document istoric | Verificat ori ipoteză explicită | Propunere și motiv |

Se separă ce afirmă documentația de ce s-a verificat efectiv. Se identifică utilizatori, date sau obligații existente care ar fi afectate.

## 4. Dependențe și contracte cu alte secțiuni

| Intrare sau ieșire | Secțiune responsabilă | Contract necesar | Condiție înainte de implementare |
|---|---|---|---|
| De completat | Sxx | Date, API, eveniment, politică sau artefact | Ce trebuie stabilit ori verificat |

Dependența privește funcțiile și contractele necesare, nu finalizarea întregii secțiuni. Se identifică schimbările care trebuie reflectate în planul general.

## 5. Scenarii de utilizare

Pentru fiecare scenariu se descriu actorul, compania/mandatul, declanșatorul, datele de intrare, acțiunile, rezultatul și excepțiile.

Se includ, după relevanță:

- folosirea manuală din spațiul de lucru;
- folosirea de către un angajat AI cu mandat limitat;
- execuția prin workflow sau API extern;
- acțiuni concurente, date lipsă ori întârziate;
- refuz, aprobare, revocare, anulare și preluare de către om.

Se arată ce vede utilizatorul când operațiunea este în așteptare, indisponibilă sau are rezultat necunoscut.

## 6. Structură și date

- Componentele logice, responsabilitățile și limitele de execuție.
- Entitățile, identificatorii, relațiile și sursa autoritativă per tip de date.
- Contextul companiei și regulile de izolare.
- Persistența, proveniența, versiunile și snapshot-urile necesare.
- Retenția, ștergerea, exportul și clasificarea datelor.
- Stările proceselor și ce se întâmplă la restart sau la schimbarea versiunii.

Se precizează separat dacă un element necesită un proces, serviciu, bază de date sau repository distinct și de ce. O separare logică nu impune automat una fizică.

## 7. Capabilități, API-uri și evenimente

Pentru fiecare contract relevant se definesc numele, versiunea, schema intrărilor/rezultatelor/erorilor, autorizarea și efectele.

Se descriu:

- capabilitățile consumate și expuse;
- validările de business comune UI-ului, AI-ului și automatizărilor;
- identificatorii de corelare și deduplicare;
- intențiile persistente, retry-ul, timeout-ul și reconcilierea;
- ordinea și redelivery-ul evenimentelor;
- compatibilitatea și deprecarea contractelor.

Pentru un conector extern se adaugă sursele API consultate, autentificarea, mediile, condițiile comerciale, limitele, mapările și ce nu este suportat. Nu se presupun webhook-uri sau capabilități care nu au fost confirmate.

## 8. Securitate, politici și AI

- Actorii și drepturile minime; delegarea și revocarea.
- Cine poate vedea datele, aproba și executa operațiunea.
- Legătura aprobării cu payload-ul și efectul concret.
- Gestionarea secretelor și separarea mediilor.
- Izolarea între companii, inclusiv în cozi, fișiere, cache-uri și memoria AI.
- Protecția față de conținut extern care încearcă să schimbe mandatul agentului.
- Separarea AI Constructor de AI Operator și a extensiilor de codul privilegiat.
- Audit, bugete, limite de execuție și escaladare.
- Cerințe de protecție a datelor, licențiere și conformitate de verificat cu responsabilii potriviți.

Politicile sunt aplicate de sistem, nu numai de prompt. Aprobarea funcțională a antreprenorului nu înlocuiește verificarea tehnică sau evaluarea cerințelor juridice relevante.

## 9. Alternative și decizii

| ID | Problemă și alternative | Recomandare și motiv | Probe necesare | Statut și aprobare |
|---|---|---|---|---|
| Sxx-Dnn sau RC-nnn | De completat | Include costul de operare și ieșirea din soluție | Test, comparație sau informație necesară | Propus / Aprobat / Respins / De reevaluat |

Se documentează compromisurile și condițiile de reevaluare. O tehnologie deja folosită sau populară nu este automat alegerea corectă; nici rescrierea de la zero nu este obiectiv în sine.

## 10. Plan de implementare pentru livrabil

| Pas | Rezultat observabil | Dependențe | Verificare | Responsabil |
|---|---|---|---|---|
| De definit | Un livrabil delimitat, nu doar o activitate | Contracte și funcții necesare | Proba asociată | De desemnat |

Se pornește de la teste și scenarii verificabile, apoi se livrează incremental. Nu se folosesc termene sau estimări de efort fără fundamentul și resursele aferente. Separat se listează extensiile viitoare, fără a le transforma în blocaje artificiale pentru primul livrabil.

## 11. Teste și criterii de acceptare

| Scenariu | Rezultat așteptat | Metodă și mediu | Dovadă necesară |
|---|---|---|---|
| De completat | Criteriu observabil | Test automat, verificare controlată sau pilot autorizat | Rezultat reproductibil, nu afirmație de progres |

Se includ, după relevanță, cazuri normale și negative, traversare între tenants, drepturi revocate, payload modificat după aprobare, duplicate, concurență, restart, indisponibilitate și rezultat extern necunoscut.

Se verifică migrările, restaurarea, compatibilitatea și costul. Testele generate de același AI nu constituie singura verificare a rezultatului său. Comenzile de test/build se completează din proiectele de implementare, nu se inventează în plan.

## 12. Livrare și operare

- Medii de dezvoltare, test și producție; date permise în fiecare.
- Build, artefacte aprobate, deployment și verificări post-livrare.
- Metrici, alerte, limite și intervenția responsabilului operațional.
- Proceduri pentru erori, reconciliere și preluare manuală.
- Backup, restaurare și obiective de recuperare de validat.
- Upgrade, migrări și limitele rollback-ului.
- Consum de infrastructură, modele, transporturi și servicii externe.

O componentă nu este considerată operabilă doar pentru că poate fi pornită sau publicată.

## 13. Personalizare, reutilizare și drepturi

Se precizează ce poate fi configurat, extins prin cod sau distribuit altor clienți. Se separă capabilitățile generice de datele, configurațiile și know-how-ul privat.

Se stabilesc drepturile, compatibilitățile, responsabilul de mentenanță și condițiile de transformare într-un produs reutilizabil. Monetizarea și împărțirea veniturilor se leagă de S01/S13/S15, fără a confunda evidența comercială cu o dovadă de drepturi.

## 14. Migrare și continuitate

- Datele și utilizatorii afectați, dacă există.
- Sursa autoritativă în fiecare etapă și evitarea scrierilor concurente necontrolate.
- Export, mapări, reconciliere și probe înainte de cutover.
- Funcții păstrate temporar în sistemul vechi și criterii de retragere.
- Efecte ireversibile, autorizări necesare și recuperare.
- Exportul soluției și reautorizarea serviciilor externe, dacă sunt relevante.

Descrierea unei operațiuni de producție, migrare sau retragere în plan nu autorizează executarea sa.

## 15. Riscuri și întrebări deschise

| Risc sau necunoscută | Impact | Responsabil | Probă ori răspuns necesar | Blochează ce livrabil? |
|---|---|---|---|---|
| De completat | Business, securitate, date, cost sau operare | De desemnat | Condiție explicită de rezolvare | Etapă și motiv |

Un blocaj nu dispare din plan doar fiindcă este dificil de rezolvat. Se prezintă opțiunile, efectele și decizia necesară. Informațiile care nu sunt necesare livrabilului curent pot rămâne deschise, cu limita declarată.

## 16. Checklist de aprobare

- [ ] Obiectivul, utilizatorii și scope-ul sunt clare.
- [ ] Ipotezele sunt separate de starea verificată.
- [ ] Responsabilitățile și dependențele nu se suprapun contradictoriu.
- [ ] Datele, capabilitățile și drepturile au proprietari și contracte explicite.
- [ ] Efectele externe, excepțiile și rezultatele necunoscute sunt tratate.
- [ ] Există criterii de acceptare, operare, recuperare și cost.
- [ ] Riscurile și întrebările care blochează livrabilul au o decizie sau o condiție de rezolvare.
- [ ] Drepturile de reutilizare, licențele și obligațiile existente sunt tratate unde este relevant.
- [ ] Planul general și secțiunile dependente reflectă deciziile transversale.
- [ ] Aprobarea indică persoana, versiunea documentului și livrabilul delimitat.

Această listă privește aprobarea planului. Acceptarea implementării și autorizarea lansării cer probele și deciziile separate definite în planul general.
