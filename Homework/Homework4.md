# Kodutöö VLSM

## Esitamine

**Failid:**
1. **Packet Tracer fail:** `Eesnimi_Perekonnanimi_VLSM.pkt`
2. **Print screen 1:** Topoloogia + IP aadressid (Logical workspace)
3. **Print screen 2:** Ping tulemused (mõlemad ruuterid, `show ip interface brief`)

**Tähtaeg:** ___1 nädal____

---

## Stsenaarium

Väike firma vajab võrku mitmes asukohas. Peakontoris töötab 40 inimest, kellel kõigil on arvuti ja telefon võrgus. Filiaalis on 25 töötajat oma arvutitega. Lisaks sellele on filiaalis paigaldatud IoT seadmed (5 andurit ja nutilukud), CCTV turvasüsteem (4 kaamerat) ja HVAC süsteem (4 kontrollerit kütte ja ventilatsiooni jaoks).

Probleem on selles, et firmale on antud ainult üks väike võrk: 10.10.50.0/25, mis tähendab 126 kasutatavat IP aadressi. Sellest võrgust peame looma 6 erinevat alamvõrku, aga nii, et IP aadresse ei läheks raisku. Kui kasutaksime tavalist subnetting'ut, kus kõik alamvõrgud on ühesugused, siis jääks suur osa aadressidest kasutamata.

Lahendus on kasutada VLSM (Variable Length Subnet Mask) meetodit. See tähendab, et suuremad võrgud saavad suurema maski (näiteks /26 kui vaja 40 hosti) ja väiksemad võrgud saavad väiksema maski (näiteks /29 kui vaja ainult 5 hosti). Nii kasutame IP aadresse palju efektiivsemalt.

## Sinu ülesanne

Selles laboris pead planeerima kõik kuus alamvõrku VLSM meetodil, seejärel ehitama võrgu Packet Traceris, konfigureerima mõlemad ruuterid õigete IP aadressidega ning lõpuks testima, et võrk töötab. Esitad valmis Packet Tracer faili koos kahe print-screeniga, mis näitavad sinu tööd.

---

## Topoloogia

```
[S1]----[Ruuter1]----[Ruuter2]----[S2]
           |             |
         40 hosts      25 hosts
                         |
                    IoT: 5 hosts
                   CCTV: 4 hosts
                   HVAC: 4 hosts
```

**NB!** Ruuteri nimede asemel kasuta oma eesnime koos numbriga. Näiteks kui sinu nimi on Maria, siis esimene ruuter on Maria1 ja teine ruuter on Maria2.

---

## Antud andmed

Sinu kasutuses on võrk 10.10.50.0/25, mis annab sulle 126 kasutatavat IP aadressi. Pead looma järgmised kuus alamvõrku:

| Alamvõrk | Hostide arv |
|----------|-------------|
| Ruuter1 LAN | 40 |
| Ruuter2 LAN | 25 |
| IoT LAN | 5 |
| CCTV LAN | 4 |
| HVAC LAN | 4 |
| Ruuter1-Ruuter2 Link | 2 |

Kokku vajad 80 hosti (40+25+5+4+4+2=80). Kuna sul on 126 aadressi, siis ruumi on küll, aga pead planeerima targalt.

---

## Osa 1: Planeeri alamvõrgud

### 1. Sorteeri suuruse järgi ja leia maskid

**Näide täidetud tabelist:**
| Koht | Hosts | Mask | Võimalikke hoste |
|------|-------|------|------------------|
| Office A | 200 | /24 | 254 |
| Office B | 50 | /26 | 62 |

**Sinu tabel (täida see):**
| Koht | Hosts | Mask | Võimalikke hoste |
|------|-------|------|------------------|
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |
| | | | |

---

### 2. Määra alamvõrgud

**Näide täidetud tabelist:**
| Koht | Network | Mask | First usable | Last usable | Broadcast |
|------|---------|------|--------------|-------------|-----------|
| Office A | 172.16.10.0 | /24 | 172.16.10.1 | 172.16.10.254 | 172.16.10.255 |
| Office B | 172.16.11.0 | /26 | 172.16.11.1 | 172.16.11.62 | 172.16.11.63 |

**Sinu tabel (täida see):**
| Koht | Network | Mask | First usable | Last usable | Broadcast |
|------|---------|------|--------------|-------------|-----------|
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |
| | | | | | |

**Järgmine vaba alamvõrk:** _______________________

---

## Osa 2: Router IP aadressid

**Näide täidetud tabelist:**
| Device | Interface | IP Address | Subnet Mask | Description |
|--------|-----------|------------|-------------|-------------|
| R1 | G0/0/0 | 172.16.10.1 | 255.255.255.252 | Link to R2 |
| R1 | G0/0/1 | 172.16.11.1 | 255.255.255.0 | LAN |

