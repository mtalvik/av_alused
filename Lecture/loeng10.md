# Võrgu-, leviedastus- ja hosti-aadressid ning alamvõrkude jaotamise põhitõed

## Sissejuhatus

Kujuta ette, et sinu koolis on 200 arvutit ja IT-administraator plaanib kasutada võrku, mis annab 254 IP aadressi. Kas see on piisav? Jah, see piisab - aga vaid napilt! Sellises olukorras on vähe ruumi tuleviku laienemiseks. Just seetõttu on oluline mõista, kuidas võrke õigesti planeerida. Selles peatükis õpime, kuidas IP aadressid võrgus täpsemalt töötavad ja kuidas suuri võrke väiksemateks jagada.

## Kolm erilist aadressi igas võrgus

Iga IP võrk sisaldab kolme erinevat tüüpi aadresse, millel on kõigil oma konkreetne ülesanne. Need on network address, broadcast address ja host addresses. Mõistame neid ühekaupa.

### Network Address - võrgu identifikaator

Network address on võrgu esimene aadress ja seda võib mõelda kui võrgu "nime" või identifikaatorit. See ei ole mõeldud ühegi konkreetse seadme jaoks - see on lihtsalt viis, kuidas võrku ennast tähistada. Network addressi puhul on kõik host bittid nullid, mis tähendab, et /24 võrgu puhul on viimane oktett alati 0.

Vaatame näidet. Kui meil on võrk 192.168.1.0/24, siis network address on 192.168.1.0. Router kasutab seda aadressi oma routing table'is, et teada, kuhu paketid suunata. Iga võrgus saab olla ainult üks network address ja seda ei saa kunagi määrata ühegi arvuti, printeri või muu seadme IP aadressiks.

### Broadcast Address - kõigile saatmise aadress

Broadcast address on võrgu viimane aadress ja sellel on väga spetsiifiline funktsioon: kui saadame paketi broadcast aadressile, siis see pakett jõuab kõigile seadmetele selles võrgus. Broadcast addressi puhul on kõik host bittid ühed, mis tähendab, et /24 võrgu puhul on viimane oktett alati 255.

Meie näite võrgus 192.168.1.0/24 on broadcast address 192.168.1.255. Kujuta ette olukorda, kus üks arvuti tahab teada, kes kõik on võrgus. Ta saadab broadcast paketi aadressile 192.168.1.255 ja kõik võrgus olevad seadmed saavad selle paketi kätte ning võivad vastata. Broadcast aadressi ei saa samuti kunagi määrata konkreetsele seadmele.

### Host Addresses - seadmete aadressid

Host aadressid on kõik aadressid, mis jäävad network ja broadcast aadressi vahele. Need on aadressid, mida me tegelikult saame oma seadmetele määrata - arvutitele, printeritele, serveritele, routeritele ja kõigele muule, mis võrku ühendub.

Näiteks võrgus 192.168.1.0/24 on olukord järgmine: network address 192.168.1.0 ei saa kasutada, broadcast address 192.168.1.255 ei saa kasutada, aga kõik aadressid vahemikus 192.168.1.1 kuni 192.168.1.254 on vabad kasutamiseks. See teeb kokku 254 aadressi, mida saame määrata seadmetele. Tavaliselt määratakse esimene aadress (192.168.1.1) routeri liidese jaoks - seda kutsutakse default gateway'ks - ja ülejäänud aadressid lähevad arvutitele ja teistele seadmetele.

## Hostide arvu arvutamine - valem 2^n - 2

Nüüd, kui me teame, et igas võrgus on kaks aadressi, mida ei saa kasutada (network ja broadcast), tekib küsimus: kuidas me teame, mitu seadet saame võrku ühendada? Õnneks on selleks lihtne matemaatiline valem.

Valem on järgmine: hostide arv võrdub 2 astmes n miinus 2, kus n on host bittide arv. Vaatame, kuidas see praktikas töötab.

Võtame näiteks võrgu maskiga /24. IPv4 aadress koosneb 32 bitist, seega kui 24 bitti on network osa, siis host osa jaoks jääb 32 miinus 24, mis teeb 8 bitti. Nüüd võtame arvu 2 astmes 8, mis annab meile 256. See 256 on kõigi võimalike kombinatsioonide arv 8 bitiga. Aga meie peame lahutama 2, sest network address ja broadcast address ei saa kasutada. Seega 256 miinus 2 annab meile 254 - täpselt nii palju aadressi saame määrata seadmetele võrgus 192.168.1.0/24.

