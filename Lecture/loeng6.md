# Ethernet Kaablid

## Sisukord
1. Võrgukaablite tüübid
2. RJ-45 otsikud
3. Tööriistad
4. Standardid (T-568A vs T-568B)
5. Kaablitüübid (Straight-through vs Crossover)

---

# 1. VÕRGUKAABLITE TÜÜBID

## 1.1 Keerdpaar Kaablid (Twisted Pair)

![Keerdpaar kaabel](/Labs/images/twisted.png)

**Mis on keerdpaar?** Kaabel, mis koosneb 8 vasejuhtmest (4 paari), mis on omavahel keerdunud. Keerdmine vähendab elektromagnetilist häiret ja parandab signaali kvaliteeti.

---

## 1.2 Varjestuse Tüübid

| Tüüp | Varjestus | Hind | Paindlikkus | Kasutus | Märkused |
|------|-----------|------|-------------|---------|----------|
| **UTP** | Puudub | € | ★★★★★ | Kodu, kontor | 90% kõigist paigaldustest. Ei vaja maandust. |
| **FTP (F/UTP)** | Foolium ümber kõigi paaride | €€ | ★★★★☆ | Kontor, serverruum | Vajab maandust. Hea EMI kaitse. |
| **STP (S/UTP)** | Punutud metallist võrk | €€€ | ★★☆☆☆ | Tööstus, kõrge EMI | Jäik, raske paigaldada. Väga hea EMI kaitse. |
| **SFTP (S/FTP)** | Foolium + metallist võrk | €€€€ | ★☆☆☆☆ | Andmekeskused, kriitilised süsteemid | Parim kaitse. Kõige kallim ja jäigem. |

![Varjestuse tüübid](/Labs/images/TP_types.png)

---

## 1.3 Kaablite Kategooriad (Cat)

| Kategooria | Max Kiirus | Sagedus | Max Pikkus | Hind/m | Kasutus | Märkused |
|------------|-----------|---------|------------|--------|---------|----------|
| **Cat 3** | 10 Mbps | 16 MHz | 100m | - | Vananenud | Ainult telefonikaablid. Ei kasutata enam. |
| **Cat 5** | 100 Mbps | 100 MHz | 100m | - | Vananenud | Asendatud Cat 5e-ga. |
| **Cat 5e** ⭐ | 1 Gbps | 100 MHz | 100m | 0.30-0.50€ | Kodu, kontor | **KÕIGE LEVINUM**. Parim valik enamikule. |
| **Cat 6** | 10 Gbps* | 250 MHz | 55m* / 100m | 0.50-0.80€ | Ettevõte, server | *10 Gbps ainult 55m. Paksem, jäigem kui Cat 5e. |
| **Cat 6a** | 10 Gbps | 500 MHz | 100m | 0.80-1.50€ | Andmekeskus | 10 Gbps täispikkuses. Väga jäik. |
| **Cat 7** | 10 Gbps | 600 MHz | 100m | 1.50-3.00€ | Spetsiaalne | Täielikult varjestatud. Harv kasutus. |
| **Cat 8** | 40 Gbps | 2000 MHz | 30m | 3.00-5.00€ | Andmekeskus | Ainult lühikesed vahemaad. Väga kallis. |

![Kategooriate võrdlus](/Labs/images/eth_types.png)

**Soovitus:** 
- Kodu/kontor → **Cat 5e UTP** (odav, piisav)
- Ettevõte → **Cat 6 FTP** (tulevikukindel)
- Andmekeskus → **Cat 6a SFTP** (maksimum jõudlus)

---

## 1.4 Muud Kaablitüübid

### Koaksiaalkaabel

![Koaksiaalkaabel](/Labs/images/coaxial.png)

**Struktuur:** Keskmine vasejuhe + isolatsioon + metallist punutud varjestus + välimine kest.

**Kasutus:** Kaabel-TV, satelliitantennid. Võrkude jaoks **vananenud** (asendatud keerdpaariga).

---

### Valguskaabel (Fiber Optic)

![Valguskaabel](/Labs/images/fiber.png)

**Põhimõte:** Andmed liiguvad valgusimpulsidena läbi klaaskiudude. Täielik immuunsus EMI vastu.