**Sinu tabel (täida see):**
| Device | Interface | IP Address | Subnet Mask | Description |
|--------|-----------|------------|-------------|-------------|
| Eesnimi1 | G0/0/0 | | | Link to Eesnimi2 |
| Eesnimi1 | G0/0/1 | | | LAN 40h |
| Eesnimi2 | G0/0/0 | | | Link to Eesnimi1 |
| Eesnimi2 | G0/0/1 | | | LAN 25h |

---

## Osa 3: Packet Tracer Setup

### 3.1 Lisa seadmed

1. 2x Router (2911 või 4221)
2. 2x Switch (2960)
3. Ühenda kaablid vastavalt topoloogiale
4. **Salvesta fail:** `Eesnimi_Perekonnanimi_VLSM.pkt`

### 3.2 Konfigureeri Ruuter1 (Eesnimi1)

Konfigureeri järgmised asjad (leia käsud ise):

1. **Hostname:** Sinu eesnimi + number (nt Maria1)
2. **Blokeeri DNS lookup**
3. **Enable password:** class (encrypted)
4. **Console password:** cisco
5. **VTY password:** cisco (lines 0-4)
6. **Krüpti kõik passwordid**
7. **Banner:** "Unauthorized Access Prohibited"
8. **Interface G0/0/0:**
   - Description: Link to [teise ruuteri nimi]
   - IP address: (sinu tabelist)
   - Aktiveeri interface
9. **Interface G0/0/1:**
   - Description: LAN 40 hosts
   - IP address: (sinu tabelist)
   - Aktiveeri interface
10. **Salvesta config**

### 3.3 Konfigureeri Ruuter2 (Eesnimi2)

Konfigureeri järgmised asjad (leia käsud ise):

1. **Hostname:** Sinu eesnimi + number (nt Maria2)
2. **Blokeeri DNS lookup**
3. **Enable password:** class (encrypted)
4. **Console password:** cisco
5. **VTY password:** cisco (lines 0-4)
6. **Krüpti kõik passwordid**
7. **Banner:** "Unauthorized Access Prohibited"
8. **Interface G0/0/0:**
   - Description: Link to [esimese ruuteri nimi]
   - IP address: (sinu tabelist)
   - Aktiveeri interface
9. **Interface G0/0/1:**
   - Description: LAN 25 hosts
   - IP address: (sinu tabelist)
   - Aktiveeri interface
10. **Salvesta config**

---

## Osa 4: Testimine

### Test 1: Vaata IP aadresse

Mõlemal ruuteril:
```
Router# show ip interface brief
```

**Kas mõlemad interface'id on "up/up"?** ☐ Jah  ☐ Ei

### Test 2: Ping link

```
Eesnimi1# ping [Eesnimi2 G0/0/0 IP]
```

**Kas töötab?** ☐ Jah  ☐ Ei

### Test 3: Ping LAN

```
Eesnimi1# ping [Eesnimi2 G0/0/1 IP]
```

**Kas töötab?** ☐ Jah  ☐ Ei

**Miks Test 3 ei tööta?**

_______________________________________________________


## Osa 5: Print Screenid

### Print Screen 1: Topoloogia

Packet Tracer Logical workspace:
- Näha kõik seadmed
- Näha IP aadressid (kasuta Inspect tool või hoia hiirt seadme peal)
- **Salvesta:** `Eesnimi_Perekonnanimi_Topology.png`

### Print Screen 2: Ping ja Interface'id

CLI aknas näita:
```
Eesnimi1# show ip interface brief
Eesnimi1# ping [Eesnimi2 link IP]
```

Tee sama Eesnimi2 peal:
```
Eesnimi2# show ip interface brief
Eesnimi2# ping [Eesnimi1 link IP]
```

**Salvesta:** `Eesnimi_Perekonnanimi_Tests.png`

---

## Kontrolli enne esitamist

- ☐ .pkt faili nimi: Eesnimi_Perekonnanimi_VLSM.pkt
- ☐ Ruuteri hostnamed: Eesnimi1, Eesnimi2 (mitte BR1/BR2!)
- ☐ Kõik alamvõrgud planeeritud?
- ☐ IP aadressid õiged?
- ☐ Passwordid seatud (class, cisco)?
- ☐ Interface'id konfigureeritud ja aktiivsed?
- ☐ Config salvestatud (copy run start)?
- ☐ Ping töötab link'il?
- ☐ Print screen 1: topoloogia + IP-d
- ☐ Print screen 2: show ip int brief + ping tulemused
- ☐ Kõik 3 faili esitatud (.pkt + 2x .png)