Miks me lahutame täpselt 2? Üks aadress on reserveeritud network addressiks (kõik host bittid on nullid) ja üks broadcast addressiks (kõik host bittid on ühed). Need kaks aadressi on igas võrgus erilist tüüpi ja neid ei saa kunagi määrata tavalistele seadmetele.

## Erinevad maskid ja võrgu suurused

Oluline on mõista, et mida suurem on maski number, seda väiksem on võrk. See võib esmapilgul tunduda vastupidine, aga see tuleneb sellest, et maski number näitab, mitu bitti on network osas, mitte host osas.

Vaatame erinevaid maske ja nende võrkude suurusi. Mask /24 annab meile 8 host bitti (32 - 24 = 8) ja seega 254 kasutatavat aadressi. See on ideaalne väikese ettevõtte jaoks, kus on paar sada seadet. Mask /25 annab meile 7 host bitti (32 - 25 = 7), mis tähendab 2 astmes 7 miinus 2, ehk 126 aadressi. Selline võrk sobib näiteks ühele osakonnale. Mask /26 annab 6 host bitti ja 62 aadressi, mis võiks sobida väiksele kontorile. Mask /27 annab 5 host bitti ja 30 aadressi - see võiks olla ühe kabineti või klassiruumi võrk. Mask /28 annab 4 host bitti ja ainult 14 aadressi, mis sobib väga väikesele gruppile.

Siin on täielik tabel võrdluseks:

**Mask /24:** 8 host bitti → 254 aadressi → väike ettevõte  
**Mask /25:** 7 host bitti → 126 aadressi → osakond  
**Mask /26:** 6 host bitti → 62 aadressi → kontor  
**Mask /27:** 5 host bitti → 30 aadressi → üks ruum  
**Mask /28:** 4 host bitti → 14 aadressi → väike grupp

Kui tahad arvutada mis tahes maski hostide arvu, järgi alati sama protsessi: lahuta maski number 32-st, et saada host bittide arv, võta 2 selle astmes ja lahuta 2.

## Subnetting - võrkude jagamine

Subnetting on tehnika, kus võtame suure võrgu ja jagame selle väiksemateks võrkudeks. See on äärmiselt oluline oskus võrguadministraatorile ja sellel on mitu head põhjust.

Esiteks aitab subnetting meil ressursse paremini kasutada. Kujuta ette, et sul on /24 võrk 254 aadressiga, aga ühe osakonna jaoks on vaja ainult 30 aadressi. Kui annad neile kogu /24 võrgu, raiskad sa 224 aadressi, mida keegi ei kasuta. Parem on jagada see /24 võrk väiksemateks osadeks, näiteks /27 võrkudeks, millest igaüks annab 30 aadressi.

Teiseks toob subnetting kaasa suurema turvalisuse. Kui jagad võrgu osadeks, saad erinevad osakonnad hoida eraldi võrkudes. Näiteks raamatupidamise osakond võib olla ühes võrgus ja müügiosakond teises. See tähendab, et kui ühe võrgu turvalisus on ohustatud, ei mõjuta see automaatselt teist võrku.

Kolmandaks vähendab subnetting broadcast liiklust. Broadcast paketid jõuavad ainult samasse võrku kuuluvatele seadmetele. Kui sul on üks suur võrk, siis iga broadcast pakett jõuab kõigile seadmetele. Aga kui jagad võrgu väiksemateks osadeks, jääb broadcast liiklus väiksemaks ja võrk töötab efektiivsemalt.

Vaatame konkreetset näidet. Kujuta ette, et koolil on võrk 192.168.1.0/24, mis annab 254 aadressi. Aga erinevatel osakondadel on erinevad vajadused: IT klass vajab 30 arvutit, kontor vajab 20 arvutit ja õpilaste võrk vajab 100 arvutit. Selle asemel, et anda kõigile üks suur võrk, saame jagada selle järgmiselt: IT klassile 192.168.1.0/27 (30 aadressi), kontorile 192.168.1.32/27 (30 aadressi) ja õpilastele 192.168.1.64/25 (126 aadressi). Nii on igal grupil täpselt nii palju ruumi kui vaja ja võrk on paremini organiseeritud.

## CIDR notatsioon