| Tüüp | Südamik | Vahemaa | Kiirus | Kasutus |
|------|---------|---------|--------|---------|
| **Multimode (MM)** | 50-62.5 μm | Kuni 2 km | 100 Mbps - 100 Gbps | Hooned, kampused |
| **Singlemode (SM)** | 8-10 μm | Kuni 100+ km | 1 Gbps - 400+ Gbps | ISP-d, linnadevaheline |

**Otsikud:** LC, SC, ST, MT-RJ

**Eelised:** Väga kiire, pikad vahemaad, EMI immuunne, turvaline.

**Puudused:** Väga kallis, nõuab spetsiaalset oskust, habras.

---

# 2. RJ-45 OTSIKUD

![RJ-45 otsik](/Labs/images/rj45.png)

**RJ-45** = Registered Jack 45. 8-positsiooniline, 8-kontaktiline (8P8C) pistik Ethernet kaablitele.

## 2.1 Otsiku Osad

1. **8 metallkontakti** - Läbivad plastiku ja lõikavad kiudude isolatsiooni
2. **Läbipaistev korpus** - Võimaldab näha kiudude paigutust
3. **8 kanalit** - Hoiavad kiud eraldi
4. **Kaabliklamber** - Kinnitab väliskile, vähendab pinget
5. **Plastlukk** - Klõpsub kinni pesasse

---

## 2.2 Otsikute Tüübid

| Tüüp | Hind | Kasutus | Eelised | Puudused |
|------|------|---------|---------|----------|
| **Standard RJ-45** | 0.10-0.20€ | Cat 5e | Odav, lihtne, kõige levinum | Ainult UTP kaablitele |
| **Cat 6 RJ-45** | 0.30-0.50€ | Cat 6 | Nihkes kontaktid, paksematele kaablitele | Kallim |
| **Varjestatud** | 0.50-1.00€ | FTP/STP | Metallist korpus, EMI kaitse | Vajab maandust, kallim |
| **Pass-Through** | 0.40-0.60€ | Kõik | Lihtsam kiudude sisestamine | Vajab spetsiaalset tangide |
| **Load Bar** | 0.50-0.80€ | Cat 6a | Täpne kiudude paigutus | Aeglasem, rohkem samme |

![Otsikute tüübid](/Labs/images/rj45_connectors.png)

**Soovitus:** Cat 5e jaoks → standard RJ-45. Cat 6 jaoks → Cat 6 RJ-45. Algajale → Pass-through.

---

# 3. TÖÖRIISTAD

## 3.1 Kaablitangid (Crimping Tool)
![Kaablitangid PASS THROUGH](/Labs/images/vana_nuga.jpg)
**Funktsioon:** Tavaline Crimp Tool.

![Kaablitangid PASS THROUGH](/Labs/images/uus_nuga.jpg)

**Funktsioon:** Crimp Tool "Pass Through". Surub metallkontaktid läbi plastiku ja kiudude isolatsiooni, luues elektrilise ühenduse.

### Tangide Tüübid

| Tüüp | Hind | Omadused | Kasutus | Soovitus |
|------|------|----------|---------|----------|
| **Põhiline** | 5-15€ | Lihtne, plast + teras | 1-5 kaablit/aastas | Harv kodukasutus |
| **Keskmine** ⭐ | 20-50€ | Kvaliteetne teras, ergonoomiline | 10-50 kaablit/aastas | **PARIM VALIK** |
| **Professionaalne** | 60-150€ | Ratched mehhanism, vahetatavad pead | 100+ kaablit/aastas | Professionaalid |
| **Pass-Through** | 30-80€ | Krimpimine + kiudude lõikamine | Algajad | Lihtsam õppida |

![Tangide võrdlus](/Labs/images/tangide_vordlus.png)

**Ratched mehhanism:** Ei lase tangisid vabastada enne täielikku krimpimist. Tagab ühtlase kvaliteedi.

---

## 3.2 Kaablitester

![Kaablitester](/Labs/images/tester.png)

**Funktsioon:** Kontrollib kõiki 8 kiudu pidevust ja õiget järjekorda.

