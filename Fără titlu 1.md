# Plan IP + strategie OPNsense — dev migrat în Hetzner

  

**Data:** 2026-07-10 · **Status:** propunere (rețea verificată live) · **Autoritate alocare IP:** OPNsense HA (r-04)  

**Scop:** propune IP-uri concrete pentru VM-urile dev migrate din GTS și strategia de a le fixa/rezerva în **OPNsense** (care azi face alocarea DHCP), fără coliziuni.

  

> Conex: [Plan DNS/IP/trafic](2026-07-10-plan-dns-ip-trafic-migrare.md) · [Plan final migrare](2026-07-10-plan-final-migrare-dev-si-reechilibrare.md)

  

---

  

## 1. Realitatea rețelei Hetzner (verificat live 2026-07-10)

  

Există **două rețele interne** pe noduri (`pvecm` + `ip addr` + tabel ARP de pe pve-hetzner-1):

  

### 1.1. `10.4.0.0/24` — INFRA / management (`vmbr1`)

  

| IP | Element |

|----|---------|

| **10.4.0.1** | CARP **VIP gateway** (OPNsense) — rutează spre GTS `10.1/10.2/10.3` |

| 10.4.0.10 / 10.4.0.11 | opnsense-01 (primary) / opnsense-02 (standby) |

| 10.4.0.20–10.4.0.26 | mgmt PVE noduri 1–7 |

| 10.4.0.50 | pbs-hetzner |

  

### 1.2. `10.4.1.0/24` — INTERNAL / servicii (`vmbr2`) ◄── aici merg VM-urile dev

  

| IP | Element |

|----|---------|

| 10.4.1.1 | CARP VIP gateway INTERNAL (OPNsense) |

| 10.4.1.10–10.4.1.16 | PVE noduri 1–7 (interfața servicii) |

| **10.4.1.20** | ⚠️ deja ocupat (un VM existent — de identificat) |

| 10.4.1.101 / 10.4.1.102 | obs-01 / auth-01 |

| **10.4.1.120–10.4.1.140** | **LIBER** (confirmat: fără intrări ARP) → bloc dev |

  

> Corecție față de planul inițial: **NU** folosi `.101/.102` (ocupate). Blocul `.120–.140` e liber.

  

---

  

## 2. Propunere IP-uri dev migrat (INTERNAL `10.4.1.0/24`)

  

| VM | Nume | IP GTS azi | **IP nou Hetzner** | FQDN intern (Technitium) |

|----|------|-----------|--------------------|--------------------------|

| 208 | docker-internal | 10.1.1.20 | **10.4.1.120** ◄ ancoră | `docker.internal` (+9 CNAME) |

| 202 | gitlab | 10.1.1.30 | **10.4.1.121** ◄ ancoră | `gitlab.internal` (+registry CNAME) |

| 215 | mariadb-internal | 10.1.1.40 | **10.4.1.122** ◄ ancoră | `mysql.internal` |

| 201 | jitsi | 10.1.2.x | **10.4.1.123** (+IP **public** Hetzner) | `meet` (public) |

| 211 | rustdesk | 10.1.2.x | **10.4.1.124** (+IP public dacă e relay) | `rustdesk` (public) |

| 223 | gitlab-build | 10.1.1.x | **10.4.1.125** | — |

| 233 | bt-debug | 10.1.2.35 | **10.4.1.126** | — |

| — | Technitium **secondary** Hetzner | — | **10.4.1.5** | resolver local (§3.3 plan DNS) |

| — | rezervă / viitor | — | 10.4.1.127–10.4.1.140 | spare |

  

**Recomandare adresare:** păstrează convenția „DNS pe `.5`" (ca GTS `10.1.0.5`) → Technitium secondary la `10.4.1.5`. Restul dev grupat compact în `.120+` pentru reguli firewall/QoS ușor de scris (`10.4.1.120/29`).

  

---

  

## 3. Strategia OPNsense — cum fixăm și rezervăm IP-urile

  

OPNsense e azi **autoritatea de alocare** (DHCP pe INTERNAL). Ai două mecanisme; recomandarea e **combinat**:

  