CIDR tähendab Classless Inter-Domain Routing ja see on tänapäeval standardne viis IP aadresside kirjutamiseks. Kui näed aadressi nagu 192.168.1.0/24, siis see kaldkriips ja number selle järel ongi CIDR notatsioon.

Number pärast kaldkriipsu (prefix) näitab, mitu bitti IP aadressis on network osa. Näiteks 192.168.1.0/24 tähendab, et esimesed 24 bitti on network osa ja ülejäänud 8 bitti (32 - 24 = 8) on host osa. See on palju lihtsam ja lühem viis kirjutada kui traditsioonilised subnet maskid decimaal kujul.

---

## 9. 🎯 SAMM-SAMMULT NÄIDE

**Ülesanne:** Leia kõik olulised aadressid võrgus `10.20.30.0/24`

### Samm 1: Host bittide arv
```
32 - 24 = 8 host bitti
```

### Samm 2: Hostide arv
```
2^8 - 2 = 256 - 2 = 254 aadressi
```

### Samm 3: Network address
```
Kõik host bittid 0 → 10.20.30.0
```

### Samm 4: Broadcast address
```
Kõik host bittid 1 → 10.20.30.255
```

### Samm 5: Kasutatavad aadressid
```
Esimene: 10.20.30.1 (tavaliselt router)
Viimane: 10.20.30.254
```

### ✅ KOKKUVÕTE:
```
Võrk:          10.20.30.0/24
Network:       10.20.30.0       (ei saa kasutada)
Esimene host:  10.20.30.1       (router/gateway)
Viimane host:  10.20.30.254     (arvuti)
Broadcast:     10.20.30.255     (ei saa kasutada)

Kokku: 254 aadressi arvutitele!
```

## Subnettingu ajalugu ja praktiline vajadus

Üheksakümnendatel aastatel mõistsid IT-spetsialistid, et IP võrkude suurus ei peaks tingimata olema fikseeritud klasside järgi. See mõte tõi kaasa tehnika nimega VLSM (Variable-Length Subnet Masking) ehk muutuva pikkusega alamvõrgu maskid. VLSM võimaldas organisatsioonidel jagada oma klassipõhiseid võrke väiksemateks alamvõrkudeks, mis sobisid nende vajadustega kõige efektiivsemalt.

Vaatame konkreetset näidet. Oletame, et jaekaubandusettevõte tahab avada kolm uut kauplust erinevates linnades. Üks kauplus New Yorgis vajab 32 host aadressi ja kaks väiksemat kauplust vajavad kummalgi 16 IP aadressi. Ettevõte on ostnud C-klassi võrgu 200.1.1.0/24. IP subnettingu peamine idee on see, et organisatsioon saab kasutada seda ühte C-klassi võrku võimalikult efektiivselt, raiskmata suuri IP-aadresside plokke.

**Viide joonisele 8:** Subnetting näide

![Joonis 8: Subnetting näide](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-subnetting-example.svg)

Nagu jooniselt näha, on ettevõttele kuuluv C-klassi võrk igale kauplusele palju liiga suur. Et muuta see kasulikuks, tükeldab ettevõte selle väiksemateks aadressiplokideks, mida kutsutakse alamvõrkudeks, ja määrab need alamvõrgud võrgu erinevatele osadele. See on palju efektiivsem ega raiska ressursse võrreldes ainult fikseeritud suurusega plokkide kasutamisega.

## Klassivaba subnetting

Klassivaba subnetting võimaldab IP aadressidele määrata suvaliseid võrgumaske, arvestamata "klassi". See tähendab, et /8, /16 ja /24 võrgumaske saab määrata mis tahes aadressile, mis traditsiooniliselt oleks kuulunud A, B või C klassi. Lisaks ei ole me enam seotud ainult nende kolme valikuga.

**Viide joonisele 9:** Klassivabade võrgumaskide näited

