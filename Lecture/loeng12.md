# VLSM - Variable Length Subnet Mask

## Mis on VLAN? 🏢

Enne kui räägime VLSM-ist, peame mõistma **VLAN-e** (Virtual Local Area Network).

### Probleem Ilma VLAN-ideta

**Traditsiooniline switch:**
```
┌────────────────────────────────────┐
│         Switch (üks võrk)         │
│                                    │
│  Port 1: Raamatupidamine          │
│  Port 2: IT osakond               │  } Kõik näevad
│  Port 3: Müük                     │  } üksteist!
│  Port 4: Külalised                │  } Broadcasting
└────────────────────────────────────┘

Probleem:
- Külalised näevad firma dokumente ❌
- Broadcast liiklus jõuab kõikidesse portidesse ❌
- Turvalisus madal ❌
```

### Lahendus: VLAN-id

**VLAN = Virtuaalne võrk switch-is**

```
┌────────────────────────────────────┐
│         Switch (4 VLAN-i)         │
│                                    │
│  VLAN 10: Raamatupidamine         │ ← Eraldatud
│  VLAN 20: IT osakond              │ ← Eraldatud
│  VLAN 30: Müük                    │ ← Eraldatud
│  VLAN 40: Külalised               │ ← Eraldatud
└────────────────────────────────────┘

Tulemus:
✅ Iga VLAN = eraldi võrk
✅ VLAN 40 ei näe VLAN 10 liiklust
✅ Broadcast ainult oma VLAN-is
✅ Turvalisus parem
```

### VLAN-id ja IP Aadressid

**Oluline:** Iga VLAN vajab oma IP alamvõrku!

```
VLAN 10: 192.168.10.0/24
VLAN 20: 192.168.20.0/24
VLAN 30: 192.168.30.0/24
VLAN 40: 192.168.40.0/24
```

**Aga** klassikalise subnetting-iga kõik alamvõrgud sama suurusega! → **Raiskamine!**

Siin tuleb **VLSM** appi! 🎯

---

## Miks VLSM on vajalik? 🤔

Klassikaline subnetting kasutab sama maskid kõigile alamvõrkudele. See tähendab:

**Probleem:**
- Ettevõttel on `192.168.10.0/24`
- Võtame 4 bitti → `/28` mask
- Saame 16 alamvõrku, igaüks 14 hosti

```
Alamvõrk 1: 192.168.10.0/28    (14 hosti)
Alamvõrk 2: 192.168.10.16/28   (14 hosti)
Alamvõrk 3: 192.168.10.32/28   (14 hosti)
...kõik sama suurusega
```

**Aga tegelikkus:**
- Kontor A vajab 100 hosti → saab 14 hosti ❌
- Router-router link vajab 2 hosti → saab 14 hosti ❌ (12 raisku!)
- DMZ vajab 5 hosti → saab 14 hosti ❌ (9 raisku!)

**Lahendus = VLSM** ✅

VLSM lubab kasutada **erinevaid maske** samas võrgus. See tähendab:
- Suur osakond saab suure alamvõrgu `/25` (126 hosti)
- Väike osakond saab väikese `/28` (14 hosti)  
- Router link saab `/30` (2 hosti)
- Efektiivne = **0 raisku!**

---

## VLSM Põhimõtted

### 1. Subnetting a Subnet

VLSM = "subnetting a subnet" ehk **alamvõrgu alamvõrk**.

**Näide:**

```
Algne võrk: 192.168.0.0/16 (65,534 hosti)
     ↓
Esimene jaotus (/18): 4 suurt alamvõrku
     ↓
Võtame ühe alamvõrgu: 192.168.64.0/18
     ↓
Teine jaotus (/20): Jagame veel 4 osaks
     ↓
192.168.64.0/20  (4,094 hosti)
192.168.80.0/20  (4,094 hosti)
192.168.96.0/20  (4,094 hosti)
192.168.112.0/20 (4,094 hosti)
```

Iga alamvõrku võib **uuesti jagada** vastavalt vajadusele.

### 2. Klassikaline vs VLSM

**Klassikaline subnetting:**
```
192.168.1.0/24 → Võtame 3 bitti
Tulemus: 8 alamvõrku × 30 hosti = KÕIK SAMA SUURUSEGA
```

**VLSM:**
```
192.168.1.0/24 → Jagame vastavalt vajadusele
- Alamvõrk A: /26 (62 hosti)
- Alamvõrk B: /27 (30 hosti)
- Alamvõrk C: /28 (14 hosti)
- Link 1: /30 (2 hosti)
- Link 2: /30 (2 hosti)
= EFEKTIIVNE!
```

