# Loeng 16 - Dünaamiline marsruutimine (Dynamic Routing)

---

## SISUKORD

1. [Sissejuhatus](#1-sissejuhatus)
2. [Miks dünaamiline routing?](#2-miks-dünaamiline-routing)
3. [Kuidas dünaamiline routing töötab?](#3-kuidas-dünaamiline-routing-töötab)
4. [RIP - Routing Information Protocol](#4-rip---routing-information-protocol)
5. [OSPF - Open Shortest Path First](#5-ospf---open-shortest-path-first)
6. [EIGRP - Enhanced Interior Gateway Routing Protocol](#6-eigrp---enhanced-interior-gateway-routing-protocol)
7. [Protokollide võrdlus](#7-protokollide-võrdlus)
8. [Kuidas router valib parima tee?](#8-kuidas-router-valib-parima-tee)
9. [Kokkuvõte](#9-kokkuvõte)

---

## 1. SISSEJUHATUS

### Mida me juba teame?

Eelmistes tundides õppisime staatilise marsruutimise põhitõdesid. Teame, et `ip route` käsuga saab käsitsi lisada marsruute routing table-sse. Teame ka, et iga marsruut peab olema konfigureeritud mõlemas suunas - muidu paketid lähevad kohale, aga vastused ei tule tagasi.

Staatiline marsruutimine töötab hästi väikestes võrkudes. Aga mis juhtub, kui võrk kasvab suuremaks?

### Mida me täna õpime?

Selles tunnis õpime dünaamilist marsruutimist - meetodit, kus routerid õpivad marsruute automaatselt üksteiselt. Tutvume kolme protokolliga: RIP, OSPF ja EIGRP. Õpime, kuidas nad töötavad, mille poolest erinevad ja millal millist kasutada.

---

## 2. MIKS DÜNAAMILINE ROUTING?

### Staatilise routing'u piirangud

Kujuta ette, et töötad suures ettevõttes, kus on 50 kontorit üle Eesti. Igas kontoris on router ja mitu võrku. Kokku on võrgus 50 routerit ja 200 erinevat alamvõrku.

Kui kasutaksid ainult staatilist marsruutimist, peaksid:
- Igale routerile lisama ~200 staatlist marsruuti
- Kokku konfigureerima 50 × 200 = **10 000 marsruuti**
- Iga kord kui lisandub uus võrk, käsitsi uuendama KÕIKI 50 routerit

See on õudusunenägu. Ja mis juhtub, kui üks kaabel läheb katki? Keegi peab käsitsi kõik marsruudid ümber tegema, et liiklus läheks teist teed. Selle ajani on võrk maas.

### Dünaamilise routing'u eelised

Dünaamiline routing lahendab need probleemid:

**Automaatne õppimine.** Routerid "räägivad" omavahel ja jagavad infot võrkude kohta. Kui uus võrk lisandub, levib info automaatselt kõigile routeritele.

**Automaatne taastumine.** Kui link läheb katki, leiavad routerid automaatselt uue tee. Administraator ei pea midagi tegema - võrk parandab ennast ise.

**Vähem tööd.** Administraator ei pea käsitsi sadu marsruute lisama. Ta lihtsalt lülitab protokolli sisse ja routerid teevad ülejäänu ise.

### Elunäide: GPS navigatsioon

Mõtle GPS navigatsioonile autos. 

**Staatiline marsruutimine** oleks nagu paberkaart - sa valid tee enne sõitu ja järgid seda, ükskõik mis juhtub. Kui tee on kinni, oled hädas.

**Dünaamiline marsruutimine** on nagu Waze või Google Maps - see jälgib liiklust reaalajas ja soovitab uue tee, kui ees on ummik. Tee muutub automaatselt vastavalt olukorrale.

---

## 3. KUIDAS DÜNAAMILINE ROUTING TÖÖTAB?

### Põhiprotsess

Dünaamilise routing'u puhul toimub pidev suhtlus routerite vahel:

1. **Naabrite leidmine.** Router saadab "Hello" sõnumeid, et leida teisi routereid võrgus.

2. **Info jagamine.** Routerid jagavad üksteisega infot võrkude kohta, mida nad teavad.

3. **Parima tee arvutamine.** Iga router arvutab saadud info põhjal parima tee igasse võrku.

4. **Routing table uuendamine.** Parimad teed lisatakse routing table-sse.

5. **Pidev jälgimine.** Routerid jälgivad pidevalt, kas naabrid on elus ja kas võrgus on muutusi.

<!-- PLACEHOLDER: pilt dünaamilise routing'u protsessist -->

### Routing protokolli ülesanded

Iga dünaamiline routing protokoll peab lahendama kolm põhiküsimust:

**1. Kuidas leida naabrid?**
Router peab teadma, kellega ta saab infot jagada. Selleks saadab ta perioodiliselt "Hello" pakette.

**2. Kuidas jagada infot?**
Millist infot jagada ja kui tihti? Kas saata kogu routing table või ainult muutused?

**3. Kuidas valida parim tee?**
Kui samasse sihtkohta on mitu teed, millist eelistada? Siin tulevad mängu metric ja algorithm.

### Metric - tee "hind"

Metric on number, mis näitab, kui "kallis" on tee sihtkohta. Madalam metric = parem tee.

Erinevad protokollid kasutavad erinevaid metric-eid:
- **RIP** loeb hop-e (mitu routerit on teel)
- **OSPF** arvutab cost-i (põhineb lingi kiirusel)
- **EIGRP** kasutab keerulist valemit (bandwidth + delay)

---

## 4. RIP - ROUTING INFORMATION PROTOCOL

### Mis on RIP?

RIP (Routing Information Protocol) on kõige vanem ja lihtsam dünaamiline routing protokoll. See loodi 1980ndatel ja on siiani kasutusel, kuigi peamiselt õppetöös ja väga väikestes võrkudes.

### Kuidas RIP töötab?

RIP kasutab väga lihtsat loogikat:

**1. Hop count.** RIP loeb, mitu routerit (hop-i) on teel sihtkohta. Iga router teel on üks hop.

**2. Parima tee valik.** RIP valib alati tee, kus on VÄHEM hop-e - isegi kui see tee on tegelikult aeglasem.

**3. Perioodiline uuendus.** RIP saadab kogu oma routing table naabritele iga 30 sekundi tagant.

### RIP näide

Vaatame lihtsat näidet:

```
[Võrk A] ----[R1]----[R2]----[R3]---- [Võrk B]
              |                          
              +--------[R4]-------------+
```

R1-l on kaks teed võrku B:
- Tee 1: R1 → R2 → R3 → Võrk B (3 hop-i)
- Tee 2: R1 → R4 → Võrk B (2 hop-i)

RIP valib Tee 2, sest seal on vähem hop-e.

**Aga mis siis, kui:**
- Tee 1 on 1 Gbps kiudoptika?
- Tee 2 on 10 Mbps vana kaabel?

RIP valib IKKA Tee 2! See on RIP-i suurim puudus - ta ei arvesta lingi kiirust.

### RIP piirangud

**Maksimaalselt 15 hop-i.** Kui sihtkoht on rohkem kui 15 routeri kaugusel, loetakse see "kättesaamatuks" (unreachable). See teeb RIP-i sobimatuks suurtes võrkudes.

**Aeglane konvergents.** Kui võrgus midagi muutub, võtab RIP-il minuteid, et kõik routerid saaksid uue info. Selle aja jooksul võivad paketid kaduda.

**Suur ribalaiuse kasutus.** RIP saadab kogu routing table iga 30 sekundi tagant, isegi kui midagi pole muutunud. Suurtes võrkudes tekitab see palju liiklust.

### RIP käsud

RIP-i konfigureerimine on väga lihtne:

```
Router(config)# router rip
Router(config-router)# version 2
Router(config-router)# network 192.168.1.0
Router(config-router)# network 10.0.0.0
```

**Mida need käsud teevad:**
- `router rip` - lülita RIP sisse
- `version 2` - kasuta RIPv2 (parem kui vana v1)
- `network X.X.X.X` - reklaami seda võrku naabertele

### Millal kasutada RIP-i?

RIP sobib:
- Väga väikestes võrkudes (alla 15 routeri)
- Õppetööks (lihtne seadistada ja mõista)
- Ajutisteks lahendusteks

RIP EI sobi:
- Suurtes võrkudes
- Kui vajad kiiret taastumist
- Kui on erinevate kiirustega lingid

---

## 5. OSPF - OPEN SHORTEST PATH FIRST

### Mis on OSPF?

OSPF (Open Shortest Path First) on tänapäeval kõige levinum routing protokoll ettevõtete võrkudes. See on "avatud standard" - töötab kõigi tootjate seadmetega (Cisco, Juniper, Huawei jne).

### Kuidas OSPF töötab?

OSPF kasutab täiesti teistsugust lähenemist kui RIP:

**1. Link-state andmebaas.** Iga router kogub infot KOGU võrgu topoloogia kohta. Ta teab kõiki routereid, kõiki linke ja nende olekuid.

**2. SPF algoritm.** Router kasutab Dijkstra algoritmi, et arvutada lühim tee igasse võrku. See on sama algoritm, mida kasutab Google Maps!

**3. Cost-põhine metric.** OSPF arvutab tee "hinna" (cost) lingi kiiruse põhjal. Kiirem link = madalam cost = eelistatum tee.

### OSPF cost arvutamine

OSPF arvutab cost-i valemiga:

```
Cost = 100,000,000 / bandwidth (bps)
```

Näited:
| Lingi kiirus | Cost |
|--------------|------|
| 10 Mbps | 10 |
| 100 Mbps | 1 |
| 1 Gbps | 1 (vaikimisi) |
| 10 Gbps | 1 (vaikimisi, tuleb käsitsi muuta) |

Kui on mitu teed, valib OSPF tee, kus on MADALAM kogumaksumus (cost summa).

### OSPF näide

Sama näide mis RIP-i puhul:

```
[Võrk A] ----[R1]----[R2]----[R3]---- [Võrk B]
         1Gbps   1Gbps   1Gbps
              |                          
              +--------[R4]-------------+
                 10Mbps      10Mbps
```

OSPF arvutab:
- Tee 1 cost: 1 + 1 + 1 = **3**
- Tee 2 cost: 10 + 10 = **20**

OSPF valib Tee 1! Kuigi seal on rohkem hop-e, on see tegelikult kiirem.

### OSPF eelised

**Kiire konvergents.** Kui link läheb katki, levib info sekunditega. OSPF reageerib palju kiiremini kui RIP.

**Skaleeruvus.** OSPF töötab hästi ka väga suurtes võrkudes. Pole 15 hop-i piirangut.

**Arvestab lingi kiirust.** Valib tegelikult kiireima tee, mitte lihtsalt lühima.

**Avatud standard.** Töötab kõigi tootjate seadmetega.

### OSPF Areas

Suurtes võrkudes jagatakse OSPF "aladeks" (areas), et vähendada arvutuskoormust:

```
        Area 1              Area 0              Area 2
    [R1]---[R2]         (Backbone)          [R5]---[R6]
                    [ABR1]---[ABR2]
```

- **Area 0** on alati "backbone" (selgroog)
- Teised area-d ühenduvad läbi Area 0
- Iga area sees arvutavad routerid oma teed iseseisvalt

<!-- PLACEHOLDER: pilt OSPF multi-area topoloogiast -->

### OSPF käsud

OSPF konfigureerimine:

```
Router(config)# router ospf 1
Router(config-router)# network 192.168.1.0 0.0.0.255 area 0
Router(config-router)# network 10.0.0.0 0.0.0.3 area 0
```

**Mida need käsud teevad:**
- `router ospf 1` - lülita OSPF sisse (1 on process ID)
- `network X.X.X.X W.W.W.W area Y` - reklaami võrku selles area-s
- `0.0.0.255` on **wildcard mask** (vastupidine subnet maskile)

### Wildcard mask

Wildcard mask on subnet maski "peegelpilt":

| Subnet mask | Wildcard mask |
|-------------|---------------|
| 255.255.255.0 (/24) | 0.0.0.255 |
| 255.255.255.252 (/30) | 0.0.0.3 |
| 255.255.0.0 (/16) | 0.0.255.255 |

Arvutus: 255 - subnet mask oktett = wildcard oktett

---

## 6. EIGRP - ENHANCED INTERIOR GATEWAY ROUTING PROTOCOL

### Mis on EIGRP?

EIGRP (Enhanced Interior Gateway Routing Protocol) on Cisco poolt loodud protokoll. See on "hübriid" - ühendab RIP-i lihtsuse OSPF-i efektiivsusega.

### Kuidas EIGRP töötab?

EIGRP kasutab unikaalset lähenemist:

**1. DUAL algoritm.** EIGRP kasutab DUAL (Diffusing Update Algorithm) algoritmi, mis hoiab meeles ka "backup" teed.

**2. Keerukas metric.** EIGRP arvestab mitut tegurit:
- Bandwidth (ribalaius)
- Delay (viivitus)
- Valikuliselt: Load, Reliability

**3. Successor ja Feasible Successor.** EIGRP teab nii parimat teed (Successor) kui ka varuteed (Feasible Successor).

### EIGRP eelised

**Väga kiire konvergents.** Kui peamine tee läheb katki, saab EIGRP kohe üle minna varutele - sageli alla sekundi. Ta ei pea uuesti arvutama, sest varutee on juba teada!

**Efektiivne ribalaiuse kasutus.** EIGRP saadab ainult muutusi, mitte kogu routing table-t.

**Lihtne konfigureerida.** Lihtsam kui OSPF, ei vaja area planeerimist.

### EIGRP käsud

```
Router(config)# router eigrp 100
Router(config-router)# network 192.168.1.0
Router(config-router)# network 10.0.0.0
```

**Mida need käsud teevad:**
- `router eigrp 100` - lülita EIGRP sisse (100 on AS number)
- `network X.X.X.X` - reklaami seda võrku

**NB!** AS number peab olema SAMA kõigil routeritel, kes peavad omavahel rääkima.

### EIGRP piirang

**Peamiselt Cisco.** Kuigi EIGRP on nüüd osaliselt avatud (RFC 7868), kasutatakse seda praktikas peamiselt Cisco võrkudes. Kui sul on erinevate tootjate seadmed, kasuta OSPF-i.

---

## 7. PROTOKOLLIDE VÕRDLUS

### Võrdlustabel

| Omadus | RIP | OSPF | EIGRP |
|--------|-----|------|-------|
| **Tüüp** | Distance-vector | Link-state | Hybrid |
| **Metric** | Hop count | Cost (bandwidth) | Bandwidth + Delay |
| **Max hop-e** | 15 | Piiramatu | 255 |
| **Konvergents** | Aeglane (minutid) | Kiire (sekundid) | Väga kiire (<1 sek) |
| **Standard** | Avatud | Avatud | Cisco (osaliselt avatud) |
| **Keerukus** | Lihtne | Keskmine | Keskmine |
| **Skaleeruvus** | Väike | Suur | Suur |
| **Kasutus** | Õppetöö, väikesed võrgud | Ettevõtted, ISP-d | Cisco võrgud |

### Millist valida?

**Vali RIP, kui:**
- Õpid routing'ut esimest korda
- Võrk on väga väike (<10 routerit)
- Vajad kiiret ajutist lahendust

**Vali OSPF, kui:**
- Töötad ettevõtte võrgus
- On erinevate tootjate seadmed
- Vajad skaleeruvust ja kiiret konvergentsi

**Vali EIGRP, kui:**
- Kõik seadmed on Cisco
- Vajad väga kiiret konvergentsi
- Ei taha area-dega tegeleda

---

## 8. KUIDAS ROUTER VALIB PARIMA TEE?

### Administrative Distance (AD)

Mis juhtub, kui routing table-s on sama võrgu kohta mitu marsruuti erinevatest allikatest? Näiteks üks staatiline ja üks OSPF-ist õpitud?

Router kasutab **Administrative Distance** (AD) väärtust, et otsustada, millist allikat usaldada.

| Allikas | AD väärtus |
|---------|------------|
| Connected | 0 |
| Static | 1 |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |

**Madalam AD = rohkem usaldatud.**

Näide: Kui sama võrgu kohta on:
- OSPF route (AD=110)
- RIP route (AD=120)

Router valib **OSPF**, sest 110 < 120.

### Metric

Kui mitu marsruuti tulevad SAMAST protokollist, siis router võrdleb **metric**-ut.

Näide: Kaks OSPF marsruuti samasse võrku:
- Tee 1: cost = 10
- Tee 2: cost = 25

Router valib **Tee 1**, sest madalam cost on parem.

### Routing table koodid

Dünaamilised protokollid lisavad routing table-sse oma marsruudid erinevate koodidega:

```
R1# show ip route

C    192.168.1.0/24 is directly connected, G0/0
S    192.168.2.0/24 [1/0] via 10.0.0.2
R    192.168.3.0/24 [120/2] via 10.0.0.3
O    192.168.4.0/24 [110/20] via 10.0.0.4
D    192.168.5.0/24 [90/156160] via 10.0.0.5
```

| Kood | Tähendus |
|------|----------|
| C | Connected (otse ühendatud) |
| S | Static (käsitsi lisatud) |
| R | RIP |
| O | OSPF |
| D | EIGRP |

Nurksulgudes olevad numbrid: `[AD/metric]`

---

## 9. KOKKUVÕTE

### Põhipunktid meelde jätmiseks

1. **Dünaamiline routing** - routerid õpivad marsruute automaatselt üksteiselt

2. **RIP** - lihtne, hop count metric, max 15 hop-i, aeglane

3. **OSPF** - levinuim, cost metric (bandwidth), avatud standard, kiire

4. **EIGRP** - Cisco protokoll, väga kiire konvergents, backup teed

5. **Administrative Distance** - madalam = rohkem usaldatud

6. **Routing table koodid:**
   - C = Connected
   - S = Static
   - R = RIP
   - O = OSPF
   - D = EIGRP

### Käskude kokkuvõte

| Protokoll | Käsud |
|-----------|-------|
| **RIP** | `router rip` → `version 2` → `network X.X.X.X` |
| **OSPF** | `router ospf 1` → `network X.X.X.X W.W.W.W area 0` |
| **EIGRP** | `router eigrp 100` → `network X.X.X.X` |
| **Vaatamine** | `show ip route`, `show ip protocols` |

### Static vs Dynamic - kokkuvõte

| | Static | Dynamic |
|--|--------|---------|
| **Sobib** | Väikesed võrgud, stub networks | Suured võrgud, keerukad topoloogiad |
| **Töö** | Palju käsitsi tööd | Automaatne |
| **Taastumine** | Käsitsi | Automaatne |
| **Kontroll** | Täielik | Vähem kontrolli |

---

## KONTROLLKÜSIMUSED

Vasta järgmistele küsimustele enne praktikumi:

1. Miks on dünaamiline routing parem kui staatiline suurtes võrkudes?

2. Mis on RIP-i suurim puudus?

3. Miks valib OSPF mõnikord pikema tee (rohkem hop-e)?

4. Mis vahe on Administrative Distance ja Metric vahel?

5. Kui routing table-s on sama võrgu kohta OSPF (AD=110) ja EIGRP (AD=90) marsruut, kumba router kasutab?

---

*Õppematerjal põhineb Cisco NetAcad CCNA materjalidel.*