**Decizie finală:** OPNsense HA rămâne serverul DHCP. Technitium `10.4.1.5`

oferă DNS, dar nu preia DHCP. Pe scope-ul INTERNAL, OPNsense distribuie:

  

- gateway: `10.4.1.1`;

- DNS: `10.4.1.5`;

- domeniul/search suffix intern, dacă este folosit;

- lease time redus temporar înainte de cutover, apoi revenit la valoarea normală.

  

### 3.1. Recomandat — Static DHCP mapping (rezervare MAC → IP) în OPNsense

  

**De ce:** OPNsense rămâne single-source-of-truth pentru IP-uri, VM-ul primește IP determinist prin DHCP (config VM simplu, fără editare manuală în guest), zero coliziuni. Ideal pentru majoritatea VM-urilor.

  

Pași (WebGUI OPNsense, pe **primary 10.4.0.10** — se propagă la standby prin HA XMLRPC):

  

1. **Rezervă blocul din pool-ul dinamic** ca să nu-l dea altcuiva:

   `Services → DHCPv4 → [INTERNAL]` → asigură-te că **Range** dinamic **NU** include `.120–.140` (mută pool-ul dinamic sub `.119` sau peste `.140`).

2. **Adaugă static mappings** (`Services → DHCPv4 → [INTERNAL] → jos: "DHCP Static Mappings"` → **+**):

   - `MAC address` = MAC-ul VM-ului (vezi §4)

   - `IP address` = IP-ul propus (ex. `10.4.1.120`)

   - `Hostname` = `docker-internal` (opțional, dar util)

   - `Description` = `dev migrat GTS — docker-internal`

3. **Apply** → verifică propagarea pe standby (`System → High Availability → Status`).

4. În opțiunile scope-ului INTERNAL setează **DNS server `10.4.1.5`**; nu distribui

   un resolver public ca fallback pentru zonele split-horizon.

  

### 3.2. Alternativ / complementar — Static IP în guest pentru ANCORE

  

Pentru cele 3 ancore critice (`docker-internal`, `gitlab`, `mysql.internal`) merită **IP static direct în VM** (netplan/ifcfg), ca serviciile să nu depindă de DHCP la boot (rezistență la o pană OPNsense/DHCP). În acest caz:

- setezi IP static în guest (`10.4.1.120/24`, gw `10.4.1.1`, DNS `10.4.1.5`)

- **totuși** creezi rezervarea în OPNsense (§3.1) → OPNsense știe IP-ul „e luat", nu-l dă la DHCP altcuiva.

  

> Regula: **orice IP static din guest are și rezervare în OPNsense** (SoT unic, fără dubluri).

  

### 3.3. MAC-uri: atribuie MAC nou determinist pe copiile din Hetzner

  

VM-urile de pe GTS au MAC-uri VMware moștenite (`00:0c:29`, `00:50:56`) și Proxmox (`bc:24:11`). La restore în Hetzner, **atribuie MAC nou Proxmox-style determinist** — evită orice risc de MAC duplicat în perioada de pre-sync (când există simultan VM vechi pe GTS + copie nouă în Hetzner) și simplifică rezervarea.

  

Schema propusă: `BC:24:11:04:01:<ultimul_octet_IP>` (`04`=r-04, `01`=INTERNAL):

  

| VM | IP nou | **MAC nou propus** | MAC vechi (GTS) |

|----|--------|--------------------|-----------------|

| docker-internal | 10.4.1.120 | `BC:24:11:04:01:20` | 00:50:56:89:d7:2f |

| gitlab | 10.4.1.121 | `BC:24:11:04:01:21` | 00:0c:29:78:9a:0e |

| mariadb-internal | 10.4.1.122 | `BC:24:11:04:01:22` | 00:50:56:b2:b9:b1 |

| jitsi | 10.4.1.123 | `BC:24:11:04:01:23` | 00:0c:29:a7:09:d1 |

| rustdesk | 10.4.1.124 | `BC:24:11:04:01:24` | BC:24:11:FF:D8:4A |

