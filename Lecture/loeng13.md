# Loeng 13 - Ruuter ja marsruutimine (Router & Routing)

---

## SISUKORD

1. [Sissejuhatus](#1-sissejuhatus)
2. [Mis on routing?](#2-mis-on-routing)
3. [Routing table ehk marsruutimistabel](#3-routing-table-ehk-marsruutimistabel)
4. [Connected routes](#4-connected-routes)
5. [Static routes](#5-static-routes)
6. [Default route](#6-default-route)
7. [Packet forwarding](#7-packet-forwarding)
8. [Routing käsud](#8-routing-käsud)
9. [Kokkuvõte](#9-kokkuvõte)

---

## 1. SISSEJUHATUS

### Mida me juba teame?

Eelmistes tundides õppisime, et router on Layer 3 seade, mis ühendab erinevaid võrke. Õppisime ka, et igal routeri interface-l on oma IP aadress ja et arvutid kasutavad default gateway-d, et saata pakette teistesse võrkudesse.

Nüüd läheme sügavamale. Kuidas router tegelikult otsustab, kuhu pakett saata? Vastus peitub routing table-s ehk marsruutimistabelis.

### Mida me täna õpime?

Selles peatükis õpime, kuidas router teeb otsuseid. Õpime lugema routing table-t, lisama staatilisi marsruute ja seadistama default route-i. Need on fundamentaalsed oskused, mida iga võrguadministraator peab valdama.

---

## 2. MIS ON ROUTING?

### Routing definitsioon

Routing ehk marsruutimine on protsess, mille käigus router otsustab, kuhu saabunud pakett edasi saata. See on nagu GPS navigatsioon - router vaatab, kuhu pakett tahab jõuda, ja valib parima tee sinna.

Iga kord kui pakett saabub routerisse, toimub järgmine protsess. Router vaatab paketi päisest destination IP aadressi. Seejärel otsib router oma routing table-st, kas ta teab teed selle aadressini. Kui leiab, saadab paketi õiges suunas edasi. Kui ei leia, kas kasutab default route-i või hävitab paketi.

### Miks on routing oluline?

Ilma routing-uta ei saaks internet töötada. Kui sa saadad e-kirja Ameerikasse, läbib see pakett kümneid ruutereid. Iga router peab tegema otsuse, kuhu pakett edasi saata. Kui kasvõi üks router teeks vale otsuse, ei jõuaks pakett kohale.

Routing võimaldab ka võrkude eraldamist. Ettevõttes võib olla raamatupidamise võrk, müügiosakonna võrk ja külaliste võrk. Router hoiab need võrgud eraldi, aga võimaldab vajadusel ka nendevahelist suhtlust.

---

## 3. ROUTING TABLE EHK MARSRUUTIMISTABEL

### Mis on routing table?

Routing table on andmestruktuur, mida router kasutab otsuste tegemiseks. See on nagu router-i "mälu" - seal on kirjas kõik võrgud, mida router teab, ja kuidas nendesse jõuda.

Routing table-t saab vaadata käsuga `show ip route`. Siin on näide:

```
R1# show ip route

Gateway of last resort is 10.0.0.1 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.0.0.0/30 is directly connected, Serial0/0/0
L       10.0.0.2/32 is directly connected, Serial0/0/0

     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0

S    192.168.3.0/24 [1/0] via 10.0.0.1
S*   0.0.0.0/0 [1/0] via 10.0.0.1
```

### Routing table koodid

Iga rida routing table-s algab tähega, mis näitab, kuidas see route sinna sattus.

**C - Connected.** See tähendab, et võrk on otse routeriga ühendatud. Kui sa paned interface-le IP aadressi ja ütled `no shutdown`, tekib automaatselt Connected route. Router teab seda võrku, sest tema enda interface on seal.

**L - Local.** See on interface enda IP aadress. Iga Connected route kõrval on ka Local entry, mis näitab täpset interface IP-d /32 maskiga.

**S - Static.** See tähendab, et keegi (administraator) on selle route käsitsi lisanud. Router ei õppinud seda ise, vaid talle öeldi, et see võrk asub seal suunas.

**S* - Static default.** See on eriline static route, mis katab kõik tundmatud võrgud. Täht * tähendab "candidate default" ehk see on default route.

On ka teisi koode nagu D (EIGRP), O (OSPF) ja R (RIP), aga need on dünaamiliste routing protokollide jaoks. Neid õpime hiljem.

### Routing table kirje struktuur

Vaatame ühte rida lähemalt:

```
S    192.168.3.0/24 [1/0] via 10.0.0.1
```

Esimene täht (S) näitab route tüüpi - see on static route. Järgmine osa (192.168.3.0/24) on sihtkoha võrk ja mask. Nurksulgudes olevad numbrid [1/0] on administrative distance ja metric - need näitavad route "usaldusväärsust" ja "maksumust". Viimane osa (via 10.0.0.1) näitab next-hop aadressi ehk kuhu pakett edasi saata.

---

## 4. CONNECTED ROUTES

### Kuidas tekivad Connected routes?

Connected routes tekivad automaatselt, kui sa konfigureerid interface-le IP aadressi ja lülitad selle sisse. Sa ei pea midagi eraldi lisama - router teeb seda ise.

Näiteks kui sa sisestad need käsud:

```
R1(config)# interface GigabitEthernet0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# no shutdown
```

Siis routing table-sse tekib automaatselt:

```
C    192.168.1.0/24 is directly connected, GigabitEthernet0/0
L    192.168.1.1/32 is directly connected, GigabitEthernet0/0
```

### Mida Connected route tähendab?

Connected route tähendab, et router on selle võrguga otseselt ühendatud. Kui pakett tuleb sellesse võrku, siis router teab, et ta saab selle otse kohale toimetada ilma teise routeri abita.

Mõtle nii - kui sul on kaks interface-i erinevates võrkudes, siis need kaks võrku on sinu jaoks "naabrid". Sa tead neid, sest sa oled nendega füüsiliselt ühendatud.

### Connected routes on alus

Connected routes on routing-u alus. Ilma nendeta ei saaks router üldse töötada. Aga Connected routes katavad ainult võrgud, mis on otse routeri küljes. Kõik teised võrgud vajavad kas static routes või dünaamilist routing protokolli.

---

## 5. STATIC ROUTES

### Miks vajame static routes?

Router teab automaatselt ainult neid võrke, mis on tema küljes (Connected). Aga mis siis, kui on võrke, mis on kaugemal - teise routeri taga?

Kujuta ette sellist topoloogiat:

```
[Võrk A] ----[R1]---- [Link võrk] ----[R2]---- [Võrk B]
192.168.1.0         10.0.0.0            192.168.2.0
```

R1 teab võrku A (Connected) ja link võrku (Connected). Aga R1 ei tea võrgust B midagi! Kui keegi võrgust A saadab paketi võrku B, siis R1 ei oska seda edasi saata.

Lahendus on static route - me ütleme R1-le, et võrk B asub R2 suunas.

### ip route käsk

Static route lisamiseks kasutame käsku `ip route`. Süntaks on:

```
ip route [sihtkoha-võrk] [mask] [next-hop-aadress]
```

Või alternatiivne variant:

```
ip route [sihtkoha-võrk] [mask] [exit-interface]
```

### Praktiline näide

Jätkame eelmise näitega. R1 peab teadma, kuidas jõuda võrku 192.168.2.0/24. R2 aadress link võrgus on 10.0.0.2.

```
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

See käsk ütleb R1-le: "Kui sa saad paketi, mis läheb võrku 192.168.2.0/24, saada see edasi aadressile 10.0.0.2".

Pärast seda käsku näeme routing table-s:

```
S    192.168.2.0/24 [1/0] via 10.0.0.2
```

### Tähtis meeldetuletus

Static routes on kahesuunalised probleemid! Kui R1 teab, kuidas jõuda võrku B, siis see ei tähenda, et R2 teab, kuidas jõuda võrku A. Mõlemal routeril peavad olema route-id mõlemas suunas.

See on sage algajate viga - nad konfigureerivad route ainult ühes suunas ja siis imestävad, miks ping ei tööta. Ping saadab paketi KOHALE, aga vastus ei tule TAGASI, sest teine router ei tea tagasiteed.

### Static route eemaldamine

Kui sa pead static route eemaldama, kasuta sama käsku eesliitega `no`:

```
R1(config)# no ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

---

## 6. DEFAULT ROUTE

### Mis on default route?

Default route on eriline route, mis katab kõik võrgud, mida router ei tea. See on nagu "catch-all" - kui ükski teine route ei sobi, kasutatakse default route-i.

Default route-i võrguaadress on 0.0.0.0 ja mask on samuti 0.0.0.0. See kombinatsioon tähendab "kõik võrgud".

```
R1(config)# ip route 0.0.0.0 0.0.0.0 10.0.0.1
```

Routing table-s näeb see välja nii:

```
S*   0.0.0.0/0 [1/0] via 10.0.0.1
```

Täht * näitab, et see on "candidate default" ehk default route.

### Millal kasutada default route-i?

Default route on eriti kasulik väikestes võrkudes, kus on ainult üks väljapääs. Näiteks kontori võrk, kus kogu internet-liiklus läheb ühe ISP routeri kaudu. Sel juhul pole mõtet lisada tuhandeid static route-e igale internet-võrgule - lihtsam on öelda "kõik tundmatu läheb ISP poole".

Teine tüüpiline kasutuskoht on stub network ehk tupik-võrk. See on võrk, millel on ainult üks ühendus ülejäänud võrguga. Kuna kõik liiklus peab nagunii minema sama teed, on default route loogiline valik.

### Gateway of last resort

Kui default route on konfigureeritud, näitab routing table ülaosas:

```
Gateway of last resort is 10.0.0.1 to network 0.0.0.0
```

See tähendab, et kui router ei leia ühtegi muud sobivat route-i, saadab ta paketi sellele gateway-le.

---

## 7. PACKET FORWARDING

### Kuidas router otsustab?

Kui pakett saabub routerisse, toimub otsustusprotsess. Esiteks loeb router paketi päisest destination IP aadressi. Seejärel otsib router routing table-st kõik route-id, mis võiksid sobida. Kui leiab mitu võimalikku vastet, valib kõige täpsema (longest prefix match). Lõpuks saadab paketi välja õigest interface-st.

### Longest prefix match

See on üks olulisemaid routing kontseptsioone. Kui routing table-s on mitu route-i, mis võiksid paketti teenindada, valib router alati kõige täpsema ehk kõige pikema maskiga route-i.

Vaatame näidet. Routing table-s on:

```
S    10.0.0.0/8 via 192.168.1.1
S    10.1.0.0/16 via 192.168.1.2
S    10.1.1.0/24 via 192.168.1.3
```

Kui tuleb pakett aadressile 10.1.1.50, siis kõik kolm route-i sobivad! 10.1.1.50 kuulub võrku 10.0.0.0/8, samuti võrku 10.1.0.0/16 ja ka võrku 10.1.1.0/24.

Millist router kasutab? Vastus on: /24 route-i ehk via 192.168.1.3, sest /24 on kõige pikem (täpsem) mask.

Mõtle sellele nii: /24 mask tähendab, et 24 bitti peavad klapima. /16 tähendab 16 bitti ja /8 tähendab 8 bitti. Mida rohkem bitte peab klapima, seda täpsem on vaste.

### Mis juhtub kui route puudub?

Kui router ei leia ühtegi sobivat route-i ja default route ka puudub, siis pakett hävitatakse. Router saadab algsele saatjale ICMP teate "Destination Unreachable" - sihtkoht pole kättesaadav.

See on sage põhjus, miks ping ei tööta - kuskil teel puudub route ja paketid kaovad.

---

## 8. ROUTING KÄSUD

### show ip route

See on põhiline käsk routing table vaatamiseks. Näitab kõiki route-e, mida router teab.

```
R1# show ip route
```

### show ip route static

Näitab ainult staatilisi route-e. Kasulik, kui tahad kontrollida, mida sa oled käsitsi lisanud.

```
R1# show ip route static
```

### show ip route connected

Näitab ainult connected route-e ehk võrke, mis on otse routeri küljes.

```
R1# show ip route connected
```

### show ip route [võrk]

Näitab detailset infot konkreetse võrgu kohta.

```
R1# show ip route 192.168.1.0
```

### ip route (konfigureerimiskäsk)

Lisab staatilise route. Peab olema global configuration mode-s.

```
R1(config)# ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

### no ip route (konfigureerimiskäsk)

Eemaldab staatilise route. Käsk peab olema täpselt sama mis lisamisel.

```
R1(config)# no ip route 192.168.2.0 255.255.255.0 10.0.0.2
```

---

## 9. KOKKUVÕTE

### Mida me õppisime?

Selles peatükis õppisime routing põhitõdesid. Routing on protsess, mille käigus router otsustab, kuhu pakette saata. Otsused põhinevad routing table-l.

Routing table sisaldab erinevat tüüpi route-e. Connected routes tekivad automaatselt, kui interface konfigureeritakse. Static routes lisab administraator käsitsi käsuga `ip route`. Default route katab kõik tundmatud võrgud.

Kui routing table-s on mitu võimalikku vastet, kasutab router longest prefix match põhimõtet - valib kõige täpsema route.

### Põhimõisted

**Routing** - protsess, mille käigus router otsustab pakettide marsruudi.

**Routing table** - andmestruktuur, mis sisaldab kõiki route-e, mida router teab.

**Connected route** - automaatselt tekkinud route otse ühendatud võrgu jaoks.

**Static route** - käsitsi lisatud route.

**Default route** - "catch-all" route tundmatute võrkude jaoks (0.0.0.0/0).

**Next-hop** - järgmise routeri IP aadress, kuhu pakett edasi saata.

**Longest prefix match** - põhimõte, mille järgi router valib mitmest võimalikust route-st kõige täpsema.

### Käskude kokkuvõte

| Käsk | Kirjeldus |
|------|-----------|
| `show ip route` | Näita kogu routing table |
| `show ip route static` | Näita ainult static routes |
| `show ip route connected` | Näita ainult connected routes |
| `ip route X.X.X.X M.M.M.M N.N.N.N` | Lisa static route |
| `no ip route X.X.X.X M.M.M.M N.N.N.N` | Eemalda static route |
| `ip route 0.0.0.0 0.0.0.0 X.X.X.X` | Lisa default route |

### Järgmine samm

Nüüd kui sa tead teooriat, on aeg seda praktiseerida. Server labis saad ise routing table-t vaadata, static route-e lisada ja testida, kas paketid jõuavad kohale. Praktiline kogemus on parim õpetaja!

---

## 10. DÜNAAMILINE ROUTING - LÜHIÜLEVAADE

### Miks mitte ainult static routes?

Static routes töötavad hästi väikestes võrkudes. Aga kujuta ette suurt ettevõtet, kus on 500 routerit ja tuhandeid võrke. Kas keegi peaks käsitsi lisama tuhandeid static route-e igale routerile? Ja mis juhtub, kui üks link läheb katki - kas keegi peaks käsitsi kõik route-id ümber tegema?

See oleks õudusunenägu. Sellepärast eksisteerib dünaamiline routing.

### Mis on dünaamiline routing?

Dünaamilise routing puhul routerid "räägivad" omavahel ja jagavad infot võrkude kohta automaatselt. Kui uus võrk lisandub, levib info automaatselt. Kui link läheb katki, leiavad routerid automaatselt uue tee.

Administraator ei pea käsitsi route-e lisama - ta lihtsalt lülitab routing protokolli sisse ja routerid teevad ülejäänu ise.

### Peamised dünaamilised protokollid

#### RIP (Routing Information Protocol)

RIP on kõige vanem ja lihtsam dünaamiline routing protokoll. Nimi tähendab "Routing Information Protocol" ehk marsruutimisinfo protokoll.

**Kuidas töötab?** RIP kasutab metric-una hop count-i ehk loeb, mitu routerit on teel sihtkohani. Iga router teel on üks "hop". RIP valib alati tee, kus on VÄHEM hop-e, isegi kui see tee on tegelikult aeglasem.

**Piirangud:** Maksimaalselt 15 hop-i. Kui sihtkoht on rohkem kui 15 routeri kaugusel, loetakse see kättesaamatuks. See piirang teeb RIP-i sobimatuks suurtes võrkudes.

**Konvergents:** RIP saadab oma routing table koopiat naabritele iga 30 sekundi tagant. Kui võrgus midagi muutub, võib võrgu stabiliseerumine (konvergents) võtta minuteid. See on väga aeglane.

**Kasutus tänapäeval:** Harva kasutatakse, sest on aeglane ja ebaefektiivne. Võib kohata väga vanades võrkudes või õppetöös, sest on lihtne seadistada.

#### OSPF (Open Shortest Path First)

OSPF on kõige levinum routing protokoll ettevõtete võrkudes. Nimi tähendab "Open Shortest Path First" ehk avatud lühima tee esimesena.

**Kuidas töötab?** OSPF kasutab Dijkstra algoritmi, et arvutada lühim tee igasse võrku. Erinevalt RIP-ist ei loe OSPF hop-e, vaid kasutab "cost" väärtust, mis põhineb lingi kiirusel. Kiirem link = madalam cost = eelistatum tee.

**Cost arvutamine:** Cost = 100,000,000 / bandwidth (bps). Näiteks:
- 100 Mbps link: cost = 1
- 10 Mbps link: cost = 10
- 1 Gbps link: cost = 1 (vaikimisi, saab muuta)

**Link-state protokoll:** OSPF on "link-state" protokoll - iga router teab KOGU võrgu topoloogiat ja arvutab ise parimad teed. See on efektiivsem kui RIP-i "distance-vector" lähenemine.

**Konvergents:** OSPF konvergeerub sekunditega, mitte minutitega. Kui link läheb katki, levib info kohe ja routerid arvutavad uued teed kiiresti.

**Areas:** Suurtes võrkudes jagatakse OSPF aladeks (areas), et vähendada arvutuskoormust. Area 0 on alati backbone.

**Kasutus:** Kõige populaarsem IGP (Interior Gateway Protocol) ettevõtetes. Avatud standard - töötab kõigi tootjate seadmetega.

#### EIGRP (Enhanced Interior Gateway Routing Protocol)

EIGRP on Cisco väljatöötatud protokoll. Nimi tähendab "Enhanced Interior Gateway Routing Protocol" ehk täiustatud sisemine gateway protokoll.

**Kuidas töötab?** EIGRP kasutab keerukat metric valemit, mis arvestab:
- Bandwidth (ribalaius)
- Delay (viivitus)
- Valikuliselt: Load, Reliability

Vaikimisi valem: Metric = (K1 × Bandwidth + K3 × Delay) × 256

**DUAL algoritm:** EIGRP kasutab DUAL (Diffusing Update Algorithm) algoritmi. See hoiab meeles ka "backup" teed, nii et kui peamine tee läheb katki, saab EIGRP kohe üle minna ilma uuesti arvutamata.

**Konvergents:** Äärmiselt kiire - sageli alla sekundi, sest backup teed on juba teada.

**Naabrid:** EIGRP routerid saadavad "Hello" pakette iga 5 sekundi tagant (kiiretes võrkudes) ja hoiavad naabritega pidevat suhet.

**Kasutus:** Populaarne Cisco-põhistes võrkudes. Algselt Cisco proprietary, nüüd osaliselt avatud (RFC 7868), aga praktikas kasutatakse peamiselt Cisco keskkonnas.

### Protokollide võrdlus

| Omadus | RIP | OSPF | EIGRP |
|--------|-----|------|-------|
| **Tüüp** | Distance-vector | Link-state | Hybrid |
| **Metric** | Hop count | Cost (bandwidth) | Bandwidth + Delay |
| **Max hops** | 15 | Piiramatu | 255 |
| **Konvergents** | Aeglane (minutid) | Kiire (sekundid) | Väga kiire (<1 sek) |
| **Standard** | Avatud | Avatud | Cisco (osaliselt avatud) |
| **Keerukus** | Lihtne | Keskmine | Keskmine |
| **Skaleeruvus** | Väike | Suur | Suur |

### Kuidas router valib parima tee?

Kui routing table-s on mitu võimalikku teed samasse sihtkohta, kasutab router kahte kriteeriumit.

**1. Administrative Distance (AD)**

Kui route-id tulevad ERINEVATEST allikatest (näiteks üks static, teine OSPF), siis router usaldab madalamat AD-d.

| Allikas | Administrative Distance |
|---------|------------------------|
| Connected | 0 |
| Static | 1 |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |

Näide: Kui sama võrgu kohta on OSPF route (AD=110) ja RIP route (AD=120), valitakse OSPF, sest 110 < 120.

**2. Metric**

Kui route-id tulevad SAMAST protokollist, siis võrreldakse metric-ut. Madalam metric = parem tee.

- RIP: vähem hop-e = parem
- OSPF: madalam cost = parem
- EIGRP: madalam composite metric = parem

### Routing table koodid

Dünaamilised protokollid lisavad routing table-sse oma route-id erinevate koodidega:

| Kood | Protokoll |
|------|-----------|
| R | RIP |
| O | OSPF |
| D | EIGRP |

Näide routing table-st, kus on nii static kui dünaamilised route-id:

```
C    192.168.1.0/24 is directly connected, G0/0
S    192.168.2.0/24 [1/0] via 10.0.0.2
O    192.168.3.0/24 [110/20] via 10.0.0.3
D    192.168.4.0/24 [90/156160] via 10.0.0.4
```

### Static vs dünaamiline - millal mida?

**Static routes sobivad:**
- Väikesed võrgud (alla 10 routeri)
- Stub networks (ühe väljapääsuga võrgud)
- Default route ISP suunas
- Kui tahad täpset kontrolli

**Dünaamiline routing sobib:**
- Suured võrgud (üle 10 routeri)
- Keerukad topoloogiad mitme teega
- Kui vajad automaatset failover-it
- Ettevõtte võrgud

### Mida sa pead praegu teadma?

Selles kursusel me dünaamilist routing-ut ei konfigureeri. Aga sa pead teadma:

- Mis vahe on static ja dünaamilise routing vahel
- Miks dünaamilist routing-ut kasutatakse (skaleeruvus, automaatne failover)
- Protokollide nimed ja põhiomadused (RIP, OSPF, EIGRP)
- Routing table koodide tähendused (C, S, R, O, D)
- Kuidas router valib tee (AD ja metric)

Kui sa näed routing table-s koode O, D või R, siis tead, et need route-id on õpitud dünaamiliselt, mitte käsitsi lisatud.

---

## KOKKUVÕTE

### Põhipunktid meelde jätmiseks

1. **Routing table** on routeri "kaart" - näitab kõiki teadaolevaid võrke
2. **Connected routes (C)** tekivad automaatselt interface IP-st
3. **Static routes (S)** lisab administraator käsitsi
4. **Default route** (0.0.0.0/0) katab kõik tundmatud võrgud
5. **Longest prefix match** - router valib alati täpseima route
6. **Dünaamiline routing** - routerid õpivad route-e automaatselt (OSPF, EIGRP, RIP)

### Käsud mida PEAD teadma

```
show ip route              ! Näita routing table
show ip route static       ! Näita ainult static routes
ip route X.X.X.X M.M.M.M N.N.N.N   ! Lisa static route
ip route 0.0.0.0 0.0.0.0 X.X.X.X   ! Lisa default route
no ip route ...            ! Eemalda route
```
---

*Õppematerjal põhineb Cisco NetAcad CCNA moodulitel 9-10.*