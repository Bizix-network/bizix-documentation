# S01 — Produs, economie și pilot: mini-brief

> **Versiune:** 0.1 — document de lucru
> **Data:** 2026-09-13
> **Stare:** Mini-brief inițial; S01 nu este încă validat comercial sau aprobat pentru implementare.
> **Legături:** [Plan general](../README.md) · [Formatul de detaliere](../TEMPLATE_PLAN_SECTIUNE.md) · [Analiza de direcție](../../ANALIZA_DIRECTIE_AI_APPS.md)

Acesta este un format scurt de descoperire, nu planul detaliat complet al secțiunii. Separă răspunsurile confirmate de inițiator de ipotezele propuse pentru explorarea produsului și a experienței utilizatorului. Nu confirmă existența unei implementări funcționale.

## 1. Repere confirmate

Răspunsuri ale inițiatorului din 2026-09-13, nu rezultate ale unor interviuri cu clienți:

| Element | Confirmat |
|---|---|
| Stadiul actual Bizix | Doar dezvoltare internă; fără clienți externi în producție |
| Acces la o firmă pilot | Nu există încă un pilot identificat |
| Primul proces ales | Vânzare până la facturare |
| Primul rol AI ales | Asistent comercial |

Absența clienților externi nu înseamnă că putem elimina datele, configurațiile sau rezultatele de dezvoltare interne. Acestea rămân de inventariat înainte de schimbări tehnice.

## 2. Clientul și problema: ipoteză de lucru

**Profil propus, neconfirmat:** o firmă mică de servicii B2B, care gestionează solicitări, pregătește oferte și facturează serviciile acceptate. Exemple de investigat: agenție, consultant sau alt prestator de servicii.

Acest profil permite explorarea fluxului ales fără a introduce de la început stocuri, logistică ori POS. Nu limitează piața viitoare a Bizix și nu fixează încă un sector, dimensiunea firmei sau aplicațiile utilizate.

**Utilizator principal propus:** antreprenorul sau responsabilul comercial. Persoana care verifică oferta și aprobă facturarea poate avea un rol distinct sau poate fi aceeași persoană, în funcție de firmă.

**Problema presupusă, de verificat:** informațiile despre client, ofertă, următoarea acțiune și factură sunt dispersate; apar reintroduceri de date, follow-up-uri omise și dificultăți de urmărire a stadiului unei vânzări.

## 3. Promisiunea de produs

> Dintr-un singur spațiu de lucru, firma urmărește o solicitare comercială până la factura emisă. Asistentul AI pregătește munca, omul păstrează controlul angajamentelor, iar programul extern de facturare poate rămâne în uz.

Valoarea urmărită este reducerea muncii repetitive și a incertitudinii operaționale, nu simpla adăugare a unui chat sau înlocuirea forțată a tuturor aplicațiilor firmei. Beneficiile trebuie măsurate, nu presupuse ca demonstrate.

## 4. Fluxul de referință

```text
Contact / solicitare
    → Oportunitate și următoarea acțiune
    → Ofertă în ciornă
    → Verificare și trimitere aprobată
    → Acceptare consemnată
    → Intenție de facturare și aprobare
    → Emitere în sistemul de facturare ales
    → Confirmare sau reconciliere + istoric în CRM
```

Acceptarea ofertei se consemnează pe baza unei confirmări reale, nu este dedusă liber de AI. Fluxul trebuie să poată fi executat manual; AI-ul operează aceleași capabilități și reguli.

**FGO este un candidat pentru simulare și integrare, nu un furnizor ales definitiv și nici un sistem confirmat al unei firme pilot.** Alegerea se verifică după identificarea clientului. Pentru explorarea UX folosim date fictive; un prototip nu emite facturi și nu contactează clienți reali.

Un timeout la emitere este un rezultat necunoscut, nu permisiune de a crea altă factură sau de a schimba automat furnizorul. Detaliile tehnice și comportamentul conectorului aparțin S08/S09.

## 5. Limitele primei versiuni explorate

**Incluse în ipoteza de produs:**

- spațiul firmei, roluri și acces la informațiile necesare;
- contacte, oportunități, oferte și următoarele acțiuni comerciale;
- ciorne pregătite de AI, verificare umană și Action Center;
- intenție de facturare, legătură cu documentul extern și status vizibil;
- notificări autorizate și istoric comun al activității.