| gitlab-build | 10.4.1.125 | `BC:24:11:04:01:25` | BC:24:11:7B:1C:DA |

| bt-debug | 10.4.1.126 | `BC:24:11:04:01:26` | BC:24:11:87:AD:7B |

  

Setare MAC în Proxmox la restore (pe nodul Hetzner):

```bash

qm set <vmid> -net0 virtio=BC:24:11:04:01:20,bridge=vmbr2

```

  

---

  

## 4. Ordinea de execuție (IP/OPNsense)

  

| # | Pas | Unde | Când |

|---|-----|------|------|

| 1 | Confirmă range-ul DHCP dinamic INTERNAL + identifică `10.4.1.20` (ce VM e) | OPNsense WebGUI | T-3 zile |

| 2 | Exclude `.120–.140` din pool-ul dinamic | OPNsense DHCPv4 | T-3 zile |

| 3 | Creează 7 static mappings (MAC nou → IP) + rezervă Technitium `.5` | OPNsense | T-2 zile |

| 4 | Distribuie DNS `10.4.1.5` prin scope-ul DHCP INTERNAL și verifică XMLRPC sync | OPNsense HA | T-2 zile |

| 5 | La restore VM în Hetzner: setează MAC-ul nou (`qm set -net0`) + `bridge=vmbr2` | Proxmox Hetzner | weekend |

| 6 | Pentru cele 3 ancore: setează și IP static în guest (opțional dar recomandat) | guest VM | weekend |

| 7 | Boot VM → confirmă adresă, gateway și DNS primite | VM | weekend |

| 8 | Schimbă A records ancoră în Technitium (docker/gitlab/mysql → `.120/.121/.122`) | Technitium primary GTS | weekend |

| 9 | Validare: rezolvare DNS + trafic local (nu pe tunel) | — | duminică |

  

---

  

## 5. Validare

  

```bash

# pe nodul Hetzner: confirma ca VM-ul e pe vmbr2 cu MAC-ul corect

qm config <vmid> | grep net0

  

# in guest: IP-ul primit

ip -4 addr show ; ip route | grep default    # gw trebuie 10.4.1.1

resolvectl status                            # DNS trebuie 10.4.1.5

  

# din obs-01 / alt VM: rezolvare + reachability

ping -c2 10.4.1.120

dig +short @10.4.1.5 docker.internal.workleto.com   # dupa cutover -> 10.4.1.120

  

# fara coliziune IP (arping din 2 surse)

arping -c3 10.4.1.120

```

  

### 5.1. Test DHCP/HA obligatoriu

  

1. Un client de test nou primește IP din range-ul dinamic, gateway `10.4.1.1` și DNS `10.4.1.5`.

2. Nici `.5`, nici `.120–.140` nu pot fi oferite dinamic.

3. Mapping-urile și opțiunile scope-ului sunt prezente pe standby după XMLRPC sync.

4. Într-o fereastră controlată, failover-ul OPNsense păstrează serviciul DHCP și gateway-ul CARP.

5. După revenire, nu există lease-uri duplicate sau conflict ARP.

  

---

  

## 6. Decizii deschise

  

| # | Decizie | Notă |

|---|---------|------|

| 1 | Ce e `10.4.1.20`? | de identificat în OPNsense/Proxmox înainte (posibil un VM deja migrat) |

| 2 | Range DHCP dinamic INTERNAL actual | necesită citit în WebGUI (fără SSH pe OPNsense) — determină unde punem blocul static |

| 3 | Static-in-guest vs pur-DHCP pentru ancore | recomandat static-in-guest pentru docker/gitlab/mysql |

| 4 | Technitium secondary la `10.4.1.5` | confirmă că `.5` e liber (nu apare în ARP — probabil liber) |

| 5 | IP public Hetzner pentru jitsi/rustdesk | separat de adresarea internă (Hetzner Floating/Additional IP) |

  

> **Limitare:** OPNsense (FreeBSD) nu are SSH activ — pool-ul DHCP și confirmarea `10.4.1.20`/`.5` se fac din WebGUI (`10.4.0.10`). Restul (MAC-uri, IP-uri libere) e verificat live pe cluster.