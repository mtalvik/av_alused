# Nädal 6: OSI Mudel - 1. OSA (Füüsiline ja Kanalikiht)

> [!TIP]
> **Video lingid (vaata kodus):**
> - [Physical Layer](https://www.youtube.com/watch?v=AHV54tqhZU0&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=6)
> - [Number Systems](https://www.youtube.com/watch?v=RdoxJsWzFKc&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=7)
> - [Data Link Layer](https://www.youtube.com/watch?v=eK4s5TQm45c&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=8)
> - [Ethernet Switching](https://www.youtube.com/watch?v=KWm7_vsfdE4&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=9)

---

## 🎯 ÕPPEVÄLJUNDID

**Selle tunni lõpuks sa:**

### TEAD:
- ✅ Mis on OSI mudeli Layer 1 (Füüsiline kiht) ja Layer 2 (Kanalikiht)
- ✅ Mis on MAC aadress ja kuidas see erineb IP aadressist
- ✅ Kuidas Ethernet kaader (frame) on üles ehitatud
- ✅ Mis vahe on Hub-l ja Switch-l

### MÕISTAD:
- ✅ Kuidas sinu tehtud kaabel (Lab 6) tegelikult töötab
- ✅ Miks arvutid vajavad kahte erinevat tüüpi aadresse (MAC ja IP)
- ✅ Kuidas switch teab, kuhu andmeid saata
- ✅ Miks MAC aadress töötab ainult kohalikus võrgus

### OSKAD:
- ✅ Lugeda ja analüüsida MAC aadresse
- ✅ Selgitada, kuidas andmed liiguvad kaablis
- ✅ Eristada unicast, broadcast ja multicast liiklust
- ✅ Kirjeldada, kuidas switch õpib MAC aadresse

---

## 📖 LUGU: Kust me tulime?

### Mida me juba teame?

**Nädal 1-3:** Me õppisime, mis on võrk ja kuidas seadmed suhtlevad.

**Nädal 4-5:** Me nägime OSI mudelit (7 kihti) ja õppisime, et andmed liiguvad kihtide kaudu.

**Lab 6 (eelmine tund):** Me EHITASIME füüsilise kaabli:
- Võtsime UTP kaabli
- Koorisime välist kesta
- Sätitsime 8 juhet õigesse järjekorda (T-568B)
- Panime peale RJ-45 pistiku
- Krimpasime kokku
- Testisime testeriga

**AGA... meil jäi üks suur küsimus:**

> 🤔 **"Miks see kaabel töötab? Mis nendes juhtmetes toimub? Kuidas arvuti teab, et kaabli teisel pool on teine arvuti?"**

**Täna me saame vastuse!**

---

## 🧩 OSI Mudeli Meeldetuletus

OSI mudel on nagu tordiviilud - 7 kihti, mis koos moodustavad terve võrgu.

```
    ┌─────────────────────┐
    │ 7. Application      │ ← Brauserid, email
    ├─────────────────────┤
    │ 6. Presentation     │ ← JPEG pildid, HTML
    ├─────────────────────┤
    │ 5. Session          │ ← Ühenduse hoidmine
    ├─────────────────────┤
    │ 4. Transport        │ ← TCP/UDP (kas andmed jõudsid?)
    ├─────────────────────┤
    │ 3. Network          │ ← IP aadressid (192.168.1.1)
    ├─────────────────────┤
    │ 2. Data Link        │ ← MAC aadressid  ← TÄNA!
    ├─────────────────────┤
    │ 1. Physical         │ ← Kaablid, signaalid ← TÄNA!
    └─────────────────────┘
```

**Meeldetrikk:** "**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing"

**Täna me õpime kahte MADALAIMAT kihti** - need, mis tegelevad füüsilise suhtlusega.

---

## Miks on Layer 1 ja 2 koos?

**Lihtsustatud selgitus:**

- **Layer 1** = "Kuidas ma saan heli teie korterisse?" (kaabel, signaali jõud)
- **Layer 2** = "Kellele ma pean selle pakki andma?" (korteri number uksel)

**Võrgu maailmas:**

- **Layer 1** = Elektrilised signaalid kaablis (1-d ja 0-d)
- **Layer 2** = Kes selle saadab, kes võtab vastu (MAC aadress)

**Analoogia postkastidega:**

1. **Postiljon** = Layer 1 (füüsiline liikumine)
   - Ta läheb uksest uksele
   - Ei kontrolli, mis kirjas sees on
   - Lihtsalt kannab pakke

2. **Aadress uksel** = Layer 2 (MAC aadress)
   - "Korter 5B"
   - Postiljon teab, KUHU anda
   - Aga ei tea, KES seal elab (see on IP - Layer 3!)

---

---

## OSI Mudeli Meeldetuletus

OSI mudel (Open Systems Interconnection) on 7-kihiline raamistik, mis selgitab, kuidas andmed liiguvad võrgus:

```
7. Application  ← Rakendused (brauser, email)
6. Presentation ← Andmete vorming (JPEG, HTML)
5. Session      ← Seansside haldamine
4. Transport    ← TCP/UDP (usaldusväärsus)
3. Network      ← IP aadressid, marsruutimine
2. Data Link    ← MAC aadressid, switching  ← TÄNA!
1. Physical     ← Kaablid, signaalid         ← TÄNA!
```

**Meeldetrikk:** "**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing"

---

## 1. FÜÜSILINE KIHT (Layer 1)

![Füüsiline kiht](/lectures/images/physical_layer.png)

### Mis on Layer 1?

**Füüsiline kiht = kõik, mida saab KÄEGA PUUDUTADA:**

| Komponent | Kirjeldus | Näide |
|-----------|-----------|-------|
| **Kaablid** | Füüsiline meedia | UTP Cat5e (mida te tegite!) |
| **Pistikud** | Ühenduspunktid | RJ-45 (mida te krimpiste!) |
| **Signaalid** | Elekter või valgus | 1-d ja 0-d → elektriline pinge |
| **Portid** | Füüsilised pesad | Ethernet port arvutis |
| **Hubid** | Signaali jagajad | Jagab signaali KÕIGILE |

**Layer 1 ülesanne:** Muuta bittid (1 ja 0) füüsilisteks signaalideks ja saata need meedia kaudu.

---

### Kuidas Signaalid Töötavad?

#### Vaskkaablid (UTP)

**Mida te Lab 6-s tegite:**
- 8 juhet (4 paari)
- Keerdunud ümber (vähendab häireid)
- RJ-45 pistik mõlemas otsas

**Kuidas see töötab Layer 1-s:**

```
Arvuti võrgukaart:
1 → 0 → 1 → 1 → 0 → 1  (digitaalsed bitid)
         ↓
Muundatakse elektrilisteks signaalideks:
+5V → 0V → +5V → +5V → 0V → +5V
         ↓
Liiguvad vaskkaabli kaudu
         ↓
Teine arvuti loeb signaale tagasi bittideks
```

**PIN-id, mida kasutati:**
- PIN 1-2: TX (Transmit) - SAATMINE
- PIN 3-6: RX (Receive) - VASTUVÕTT
- *(100 Mbps kasutab ainult 4 juhet 8-st!)*

---

#### Fiiberoptilised Kaablid

![Fiiberoptiline kaabel](/lectures/images/fiber_optic.png)

**Erinevus vaskist:**

| Aspekt | Vaskkaabel | Fiiberkaabel |
|--------|------------|--------------|
| **Signaal** | Elektriline pinge | Valgusimpulsid |
| **Kiirus** | 1 Gbps | 100+ Gbps |
| **Vahemaa** | ~100m | Kilomeetrid |
| **Häired** | EMI mõjutab | Immuunne EMI vastu |
| **Hind** | Odav | Kallis |

**Kuidas töötab:**
- LED või laser saadab valgust
- Valgus peegeldub klaaskiu seest
- Vastuvõtja muudab valguse tagasi bittideks

---

#### Wireless (Juhtmevaba)

**Wi-Fi, Bluetooth:**
- Raadiolained (elektromagnetilised)
- Ei vaja füüsilist kaablit
- Takistused (seinad) nõrgestavad signaali

---

### Võrguseadmed Layer 1-s

#### PASSIIVSED Seadmed (ei vaja elektrit)

**Mida kasutasite Lab 6-s:**

![Liidespaneel](/lectures/images/patchpanel.png)

| Seade | Funktsioon | Kasutus |
|-------|------------|---------|
| **Patch Panel** | Kaablite korraldamine | Serveriruum |
| **Coupler** | Kaablite pikendamine | 2 kaablit → 1 ühendus |
| **Wall Jack** | Seinapistik | RJ-45 seinakarp |

**Oluline:** Need lihtsalt juhivad signaali, EI muuda seda!

---

#### AKTIIVSED Seadmed (vajavad elektrit)

![Jaotur](/lectures/images/hub.png)

| Seade | Layer | Funktsioon | Intelligentsus |
|-------|-------|------------|----------------|
| **Hub** | 1 | Jagab signaali KÕIGILE portidele | ❌ Ei tea, kellele andmed mõeldud |
| **Repeater** | 1 | Võimendab nõrka signaali | ❌ Ei töötle andmeid |
| **Media Converter** | 1 | UTP ↔ Fiber muundamine | ❌ Ainult füüsiline muundamine |

**HUB Probleem:**

```
     [HUB]
    /  |  \
  PC1 PC2 PC3

PC1 saadab → PC2
Aga HUB saadab signaali:
- PC2 ✅
- PC3 ❌ (mitte temale mõeldud!)
```

**Miks see halb?**
- Kõik näevad kõike (turvaoht!)
- Rohkem kokkupõrkeid (collisions)
- Aeglane

**Lahendus:** Kasuta SWITCH-i (Layer 2)!

---

### Füüsilise Kihi Probleemid ja Lahendused

| Probleem | Põhjus | Lahendus |
|----------|--------|----------|
| **Signaal nõrgeneb** | Kaabel liiga pikk (>100m) | Lisa repeater või switch |
| **Häired (EMI)** | Elektromagnetiline mõju | Kasuta FTP/STP kaablit või fiber |
| **Kaabel katki** | Füüsiline kahjustus | Testi kaablitesteriga (Lab 6!) |
| **Vale pistik** | Valesti krimpitud | T-568B standard järgmine |

---

## 2. KANALIKIHT (Layer 2)

![OSI kanalikiht](/lectures/images/datalink.png)

### Mis on Layer 2?

**Kanalikiht = "Kuidas saata andmeid KOHALIKUS võrgus"**

Layer 1 saadab bitte → Layer 2 organiseerib need **kaadriteks** (frames) ja lisab aadressid.

**Layer 2 ülesanded:**
1. 📦 **Framing** - Pakkimine kaadriteks
2. 🏷️ **Addressing** - MAC aadressid
3. ✅ **Error Detection** - Vigade tuvastamine (CRC)
4. 🚦 **Access Control** - Kes saab kaablit kasutada (CSMA/CD)

---

### Layer 2 Kaks Alamkihti

#### 1. LLC (Logical Link Control)

**Ülesanne:** Ühendab kõrgemad kihid (Layer 3 - IP) madalatega (Layer 1 - füüsiline)

- Protokollide tugi (IP, IPX, AppleTalk...)
- Voogude juhtimine
- Vigade parandamine

#### 2. MAC (Media Access Control)

**Ülesanne:** Kontrollib juurdepääsu füüsilisele meediumile

- MAC aadressid
- Switching (kommutaator)
- CSMA/CD (Collision Detection)

![Võrgukaart](/lectures/images/access_media.png)

---

### MAC Aadress

**Mis on MAC aadress?**

```
AA:BB:CC:DD:EE:FF
│    │    └─────── Seadme unikaalne ID (24 bitti)
└────┴──────────── Tootja ID - OUI (24 bitti)
                   (Organizational Unique Identifier)
```

**Kokku:** 48 bitti = 6 baiti = 12 hex numbrit

![MAC-aadressi struktuur](/lectures/images/mac_address_structure.png)

---

#### MAC Aadressi Näited

| MAC Aadress | Tootja | Seade |
|-------------|--------|-------|
| `00:1A:2B:3C:4D:5E` | Dell | Arvuti |
| `00:50:56:XX:XX:XX` | VMware | Virtuaalmasin |
| `B8:27:EB:XX:XX:XX` | Raspberry Pi | IoT seade |
| `FF:FF:FF:FF:FF:FF` | - | **BROADCAST** (kõigile!) |

---

#### MAC vs IP Aadress

| Omadus | MAC Aadress | IP Aadress |
|--------|-------------|------------|
| **Layer** | 2 (Data Link) | 3 (Network) |
| **Tüüp** | Füüsiline, püsiv | Loogiline, muutuv |
| **Ulatus** | Ainult KOHALIK võrk | Globaalne (internet) |
| **Näide** | `AA:BB:CC:DD:EE:FF` | `192.168.1.10` |
| **Määrab** | Tootja (riistvara) | Administrator (tarkvara) |

**Analoogia:**
- **MAC** = Sinu sünnitunnistuse number (ei muutu)
- **IP** = Sinu postiaadress (muutub, kui kolid)

---

### Ethernet Kaader (Frame)

**Kuidas andmed liiguvad Layer 2-s?**

![Etherneti raami struktuur](/lectures/images/ethernet_frame_structure.png)

```
+----------+----------+----------+------+----------+-----+
| Preamble | Dest MAC | Src MAC  | Type |   DATA   | FCS |
|  (8B)    |  (6B)    |  (6B)    | (2B) |(46-1500B)| (4B)|
+----------+----------+----------+------+----------+-----+
```

**Kaadri osad:**

1. **Preamble (8 baiti)**
   - 7 baiti: `10101010...` (sünkroniseerimine)
   - 1 bait SFD: `10101011` ("Start Frame Delimiter" - kaader algab!)

2. **Destination MAC (6 baiti)**
   - Kellele andmed lähevad?

3. **Source MAC (6 baiti)**
   - Kes andmed saadab?

4. **Type/Length (2 baiti)**
   - Mis protokoll? (0x0800 = IPv4, 0x0806 = ARP, 0x86DD = IPv6)

5. **DATA (46-1500 baiti)**
   - Tegelikud andmed (Layer 3 ja kõrgemad kihid)
   - Minimum 46 baiti (kui vähem, lisatakse padding)

6. **FCS - Frame Check Sequence (4 baiti)**
   - **CRC** (Cyclic Redundancy Check)
   - Vigade tuvastamine
   - Kui viga, kaader VISATAKSE ÄRA!

---

### Side Tüübid Layer 2-s

![Side tüübid Ethernetis](/lectures/images/ethernet_communication_types.png)

#### 1. Unicast

**Üks-ühele suhtlus:**

```
Src: AA:AA:AA:AA:AA:AA → Dest: BB:BB:BB:BB:BB:BB

Ainult üks seade võtab vastu!
```

**Kasutus:** Tavaline veebisirvimine, failide saatmine

---

#### 2. Broadcast

**Üks-kõigile suhtlus:**

```
Src: AA:AA:AA:AA:AA:AA → Dest: FF:FF:FF:FF:FF:FF

KÕIK võrgus võtavad vastu!
```

**Kasutus:**
- ARP (Address Resolution Protocol) - "Kes on IP 192.168.1.5?"
- DHCP - "Vajame IP aadressi!"

---

#### 3. Multicast

**Üks-grupile suhtlus:**

```
Src: AA:AA:AA:AA:AA:AA → Dest: 01:00:5E:XX:XX:XX

Ainult grupi liikmed võtavad vastu
```

**Kasutus:**
- IPTV streaming
- Video konverentsid
- Routing protokollid

---

## 3. SWITCHING (Kommutaator)

### Hub vs Switch

**Miks Switch on parem kui Hub?**

![Ethernet kommutaatori struktuur](/lectures/images/ethernet_switch_structure.png)

| Aspekt | HUB (Layer 1) | SWITCH (Layer 2) |
|--------|---------------|------------------|
| **Intelligentsus** | ❌ Ei tea MAC aadresse | ✅ Õpib MAC aadresse |
| **Liiklus** | Saadab KÕIGILE | Saadab ainult SIHTKOHTA |
| **Kollisioonid** | Palju | Vähe (või mitte üldse) |
| **Turvalisus** | Madal (kõik näevad kõike) | Kõrge (isoleeritud liiklus) |
| **Kiirus** | Aeglane | Kiire |
| **Hind** | Odav | Kallim |

---

### MAC Aadressi Tabel

**Kuidas switch ÕPIB, kus seadmed on?**

![MAC-aadresside tabel](/lectures/images/mac_address_table.png)

#### Õppimise Protsess

**Alguses tabel TÜHI:**

```
Port | MAC Address | Age
-----|-------------|----
(empty table)
```

---

**Samm 1: PC1 saadab kaadri PC2-le**

```
     [Switch]
    /   |   \
  PC1  PC2  PC3
  .01  .02  .03
```

Kaader:
```
Src MAC: AA:AA:AA:AA:AA:01 (PC1)
Dst MAC: BB:BB:BB:BB:BB:02 (PC2)
```

Switch õpib:
```
Port | MAC Address           | Age
-----|----------------------|----
1    | AA:AA:AA:AA:AA:01    | 0s
```

Switch ei tea, kus on `.02` → **FLOODING** (saadab kõigile portidele va port 1)

---

**Samm 2: PC2 vastab PC1-le**

Kaader:
```
Src MAC: BB:BB:BB:BB:BB:02 (PC2)
Dst MAC: AA:AA:AA:AA:AA:01 (PC1)
```

Switch õpib:
```
Port | MAC Address           | Age
-----|----------------------|----
1    | AA:AA:AA:AA:AA:01    | 5s
2    | BB:BB:BB:BB:BB:02    | 0s  ← UUS!
```

Switch TEAB nüüd mõlemat! Järgmine kord saadab ainult port 1-le.

---

**Samm 3: PC1 → PC2 (uuesti)**

Switch vaatab tabelisse:
- Src: `.01` → port 1 ✅ (teab juba)
- Dst: `.02` → port 2 ✅ (TEAB!)

**Switch saadab AINULT porti 2!** PC3 ei näe midagi. ✅

---

#### MAC Aadressi Tabeli Omadused

- **Dünaamiline:** Õpib automaatselt
- **Aegumine:** Vanad kirjed kustutatakse (tavaliselt 300s = 5 min)
- **Ülekirjutamine:** Kui MAC liigub teise porti, uuendatakse
- **Suurus:** Sõltub switch mudelist (väikesed: 2K, suured: 128K+ aadresse)

---

### Half-Duplex vs Full-Duplex

**Kuidas suhtlemine toimub?**

| Režiim | Kuidas töötab | Kiirus | Kollisioonid | Kasutus |
|--------|---------------|--------|--------------|---------|
| **Half-Duplex** | Üks korraga: SAADA *või* VÕTA VASTU | 1x | Jah (CSMA/CD) | Vanad hubid |
| **Full-Duplex** | Mõlemad korraga: SAADA *ja* VÕTA VASTU | 2x | Ei | Kaasaegsed switchid |

**Analoogia:**
- **Half-Duplex** = Raadiosaatja (üks räägib, teised kuulavad)
- **Full-Duplex** = Telefon (mõlemad saavad korraga rääkida)

**Full-Duplex eelised:**
- 2x kiirem (100 Mbps → efektiivselt 200 Mbps)
- Ei ole kollisioone
- Ei vaja CSMA/CD-d

---

### Auto-MDIX

![Auto-MDIX funktsioon](/lectures/images/auto_mdix_feature.png)

**Mida see tähendab?**

**Vanas maailmas (ilma Auto-MDIX-ta):**
- PC ↔ Switch → Straight-through kaabel
- PC ↔ PC → Crossover kaabel ❌

**Tänapäeval (Auto-MDIX-ga):**
- **IGA** kaabel töötab! ✅
- Seade tuvastab automaatselt ja vahetab TX/RX

**Lab 6 põhjus:** Te õppisite KUIDAS need töötavad, aga praktikas pole crossover enam vaja!

---

## KOKKUVÕTE

### OSI Layer 1 - Füüsiline Kiht

| Komponent | Funktsioon | Näide |
|-----------|------------|-------|
| **Meedia** | Füüsiline tee | UTP kaabel (Lab 6!) |
| **Signaalid** | Bittide kandmine | Elekter, valgus, raadio |
| **Seadmed** | Signaali jagamine/võimendamine | Hub, Repeater |
| **Topoloogia** | Füüsiline paigutus | Star, Bus, Ring |

**Mälu:** "Kõik, mida saab KÄEGA PUUDUTADA"

---

### OSI Layer 2 - Kanalikiht

| Komponent | Funktsioon | Näide |
|-----------|------------|-------|
| **MAC aadress** | Füüsiline aadress | `AA:BB:CC:DD:EE:FF` |
| **Frame** | Andmete pakkimine | Preamble + MAC + DATA + FCS |
| **Switch** | Intelligentne suunamine | MAC aadressi tabel |
| **CRC** | Vigade tuvastamine | FCS (4 baiti) |

**Mälu:** "KELLELE andmed lähevad kohalikus võrgus"

---

### Layer 1 vs Layer 2

| Aspekt | Layer 1 | Layer 2 |
|--------|---------|---------|
| **Töötleb** | Bitte (1 ja 0) | Kaadrid (frames) |
| **Aadressid** | Ei ole | MAC aadressid |
| **Vead** | Ei tuvasta | Tuvastab (CRC) |
| **Seadmed** | Hub, Repeater | Switch, Bridge |
| **Intelligentsus** | Ei tea midagi | Teab MAC aadresse |

---

## Järgmine Tund

**Nädal 7: OSI Layer 3 - Võrgukiht**

Õpime:
- IP aadressid (192.168.1.1)
- Marsruutimine (routing)
- Kuidas andmed liiguvad ERINEVATE võrkude vahel
- Router vs Switch

**Küsimus mõtlemiseks:**
> MAC aadress töötab ainult kohalikus võrgus. Kuidas saame internetti? 🤔

---

## Viited

- [OSI mudel ja TCP/IP komplekt](/lectures/contents/osi_model_and_tcp_ip_suite/README.md)
- [Ethernet LAN-i kommutatsioon](/lectures/contents/ethernet_lan_switching/README.md)
- [Võrgu seadmed](/lectures/contents/network_devices/README.md)
- [Lab 6: Ethernet kaablid](https://github.com/mtalvik/av_alused/blob/main/Labs/lab6_eth_cable.md)