**Excluse din primul pilot:**

- ERP, contabilitate și facturare fiscală native complete;
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

Pentru prima variantă, efectele externe sunt precedate de aprobare explicită. Automatizarea unor acțiuni standard pe baza unui mandat preaprobat poate fi evaluată ulterior. Bugetele și limitele sunt aplicate de sistem, nu doar de instrucțiunile modelului.

## 7. Economia: ipoteze, nu prețuri stabilite

**Model comercial de explorat:** abonament pentru spațiul firmei și capabilitățile incluse, cu limite de consum explicite. Personalizările, extensiile și consumul suplimentar pot avea un regim separat. Modelul și disponibilitatea de plată nu sunt încă validate.

Costurile de urmărit: infrastructură, stocare, modele AI, comunicare, integrare, verificare umană, suport și mentenanță. Abonamentele externe plătite direct de client se disting de costurile suportate de Bizix.

Nu stabilim încă un preț, o marjă, un volum de clienți sau un buget AI numeric. Înainte de ofertarea pilotului trebuie clarificate volumul operațiunilor, cine operează serviciul, costurile reale și limitele comerciale. Generarea ieftină a codului nu implică mentenanță gratuită.

## 8. Cum verificăm valoarea

| Aspect | Ce urmărim |
|---|---|
| Claritatea UX | Utilizatorul găsește următoarea acțiune și înțelege ce aprobă, unde este factura și ce înseamnă un status în așteptare |
| Efortul operațional | Timp activ și reintroduceri de date pentru același proces, comparate cu modul actual de lucru al firmei pilot |
| Utilitatea AI | Ciorne acceptate, corecții și intervenții necesare; dacă reduce sau doar mută munca omului |
| Controlul procesului | Oportunități fără următoare acțiune, aprobări întârziate și rezultate externe nereconciliate |
| Sustenabilitatea | Cost complet pe proces și pe client, inclusiv suport și mentenanță, raportat la disponibilitatea de plată |

Acum putem verifica schematic coerența fluxului și a ecranelor. Siguranța execuției se demonstrează ulterior prin teste, iar valoarea comercială prin discuții și utilizare reală. Pragurile de acceptare se stabilesc după obținerea unei referințe de lucru, înainte de acceptarea pilotului; nu sunt rezultate deja obținute.

## 9. Primul rezultat vizual propus

Explorarea S06 începe din acest mini-brief, în paralel cu rafinarea S01:

- **Hartă globală:** înregistrare, configurarea firmei, activarea modulelor, conectarea serviciilor, activitate zilnică, AI/aprobări, personalizare și administrare/export.
- **Prototip schematic aprofundat:** numai fluxul vânzare → ofertă → facturare, nu toate aplicațiile platformei.
- **Zone inițiale de explorat:** vedere de ansamblu, contacte/oportunități, detaliul ofertei, Action Center, integrări și documentul/statusul facturii. Nu reprezintă încă o listă definitivă de ecrane.
- **Stări necesare:** fără date, în lucru, refuz, acces interzis, aprobare în așteptare, conexiune indisponibilă și rezultat necunoscut.

Figma + FigJam rămân recomandarea de instrumente, nu o alegere confirmată sau fișiere deja create. Prototipul clarifică comportamentul și așezarea în pagină; designul vizual final și implementarea urmează după feedback. Constatările UX actualizează S01 și contractele din S02.

## 10. Ce rămâne de clarificat

1. Confirmăm sau schimbăm ipoteza firmei de servicii B2B?
2. Ce firmă sau interlocutor reprezentativ putem implica și cum lucrează astăzi?
3. Ce sistem de facturare și alte instrumente folosește efectiv?
4. Care este principala problemă pentru care ar plăti: efort, follow-up-uri, erori sau vizibilitate?
5. Ce volum de operațiuni, responsabilități umane și costuri putem susține?
6. Ce personalizare reală ar fi utilă și unui al doilea client?

Lipsa unui pilot nu blochează explorarea schematică, dar nu permite să declarăm produsul sau UX-ul validate de piață. Următorul pas este confirmarea ipotezei de client și pregătirea hărții UX de lucru, concomitent cu identificarea unui interlocutor real.