---

## VLSM Disaini Protsess

### Samm 1: Kogu Nõuded

Kirjuta üles kõik vajadused:

| Koht | Host vajadus |
|------|--------------|
| HQ LAN | 500 |
| Branch A | 120 |
| Branch B | 60 |
| DMZ | 10 |
| WAN link 1 | 2 |
| WAN link 2 | 2 |

### Samm 2: Sorteeri SUURUSELT (Cisco juhis!)

**ALATI suurimast väikseimani!** See on VLSM põhireegel.

```
1. HQ LAN: 500 hosti
2. Branch A: 120 hosti
3. Branch B: 60 hosti
4. DMZ: 10 hosti
5. WAN link 1: 2 hosti
6. WAN link 2: 2 hosti
```

**Miks see oluline?**
- Kui alustad väikestest → suured ei mahu enam!
- Kui alustad suurtest → väikesed mahuvad alati

### Samm 3: Arvuta Vajalikud Maskid

**Valem:** 2^n - 2 ≥ host vajadus

| Vajadus | Vajalik n | Mask | Hosts |
|---------|-----------|------|-------|
| 500 | 9 biti | /23 | 510 |
| 120 | 7 biti | /25 | 126 |
| 60 | 6 biti | /26 | 62 |
| 10 | 4 biti | /28 | 14 |
| 2 | 2 biti | /30 | 2 |

### Samm 4: Jaota Aadressiruum

**Oluline:** Alamvõrgud peavad olema **contiguous** (järjestikused, ilma aukudeta!)

### Antud: `192.168.10.0/22` baasil

```
1. HQ (500): 192.168.10.0/23
   - Network: 192.168.10.0
   - Broadcast: 192.168.11.255
   - Next available: 192.168.12.0

2. Branch A (120): 192.168.12.0/25
   - Network: 192.168.12.0
   - Broadcast: 192.168.12.127
   - Next available: 192.168.12.128

3. Branch B (60): 192.168.12.128/26
   - Network: 192.168.12.128
   - Broadcast: 192.168.12.191
   - Next available: 192.168.12.192

4. DMZ (10): 192.168.12.192/28
   - Network: 192.168.12.192
   - Broadcast: 192.168.12.207
   - Next available: 192.168.12.208

5. Link 1 (2): 192.168.12.208/30
   - Network: 192.168.12.208
   - Broadcast: 192.168.12.211
   - Next available: 192.168.12.212

6. Link 2 (2): 192.168.12.212/30
   - Network: 192.168.12.212
   - Broadcast: 192.168.12.215
   - Next available: 192.168.12.216
```

---

## /30 Subnet - Point-to-Point Link

Router-router linkidel vajame **ainult 2 IP aadressi**:
- Router A interface
- Router B interface

**Lahendus:** `/30` mask

```
Network bits: 30
Host bits: 2
Võimalikud aadressid: 4
Kasutatavad: 2
```

**Näide:** `192.168.1.0/30`
```
192.168.1.0   → Network address
192.168.1.1   → Router A (first usable)
192.168.1.2   → Router B (last usable)
192.168.1.3   → Broadcast address
```

**Järgmine /30:**
```
192.168.1.4   → Network
192.168.1.5   → Router C
192.168.1.6   → Router D
192.168.1.7   → Broadcast
```

**Shortcut:** Järgmine /30 = eelmine network + 4

---

## VLSM ja Routing Protocol Tugi

**VLSM töötab ainult klassless routing protokollidega!**

✅ **Toetavad VLSM:**
- RIPv2 (mitte RIPv1!)
- EIGRP
- OSPF
- IS-IS
- BGP-4

❌ **EI toeta VLSM:**
- RIPv1 (classful)
- IGRP (classful)

**Miks?**

Classful protokollid ei saada subnet mask infot:
```
RIPv1: "Mul on võrk 192.168.1.0" (ilma maskita!)
RIPv2: "Mul on võrk 192.168.1.0/25" (koos maskiga!) ✓
```

---

## Summarization ja VLSM

VLSM ja summarization käivad käsikäes.

**Summarization** = Route aggregation = mitme alamvõrgu koondamine üheks route'iks

**Näide:**

Teil on alamvõrgud:
```
192.168.32.0/24
192.168.33.0/24
192.168.34.0/24
192.168.35.0/24
```

**Ilma summarization-ita:** 4 route routing table'is