![Joonis 9: Klassivabad võrgumaskid](https://cdn.networkacademy.io/sites/default/files/2022-04/ipv4-subnetting-classless-mask.svg)

Vaatleme IP aadressi 10.1.1.1. Klassipõhise IP aadresseerimisega on see A-klassi aadress, mis tähendab, et võrgumask on fikseeritud väärtusele 255.0.0.0. Klassivaba aadresseerimisega aga ei tähenda IP aadressi teadmine automaatselt seda, et sa tead ka võrgumaski. Sulle tuleb selgesõnaliselt öelda, milline mask on. Näiteks IP aadressil 10.1.1.0 võiks olla võrgumask 255.255.255.0.

**Klassipõhine vs klassivaba adresseerimine:**

Klassipõhine adresseerimine määrab IP aadressiblokke vastavalt viiele eelmääratud klassile. See on vähem praktiline ja raiskab tohutul hulgal IP aadresse liiga suurtes plokkides. Võrgumask on alati fikseeritud sõltuvalt klassist.

Klassivaba adresseerimine kasutab muutuva pikkusega aadressiplokke, mis ei kuulu ühessegi klassi. See on praktilisem ja efektiivsem. Mis tahes IP aadress võib omada mis tahes võrgumaski. Just see ongi subnetting - võime kontrollida IP võrgu suurust vastavalt vajadusele.

## Subnet maski sügavam mõistmine

Subnetting käib täielikult võrgumaski ümber. Kujuta ette maailma, kus IP aadresside kõrval poleks subnet maski. Kuidas arvuti teaks, kas teine arvuti on samas võrgus või teises võrgus? See oleks võimatu.

**Viide joonisele:** Miks me vajame subnet maski

![Miks me vajame subnet maski](https://cdn.networkacademy.io/sites/default/files/2023-03/why-do-we-need-a-subnet-mask.svg)

Meenuta, et on oluline vahe, kas hostid suhtlevad sama võrgu sees või erinevate võrkude vahel. Võrgu sees käib suhtlemine switch'ide kaudu. Võrkude vahel aga läbib liiklus routereid. Ilma maskita ei oskaks arvuti öelda, kumba teed minna.

**Viide joonisele:** Subnet maski roll

![Subnet maski roll](https://cdn.networkacademy.io/sites/default/files/2023-03/the-role-of-the-subnet-mask.svg)

Subnet mask on 32-bitine binaararv järjestikustest ühtedest, mis jagab IP aadressi võrgu ja hosti osadeks. Inimesed kasutavad maske detsimaal kujul (näiteks 255.255.255.0), aga routerid töötavad binaar kujul (11111111.11111111.11111111.00000000).

Ühed subnet maskis määravad, millised bitid IP aadressis tähistavad võrgu osa. Nullid määravad, millised bitid tähistavad hosti osa. Just seetõttu on ainult 32 võimalikku väärtust võrgumaski jaoks - iga üks on kombinatsioon alguses olevatest ühtest ja lõpus olevatest nullidest.

**Viide joonisele 3:** Subnet maski funktsioon

![Joonis 3: Subnet maski funktsioon](https://cdn.networkacademy.io/sites/default/files/2023-03/function-subnet-mask.svg)

**Viide joonisele 4:** Hostide arv subnet'i kohta

![Joonis 4: Hostide arv subnet'i kohta](https://cdn.networkacademy.io/sites/default/files/2023-03/number-hosts-per-subnet.svg)

## Subnet'i piiride määramine

Subnet mask määrab subnet'i piiri. Maski põhjal leiame Network Identifier'i ja Broadcast Address'i, mis on võrgu piiri kaks otsa. Praktilises mõttes konverteerime aadressi ja maski binaar kujule, määrame võrgu ja hosti osad, seejärel muudame hosti osa kõik nullideks (see on Network ID) ja kõik ühtedeks (see on broadcast address).

**Viide joonisele 5:** Subnet'i piiri määramine

![Joonis 5: Subnet'i piiri määramine](https://cdn.networkacademy.io/sites/default/files/2023-03/determining-a%20subnet-boundary.svg)

Vaatame keerulisemat näidet: 192.168.15.55/255.255.255.192

**Viide joonisele 6:** Subnetting näide 1

![Joonis 6: Subnetting näide 1](https://cdn.networkacademy.io/sites/default/files/2023-03/subnetting-example1.svg)

Kasutatavate host aadresside arv selles subnet'is on (2^6 - 2) = 62 aadressi.

Veel keerulisemad näited on saadaval algmaterjalides:

**Viide joonisele:** Subnetting näide 2 (10.10.5.150/255.255.192.0)

![Subnetting näide 2](https://cdn.networkacademy.io/sites/default/files/2023-03/subnetting-example2.svg)

**Viide joonisele:** Subnetting näide 3 (37.3.15.200/255.255.255.224)

![Subnetting näide 3](https://cdn.networkacademy.io/sites/default/files/2023-03/subnetting-example3.svg)

## Täielik praktiline näide

Vaatame täielikku näidet algusest lõpuni. Oletame, et ettevõttel on 256 public IP aadressi plokk: 37.1.1.0/24. Ettevõttel on neli kontorit, igas 50 kasutajat. Ülesanne on jagada see plokk neljaks subnet'iks, millest igaühes on vähemalt 50 kasutatavat IP aadressi.

**Viide joonisele:** 256 aadressi plokk

![256 aadressi plokk](https://cdn.networkacademy.io/sites/default/files/2023-03/block-of-256-addresses.svg)

**Viide joonisele:** Subnetting nõuded

![Subnetting nõuded](https://cdn.networkacademy.io/sites/default/files/2023-03/subnetting-requirements.svg)

IP subnetting on protsess, kus jagatakse üks IP võrk väiksemateks alamvõrkudeks. See on oluline aspekt võrgu haldamisel ja seda kasutavad administraatorid tavaliselt, et optimeerida oma IP aadresside kasutust.

**Viide joonisele:** Mis on IP subnetting

![Mis on IP subnetting](https://cdn.networkacademy.io/sites/default/files/2023-03/what-is-ip-subnetting.svg)

Meie näites töötame võrgu 37.1.1.0/24 hosti osaga. Võrgu jagamiseks subnet'ideks konverteerime mõned kõige vasakpoolsemad host bitid võrgu bittideks. Valemid on järgmised:

```
2^subnet bitti = loodud subnet'ide arv
2^host bitti - 2 = hostide arv subnet'i kohta
```

**Viide joonisele:** Subnetting protsess

![Subnetting protsess](https://cdn.networkacademy.io/sites/default/files/2023-03/the-subnetting-process.svg)

Vähemalt 50 kasutatava host aadressiga subnet'ide jaoks vajame 6 host bitti (2^6 - 2 = 62 aadressi). Meil on 8 algset host bitti. Kui vajame 6 host bitti, jääb meile 2 bitti subnet bittideks. Uus mask on 24 + 2 = /26.

**Viide joonisele:** Subnetting näide lahendus

![Subnetting näide lahendus](https://cdn.networkacademy.io/sites/default/files/2023-03/ip-subnetting-an-example.svg)

Tulemus on neli subnet'i:

**Subnet 1:** 37.1.1.0/26 (kasutatavad: 37.1.1.1 - 37.1.1.62)  
**Subnet 2:** 37.1.1.64/26 (kasutatavad: 37.1.1.65 - 37.1.1.126)  
**Subnet 3:** 37.1.1.128/26 (kasutatavad: 37.1.1.129 - 37.1.1.190)  
**Subnet 4:** 37.1.1.192/26 (kasutatavad: 37.1.1.193 - 37.1.1.254)

**Viide joonisele:** Näite lõpptulemus

![Näite lõpptulemus](https://cdn.networkacademy.io/sites/default/files/2023-03/example-result.svg)

---

## Lõplik kokkuvõte

Selles peatükis õppisime, kuidas IP aadressid võrgus töötavad ja millised on kolm erinevat aadressi tüüpi. Network address on võrgu identifikaator, mille viimane oktett on .0 ja seda ei saa määrata ühegi seadme jaoks. Broadcast address on aadress, millega saadame paketi kõigile võrgus, selle viimane oktett on .255 ja sedagi ei saa määrata seadmele. Host aadressid on kõik aadressid nende kahe vahel (näiteks .1 kuni .254) ja need on aadressid, mida me saame ja peame oma seadmetele määrama.

Valem hostide arvu arvutamiseks on 2 astmes n miinus 2, kus n on host bittide arv. See valem töötab alati, olenemata sellest, milline mask sul on. Mida suurem on maski number, seda väiksem on võrk - mask /24 annab 254 aadressi, mask /27 annab 30 aadressi ja nii edasi.

Subnetting on tehnika, kus jagame suure võrgu väiksemateks võrkudeks. See aitab meil paremini planeerida ressursse, suurendada turvalisust ja vähendada võrguliiklust. CIDR notatsioon on standardne viis IP aadresside kirjutamiseks, kus number pärast kaldkriipsu näitab network bittide arvu.

Need põhiteadmised on vajalikud igale IT-spetsialistile, kes soovib võrkudega töötada. Järgmistes tundides jätkame subnettingu keerulisemate teemadega ja õpime, kuidas jagada võrke veel täpsemalt.