| Tüüp | Hind | Funktsioonid | Kasutus |
|------|------|--------------|---------|
| **Põhiline LED** ⭐ | 5-20€ | 8 LED-tuld, pidevus, järjekord | **Kodukasutus - piisav!** |
| **Multifunktsionaalne** | 30-100€ | LED-ekraan, kaabli pikkus, TDR | Professionaalid |
| **Sertifitseerija** | 500-5000€+ | Standardite testimine, raportid | Sertifitseeritud paigaldused |

**Kuidas töötab:** TX (saatja) ja RX (vastuvõtja) ühikud. LED-tulukesed 1-8 peavad süttima järjestikku.

---

## 3.3 Muud Tööriistad

| Tööriist | Hind | Funktsioon | Kohustuslik? |
|----------|------|------------|--------------|
| **Kaablikoorijad** | 5-15€ | Väliskile eemaldamine | JAH (või tangide sees) |
| **Kaabli lõikur** | 5-15€ | Kaabli puhas lõikamine | EI (tangide sees) |
| **Punch-Down Tool** | 10-30€ | Keystone'ide paigaldamine | Ainult Keystone'idele |

![Tööriistade komplekt](https://i.ebayimg.com/images/g/mBYAAOSwSTFmq1Vu/s-l960.jpg)

---

## 3.4 Algaja Komplekt

### Miinimum (30-50€):
- ✅ Kaablitangid (keskmine) - 25€
- ✅ LED tester - 10€
- ✅ 20x RJ-45 otsikut - 3€
- ✅ 5m Cat 5e kaabel - 5€

### Optimaalne (60-80€):
- ✅ Kaablitangid (hea kvaliteet) - 35€
- ✅ Multifunktsionaalne tester - 20€
- ✅ 50x RJ-45 otsikut - 8€
- ✅ Punch-down tool - 10€
- ✅ 10x Keystone pistikupesad - 12€

---

# 4. STANDARDID

## 4.1 T-568A vs T-568B

![T-568A ja T-568B](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fwww.ecloudhub.com%2Fwp-content%2Fuploads%2F2022%2F01%2FETHERNET-PINOUT_2.png&f=1&nofb=1&ipt=c2450588787310d789199e5ce08da18e811166fb5eb339070bd8539cf7ed6ec7)

Kaks standardit kiudude värvijärjekorra jaoks. **Mõlemad töötavad täpselt sama hästi!**

### T-568B (KÕIGE LEVINUM) ⭐

![T-568A/B](https://external-content.duckduckgo.com/iu/?u=https%3A%2F%2Fcdn.shopify.com%2Fs%2Ffiles%2F1%2F2267%2F1961%2Ffiles%2Fconexiones_normas_t568a_t568b_para_cable_de_red_rj45_ferretronica.jpg%3Fv%3D1531825173&f=1&nofb=1&ipt=1d6382daa1221622c1aa12eff993e891c0bf4da6b7fdda46bd35ce54ce66beb8)

**Erinevus T-568B-st:** Oranž ja roheline paarid on vahetatud (PIN 1-2 ↔ 3-6)

---

### Kumb Valida?

| Standard | Levimus | Kasutus | Soovitus |
|----------|---------|---------|----------|
| **T-568B** | 90%+ | USA, kaubandus | **KASUTA SEDA!** |
| T-568A | 10% | Valitsus, vanemad hooned | Ainult kui olemasolev süsteem |

**TÄHTIS:** Mõlemad otsad peavad kasutama **SAMA** standardit!

---

## 4.2 Millised Kiud Tegelikult Töötavad?

![Aktiivsed kiud](https://external-content.duckduckgo.com/iu/?u=http%3A%2F%2Fwww.engineeringradio.us%2Fblog%2Fwp-content%2Fuploads%2F2013%2F02%2FT-1-crossover.jpg&f=1&nofb=1&ipt=887a6e2ec41ae2d1193eee7f69355be7bf9b8021000161e5f07d8ca2072417c3)

**Kuidas see töötab:** PIN-id 1-2 saadavad andmeid (TX+, TX-) ja PIN-id 3-6 võtavad vastu (RX+, RX-) kiirustel kuni 100 Mbps; Gigabit ja kiiremad võrgud kasutavad kõiki 8 kiudu korraga, saates ja võttes vastu samaaegselt kõigis paarides (bi-directional).

| Ethernet Kiirus | Kasutatavad PIN-id | Paarid | Kuidas Töötab |
|-----------------|-------------------|--------|---------------|
| **10 Mbps** (10BASE-T) | 1, 2, 3, 6 | 2 paari | Üks paar saadab (TX), üks võtab vastu (RX) |
| **100 Mbps** (100BASE-TX) | 1, 2, 3, 6 | 2 paari | Üks paar saadab (TX), üks võtab vastu (RX) |
| **1000 Mbps** (1000BASE-T) | 1, 2, 3, 4, 5, 6, 7, 8 | 4 paari | Kõik 4 paari saadavad JA võtavad vastu samaaegselt (bi-directional) |
| **10 Gbps** (10GBASE-T) | 1, 2, 3, 4, 5, 6, 7, 8 | 4 paari | Kõik 4 paari saadavad JA võtavad vastu samaaegselt (bi-directional) |

**PIN-ide funktsioonid:**
- **PIN 1-2:** Oranž paar (TX+/TX- @ 10/100 Mbps, bi-directional @ 1G+)
- **PIN 3-6:** Roheline paar (RX+/RX- @ 10/100 Mbps, bi-directional @ 1G+)
- **PIN 4-5:** Sinine paar (kasutamata @ 10/100 Mbps, bi-directional @ 1G+)
- **PIN 7-8:** Pruun paar (kasutamata @ 10/100 Mbps, bi-directional @ 1G+)

**Märkus:** 10/100 Mbps kasutab ainult 4 kiudu (pooled otsikust tühjad!), aga 1 Gbps+ vajab kõiki 8 kiudu töötamas!

---

# 5. KAABLITÜÜBID

## 5.1 Straight-Through Kaabel (Sirge)

![Straight-through kaabel](/Labs/images/str_wire.png)

**Definitsioon:** Mõlemad otsad kasutavad **SAMA standardit** (T-568B ↔ T-568B või T-568A ↔ T-568A)

**Kasutus (90% juhtudest):**
- PC → Switch
- PC → Hub
- PC → Router
- Router → Switch
- Switch → Modem

**Meeldetrikk:** Erinevad seadmed → Straight-through

---

## 5.2 Crossover Kaabel (Rist)

![Crossover kaabel](/Labs/images/crs_wire.png)

**Definitsioon:** Üks ots T-568A, teine ots T-568B. PIN-id 1,2 ↔ 3,6 vahetatud.

**Kasutus (harv tänapäeval):**
- PC → PC (otseühendus)
- Switch → Switch
- Router → Router
- Hub → Hub

**Tänapäeval:** Enamik seadmeid toetavad **Auto-MDIX** (automaatne tuvastus), seega crossover pole enam vajalik.

**Meeldetrikk:** Sarnased seadmed → Crossover (aga tänapäeval ei ole vaja!)

---

## 5.3 Rollover Kaabel (Cisco Console)

![Rollover kaabel](/Labs/images/rollover.png)

**Definitsioon:** PIN 1 → PIN 8, PIN 2 → PIN 7, jne. Täielikult pööratud järjekord.

**Kasutus:** PC → Cisco ruuter/switch (Console port)

**Märkus:** See EI OLE Ethernet kaabel! See on **seeriakaabel** RJ-45 otsikutega.

---

## 5.4 Kaablitüüpide Võrdlus

| Kaabel | Mõlemad Otsad | Kasutus | Tänapäeval Vajalik? |
|--------|---------------|---------|---------------------|
| **Straight-Through** | T-568B ↔ T-568B | PC ↔ Switch | ✅ JAH (90%) |
| **Crossover** | T-568A ↔ T-568B | PC ↔ PC | ❌ EI (Auto-MDIX) |
| **Rollover** | PIN 1↔8, 2↔7... | Console port | ✅ JAH (Cisco) |

![Kaablite kasutus skeem](/Labs/images/cable-types-usage-diagram.png)