**Summarization-iga:** 1 route:
```
192.168.32.0/22 (hõlmab kõik 4 alamvõrku)
```

**Kuidas arvutada?**

1. Kirjuta aadressid binaarses:
```
192.168.00100000.0 = 192.168.32.0
192.168.00100001.0 = 192.168.33.0
192.168.00100010.0 = 192.168.34.0
192.168.00100011.0 = 192.168.35.0
        ^^^^^^
       2 bitti muutub
```

2. Mask = 24 - 2 = `/22`

3. Summary route = `192.168.32.0/22`

---

## Praktilised Näpunäited

### 1. Address Space Planeerimine

**ALATI jäta kasvuks ruumi!**

Halb:
```
Office: vajab 50 → annan /26 (62 hosti)
→ Peagi vajab 70 → EI MAHU! ❌
```

Hea:
```
Office: vajab 50 → annan /25 (126 hosti)
→ 76 hosti reserve ✓
```

### 2. Dokumendi Kõik!

VLSM nõuab head dokumentatsiooni:

| Subnet | Network | Mask | First | Last | Broadcast | Purpose |
|--------|---------|------|-------|------|-----------|---------|
| HQ | 192.168.1.0 | /23 | .1.1 | .2.254 | .2.255 | Main office |
| DMZ | 192.168.3.0 | /28 | .3.1 | .3.14 | .3.15 | Servers |

### 3. IP Address Management (IPAM)

Suurtes võrkudes kasuta IPAM tarkvara:
- phpIPAM
- NetBox
- Infoblox
- SolarWinds IPAM

### 4. Vältida Overlap

**EI tohi kattuda!**

Vale:
```
Subnet A: 192.168.1.0/25 (0-127)
Subnet B: 192.168.1.64/26 (64-127) ❌ OVERLAP!
```

Õige:
```
Subnet A: 192.168.1.0/25 (0-127)
Subnet B: 192.168.1.128/26 (128-191) ✓
```

---

## Näide: Ettevõtte Võrk

**Olukord:**
- Antud: `192.168.0.0/20`
- Vajadused:
  - HQ: 2000 hosti
  - Tootmine: 500 hosti
  - Arendus: 250 hosti
  - Müük: 100 hosti
  - Admin: 50 hosti
  - DMZ: 20 hosti
  - 4 WAN linki

**Lahendus:**

```
1. HQ (2000): 192.168.0.0/21 (2046 hosti)
   Network: 192.168.0.0
   Broadcast: 192.168.7.255

2. Tootmine (500): 192.168.8.0/23 (510 hosti)
   Network: 192.168.8.0
   Broadcast: 192.168.9.255

3. Arendus (250): 192.168.10.0/24 (254 hosti)
   Network: 192.168.10.0
   Broadcast: 192.168.10.255

4. Müük (100): 192.168.11.0/25 (126 hosti)
   Network: 192.168.11.0
   Broadcast: 192.168.11.127

5. Admin (50): 192.168.11.128/26 (62 hosti)
   Network: 192.168.11.128
   Broadcast: 192.168.11.191

6. DMZ (20): 192.168.11.192/27 (30 hosti)
   Network: 192.168.11.192
   Broadcast: 192.168.11.223

7. Link 1: 192.168.11.224/30
8. Link 2: 192.168.11.228/30
9. Link 3: 192.168.11.232/30
10. Link 4: 192.168.11.236/30
```

**Vaba ruum tulevikuks:** `192.168.11.240/28` ja üles kuni `192.168.15.255`

---

## Kontrollküsimused

1. **Miks VLSM on efektiivsem kui klassikaline subnetting?**

2. **Millises järjekorras VLSM alamvõrgud disaini?**

3. **Milline mask on kõige efektiivsem router-router linkidele?**

4. **Kas RIPv1 toetab VLSM-i? Miks?**

5. **Kuidas kontrollida, kas kaks alamvõrku kattuvad?**

---

## Kokkuvõte

**VLSM võimaldab:**
- ✅ Efektiivne IP aadresside kasutus
- ✅ Erineva suurusega alamvõrgud
- ✅ Paindlik network design
- ✅ Minimeerimine IP waste

**VLSM nõuab:**
- ⚠️ Head planeerimist
- ⚠️ Dokumentatsiooni
- ⚠️ Classless routing protokolle
- ⚠️ Hoolikat address space management

**Reegel:**
> **Alusta suurimast, lõpeta väikseimaga, jäta kasvuks ruumi!**