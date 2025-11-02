# IPv4 Adresseerimine

## ÕPPEVÄLJUNDID

**Tead:**
- Kuidas IPv4 aadress on üles ehitatud (32 bitti, 4 oktetti)
- Mis vahe on binary ja decimal esitusel
- Mis on IP address klassid (A, B, C, D, E)
- Mis on public ja private IP aadressid
- Mis on subnet mask ja milleks seda vajatakse

**Oskad:**
- Ära tunda IP aadressi klassi
- Eristada public ja private IP aadresse
- Lugeda subnet mask-i põhitõdesid

---

## SISSEJUHATUS

Eelmisel nädalal õppisime, et Layer 3 kasutab IP aadresse ja et router ühendab erinevaid võrke. Me nägime IP aadresse nagu `192.168.1.10` või `10.0.0.1`, aga me ei vaadanud, kuidas need aadressid täpselt üles ehitatud on.

Täna vaatame IP aadressi SISSE. Õpime, mida need numbrid tähendavad, kuidas arvuti neid tegelikult loeb ja miks on erinevaid IP aadresside tüüpe.

See on oluline, sest järgmistel tundidel hakkame IP aadresse planeerima ja arvutama. Ilma selle põhja mõistmata ei saa me subnettingut teha.

---

## 1. MIKS BINARY?

### Kuidas arvutid töötavad?

Enne kui räägime IP aadressidest, peame mõistma, kuidas arvutid üldse infot salvestavad ja töötlevad.

Arvuti on elektrooniline seade. Kõik, mida arvuti teeb, toimub **elektrisignaalide** abil. Arvuti ei oska lugeda tähti ega numbreid nagu inimesed - ta oskab ainult tuvastada kaht seisundit:
- **Elekter on olemas** = 1 (pinge on)
- **Elektrit ei ole** = 0 (pinge ei ole)

See on nagu lüliti: kas ta on SEES või VÄLJAS. Sellepärast kasutavad arvutid **binary** ehk **kahendarvu süsteemi**, kus on ainult kaks numbrit: **0 ja 1**.

### Bit ja Byte

Üks number (0 või 1) on **bit** (binary digit). See on kõige väiksem info ühik arvutis.

Aga üks bit ei võimalda palju infot edastada - see on ainult "jah" või "ei". Seega ühendatakse bittid gruppidesse.

**8 bitti** kokku moodustavad **byte** (või oktett). 8 bitiga saab juba kujutada 256 erinevat väärtust (0 kuni 255).

![Min/Max binary octet](https://raw.githubusercontent.com/Haapsalu-Kutsehariduskeskus/av-alused/main/lectures/contents/ipv4_addressing/docs/min_max_octet_1.png)

![Min/Max binary values](https://raw.githubusercontent.com/Haapsalu-Kutsehariduskeskus/av-alused/main/lectures/contents/ipv4_addressing/docs/min_max_octet_2.png)

### Binary → Decimal teisendamine

Inimesed on harjunud **decimal** süsteemiga (numbrid 0-9). Me ei taha lugeda binary-t, seega arvutid teisendavad binary decimal-iks meie jaoks.

Binary töötab "astmete" printsiibi järgi. Iga bitti positsioon tähistab 2 astmeid:

![Binary to Decimal](https://raw.githubusercontent.com/Haapsalu-Kutsehariduskeskus/av-alused/main/lectures/contents/ipv4_addressing/docs/binary_to_decimal.png)

Kui bit on **1**, siis liidad selle positsiooni väärtuse. Kui bit on **0**, siis liidad 0.

**Näide:**
```
Binary:  11000000
         128 + 64 = 192

Binary:  10101000
         128 + 32 + 8 = 168

Binary:  00000001
         1 = 1
```

Sellepärast näed IP aadresse nagu `192.168.1.1` - need on binary numbrid, mis on mugavaks teisendatud decimal kujule.

### Decimal → Binary teisendamine

Vastupidine protsess on samuti oluline - kuidas teisendada decimal number binary-ks:

![Decimal to Binary](https://raw.githubusercontent.com/Haapsalu-Kutsehariduskeskus/av-alused/main/lectures/contents/ipv4_addressing/docs/decimal_to_binary.png)

Põhimõte: lahuta suurimad võimalikud 2 astmed ja märgi need bittid 1-ks. Järgmisel PAPER tunnil harjutame seda praktikas.

### Miks IP kasutab binary-t?

IP aadressid liiguvad võrgus elektriliste signaalide kujul. Router ja switch ei "loe" numbreid nii nagu inimene - nad töötlevad bittide vooge.

Kui tahad mõista, kuidas **subnet mask** töötab või kuidas võrke jagatakse, siis pead mõistma binary-t. Decimal on ainult mugav viis INIMESELE näidata, aga võrguseadmed töötavad binary-ga.

Järgmisel PAPER tunnil me harjutame binary ↔ decimal teisendamist, et see muutuks automaatseks.

---

## 2. IPv4 STRUKTUUR

### 32 bitti ja 4 oktetti

IPv4 aadress on **32 bitti** pikk number. Arvuti näeb seda binary kujul (nullid ja ühed). Aga inimesed ei oska hästi lugeda 32 bitti järjest, seega jagame selle nelja osaks.

Iga osa on **8 bitti** pikk. Kaheksat bitti kutsutakse **oktett** või **byte**. Seega IPv4 aadress koosneb neljast oktetist.

```
Binary:   11000000.10101000.00000001.00001010
          └──8bit─┘ └──8bit─┘ └──8bit─┘ └──8bit─┘
          1. oktett  2. oktett  3. oktett  4. oktett
```

### Decimal esitus

Inimesed ei taha lugeda binary-t, seega teisendame iga okteti **decimal** numbriks (0-255). Seega IP aadress kirjutatakse neljana numbrida, mida eraldab punkt.

```
Binary:   11000000.10101000.00000001.00001010
Decimal:  192     .168     .1       .10

IPv4 aadress: 192.168.1.10
```

Iga oktett saab olla vahemikus **0 kuni 255**, sest:
- 8 bitti madalam väärtus: `00000000` = 0
- 8 bitti kõrgeim väärtus: `11111111` = 255

### Miks binary oluline on?

Sa võid küsida: "Miks ma pean binary-t teadma kui ma kirjutan ikka decimal-is?"

Põhjus on lihtne: **subnet mask** ja **subnetting** töötavad binary tasemel. Kui sa tahad aru saada, miks `255.255.255.0` on subnet mask või kuidas võrke jagada, siis pead mõistma binary-t.

Aga ära muretse - järgmisel PAPER tunnis me harjutame binary ↔ decimal konversiooni praktikas.

---

## 2. IP AADRESSIDE KLASSID

### Miks on klasse vaja?

Alguses, kui internet oli väike, otsustati IP aadressid jagada **klassideks**. Iga klass oli mõeldud erineva suurusega võrkudele:
- Suured organisatsioonid (palju arvuteid) said Class A
- Keskmised organisatsioonid said Class B  
- Väikesed organisatsioonid said Class C

Tänapäeval me ei kasuta klasse enam nii rangelt (selle asemel on CIDR), aga klasside mõistmine on ikkagi oluline, sest:
- Default subnet mask-id tulevad klassidest
- NetAcad materjal õpetab klasse
- Vanad võrgud kasutavad ikkagi klassipõhiseid aadresse

### Kuidas klassi ära tunda?

Klass määratakse **esimese okteti** järgi. Vaata, mis numbriga IP aadress algab:

| Klass | Esimene oktett | Näide         | Kellele mõeldud          |
|-------|----------------|---------------|--------------------------|
| A     | 1 - 126        | 10.0.0.1      | Väga suured võrgud       |
| B     | 128 - 191      | 172.16.0.1    | Keskmised võrgud         |
| C     | 192 - 223      | 192.168.1.1   | Väikesed võrgud          |
| D     | 224 - 239      | 224.0.0.1     | Multicast (eriotstarve)  |
| E     | 240 - 255      | 240.0.0.1     | Eksperimentaalne         |

**Näited:**
- `10.50.30.1` → algab 10 → Class A
- `172.16.5.100` → algab 172 → Class B  
- `192.168.1.50` → algab 192 → Class C

**Praktiline vihje:** Kõige sagedamini näed Class C aadresse (192.168.x.x), sest need on tavalised koduvõrkudes ja väikefirmades.

![IPv4 Address Classes](https://raw.githubusercontent.com/Haapsalu-Kutsehariduskeskus/av-alused/main/lectures/contents/ipv4_addressing/docs/ipv4_address_classes.png)

### Miks 127 vahele jäi?

Class A lõpeb 126-ga ja Class B algab 128-ga. Kus on 127?

**127.x.x.x** on reserveeritud **loopback** aadressideks. Kõige kuulsam on `127.0.0.1`, mida kutsutakse ka "localhost". See aadress viitab alati sinu enda arvutile.

Kui sa pingid `127.0.0.1`, siis pakett ei lähe võrku välja - see jääb sinu arvutisse. See on kasulik testimiseks.

---

## 3. NETWORK ja HOST OSA

### IP aadress = Network + Host

Iga IP aadress koosneb kahest osast:
- **Network osa** – näitab, millises võrgus seade asub
- **Host osa** – näitab, milline konkreetne seade selles võrgus

Analoogia: kui su postiaadress on "Tallinn, Tartu mnt 10, korter 5", siis:
- "Tallinn, Tartu mnt 10" = võrk (maja aadress)
- "korter 5" = host (konkreetne korter)

### Klassipõhised piirjooned

Klassides on eelnevalt määratud, kus see piir läheb:

**Class A:** Esimene oktett = network, ülejäänud 3 = host
```
10.50.30.1
└─network─┘ └─host──┘
```

**Class B:** Esimesed 2 oktetti = network, ülejäänud 2 = host
```
172.16.5.100
└─network──┘ └─host─┘
```

**Class C:** Esimesed 3 oktetti = network, viimane = host
```
192.168.1.50
└──network──┘ └host┘
```

### Miks see oluline on?

Kui kaks seadet on **samas võrgus**, siis nende network osa on sama. Näiteks:
- `192.168.1.10` ja `192.168.1.20` on samas võrgus (192.168.1.0)
- `192.168.1.10` ja `192.168.2.10` on ERI võrkudes

Kui seadmed on samas võrgus, nad saavad otse suhelda (kasutades MAC aadresse). Kui nad on eri võrkudes, on vaja routerit.

---

## 4. SUBNET MASK

### Mis on subnet mask?

Subnet mask on number, mis näitab, **kus läheb piir network ja host osa vahel**.

Inimene ei näe IP aadressist kohe, kus see piir on. Aga arvuti vajab seda infot, et teada, kas teine seade on samas võrgus või mitte. Selleks kasutatakse subnet mask-i.

Subnet mask on samuti 32-bitine number, mis kirjutatakse 4 oktetti kujul.

### Default subnet mask-id

Igal klassil on **default subnet mask**:

| Klass | Default Subnet Mask | Binary esitus                      |
|-------|---------------------|-------------------------------------|
| A     | 255.0.0.0           | 11111111.00000000.00000000.00000000 |
| B     | 255.255.0.0         | 11111111.11111111.00000000.00000000 |
| C     | 255.255.255.0       | 11111111.11111111.11111111.00000000 |

**Binary loogika:**
- Bit `1` = see okteti osa kuulub network-i
- Bit `0` = see okteti osa kuulub host-i

Näiteks Class C mask `255.255.255.0`:
```
11111111.11111111.11111111.00000000
└────────network────────┘ └─host─┘
```

See tähendab: esimesed 3 oktetti on network, viimane oktett on host.

### Näide

IP aadress: `192.168.1.50`  
Subnet mask: `255.255.255.0`

Mis on network aadress?

Network osa on seal, kus mask on 255, seega `192.168.1`. Host osa on seal, kus mask on 0, seega `.50`.

**Network aadress:** `192.168.1.0` (host bittid pannakse nulli)

Kõik seadmed, mis on võrgus `192.168.1.0/24`, saavad omavahel otse suhelda ilma routerita.

### CIDR notatsioon

Sa võid näha ka kirjapilti nagu `192.168.1.0/24`. See on CIDR notatsioon (lühike viis subnet mask-i kirjutamiseks).

Number `/24` tähendab, et esimesed **24 bitti** on network osa. Kuna 24 bitti = 3 oktetti, siis see on sama mis `255.255.255.0`.

| Subnet mask     | CIDR  | Klassipõhine          |
|-----------------|-------|-----------------------|
| 255.0.0.0       | /8    | Class A default       |
| 255.255.0.0     | /16   | Class B default       |
| 255.255.255.0   | /24   | Class C default       |

Järgmistel tundidel me kasutame CIDR notatsiooni palju.

---

## 5. PUBLIC vs PRIVATE IP AADRESSID

### Mis on public IP?

**Public IP** aadress on aadress, mida kasutatakse **internetis**. Iga public IP peab olema **unikaalne** kogu maailmas - kaks erinevat seadet ei saa omada sama public IP-d.

Public IP-sid annab välja organisatsioon nimega IANA (ja sealt edasi regionaalsed registrid nagu RIPE). Kui tahad oma serverit internetti panna, pead ostma või rentima public IP aadressi.

### Mis on private IP?

**Private IP** aadress on aadress, mida kasutatakse **kohalikus võrgus** (kodus, kontoris). Need aadressid ei ole internetis unikaalsed - iga firma võib kasutada samu private aadresse.

Private IP-d ei lähe kunagi internetti välja. Kui tahad private IP-ga seadmest internetti minna, siis router teeb **NAT** (Network Address Translation) - see asendab private IP public IP-ga.

### Private IP aadresside vahemikud

On kolm ametlikku private IP vahemikku (RFC 1918):

| Klass | Private vahemik               | CIDR            | Kasutus                  |
|-------|-------------------------------|-----------------|--------------------------|
| A     | 10.0.0.0 - 10.255.255.255     | 10.0.0.0/8      | Suured ettevõtted        |
| B     | 172.16.0.0 - 172.31.255.255   | 172.16.0.0/12   | Keskmised võrgud         |
| C     | 192.168.0.0 - 192.168.255.255 | 192.168.0.0/16  | Kodud ja väikefirmad     |

**Näited:**
- `192.168.1.10` → private (tüüpiline kodu-router aadress)
- `10.50.30.100` → private (firma sisevõrk)
- `8.8.8.8` → public (Google DNS server)
- `172.16.5.50` → private (firma sisevõrk)

### Miks on private IP-d olulised?

IPv4 aadresse on **piiratud arv** - kokku on ainult umbes 4 miljardit IPv4 aadressi. See ei piisa kõigile seadmetele maailmas.

Private IP-d lahendasid selle probleemi: iga firma võib kasutada samu private aadresse oma sisevõrgus. Ainult routeril (või firewallil) peab olema public IP. Nii saab miljoneid seadmeid jagada vaid mõned public aadressid.

---

## 6. IPv4 AADRESSIDE JAGAMINE JA AMMUMINE

### Kes jagab IP aadresse?

Sa võid küsida: kes otsustab, kes saab millise IP aadressi? Kas keegi lihtsalt võtab mis tahab?

Ei. IPv4 aadresside jagamine on rangelt organiseeritud. Üleval on **IANA** (Internet Assigned Numbers Authority), mis on globaalne organisatsioon, kes vastutab kogu interneti aadressivaru eest.

IANA jagab IP aadressivahemikke edasi **RIR**-dele (Regional Internet Registries) - regionaalsetele registritele. Maailmas on 5 peamist RIR-i:
- **RIPE NCC** - Euroopa, Lähis-Ida (Eesti kuulub siia)
- **ARIN** - Põhja-Ameerika
- **APNIC** - Aasia ja Vaikse ookeani piirkond
- **LACNIC** - Ladina-Ameerika ja Kariibi mere piirkond
- **AFRINIC** - Aafrika

RIR-id jagavad IP aadresse edasi **LIR**-dele (Local Internet Registries), mis on tavaliselt **internetiteenuse pakkujad** (ISP-d). Nemad omakorda jagavad IP aadresse lõppkasutajatele - firmadele ja eraisikutele.

```
        IANA (globaalne)
           |
    ┌──────┴──────┬──────┬──────┬──────┐
    |      |      |      |      |
  RIPE   ARIN   APNIC  LACNIC AFRINIC  (RIR-id)
    |
    └─── ISP-d (LIR-id)
           |
    └─── Firmad ja kasutajad
```

### IPv4 Exhaustion - aadressid said otsa

IPv4 kasutab 32 bitti, mis tähendab, et kokku on **4,294,967,296** erinevat IPv4 aadressi. See tundub palju, aga tegelikkuses see ei piisa.

**3. veebruaril 2011** anti välja viimased vabad /8 blokid IANA poolt RIR-dele. See tähendas, et IANA varu sai otsa. Järgnevatel aastatel said ka RIR-ide varud otsa või peaaegu otsa.

![Map of the Internet - IPv4 Space](https://circleid.com/images/uploads/map_of_the_internet.jpg)

See kaart näitab kogu IPv4 aadressiruumi. Iga ruut esindab /8 blokki (16 miljonit aadressi). Enamik ruute on juba välja jagatud.

### Miks aadressid said otsa?

Mitmed põhjused:
- **Seadmete arv kasvas** - igal arvutil, telefonil, tahvelarvutil, IoT seadmel on vaja IP aadressi
- **Raiskamine** - alguses anti suurtele firmadele terved Class A blokid (16 miljonit aadressi), mida nad ei kasutanud täielikult
- **Internet kasvas kiiremini** kui keegi ootas

### Lahendused

Kuna IPv4 aadressid said otsa, leiti mitu lahendust:

**1. NAT (Network Address Translation)**
NAT võimaldab paljudel seadmetel jagada ühte public IP aadressi. Kodus on sul üks public IP (mille ISP andis), aga kõik su seadmed kasutavad private IP aadresse. Router teeb NAT-i ja tõlgib private IP-d public IP-ks.

**2. Private IP aadressid**
Nagu me varem õppisime, on kolm private IP vahemikku (10.x.x.x, 172.16.x.x, 192.168.x.x). Need võimaldavad igal organisatsioonil kasutada samu aadresse sisemiselt, ilma et need oleks globaalselt unikaalsed.

**3. IPv6**
Pikaajaline lahendus on **IPv6**, mis kasutab 128 bitti (mitte 32). See annab peaaegu lõpmatu arvu aadresse - 340 undecillion (340 koos 36 nulliga). IPv6 õpime hiljem kursusel.

**4. Aadresside taaskasutamine**
Ettevõtted ja ISP-d tagastavad kasutamata IP aadresse, mida saab uuesti jagada.

---

## 7. NETWORK, BROADCAST JA HOST AADRESSID

### Kolm erilist aadressi

Igas võrgus on kolm tüüpi aadresse, mida pead teadma:

**Network address** - võrgu nimi
**Broadcast address** - aadress, millega saadad paketti kõigile võrgus
**Host addresses** - aadressid, mida saad seadmetele määrata

### Network Address

Network address on võrgu esimene aadress, kus kõik host bittid on **0**.

Näiteks võrgus `192.168.1.0/24`:
- Network osa: `192.168.1` (esimesed 24 bitti)
- Host osa: `.0` (viimased 8 bitti, kõik nullid)
- **Network address: 192.168.1.0**

Network aadressi ei saa määrata ühelegi seadmele. See on ainult võrgu "nimi" või identifikaator. Router kasutab seda aadressi routing table-s.

### Broadcast Address

Broadcast address on võrgu viimane aadress, kus kõik host bittid on **1**.

Näiteks võrgus `192.168.1.0/24`:
- Network osa: `192.168.1` (esimesed 24 bitti)
- Host osa: `.255` (viimased 8 bitti, kõik ühed: 11111111 = 255)
- **Broadcast address: 192.168.1.255**

Kui saadad paketi broadcast aadressile, siis see pakett läheb **kõigile** seadmetele selles võrgus. Broadcast aadressi ei saa samuti määrata ühelegi seadmele.

### Host Addresses

Host aadressid on kõik aadressid network ja broadcast aadressi vahel. Need on aadressid, mida saad määrata seadmetele.

Näiteks võrgus `192.168.1.0/24`:
- Network: `192.168.1.0` ❌ (ei saa kasutada)
- Host aadressid: `192.168.1.1` kuni `192.168.1.254` ✅ (254 aadressi)
- Broadcast: `192.168.1.255` ❌ (ei saa kasutada)

### Hostiide arvu arvutamine

Formula: **2^n - 2**

Kus `n` = host bittide arv

Näiteks `/24` võrgus:
- Network bitte: 24
- Host bitte: 32 - 24 = 8
- Hostide arv: 2^8 - 2 = 256 - 2 = **254**

Miks miinus 2? Sest network address ja broadcast address ei saa hostidele määrata.

### Näide: Class C võrk

```
Võrk: 192.168.1.0/24

Network:   192.168.1.0        (ei saa kasutada)
Host 1:    192.168.1.1        ✅
Host 2:    192.168.1.2        ✅
Host 3:    192.168.1.3        ✅
...
Host 254:  192.168.1.254      ✅
Broadcast: 192.168.1.255      (ei saa kasutada)

Kokku: 254 kasutatavat host aadressi
```

Tavaliselt määratakse `.1` routeri liidese jaoks (default gateway) ja ülejäänud aadressid (`.2` kuni `.254`) arvutitele ja teistele seadmetele.

---

## 8. KOKKUVÕTE

### Peamised punktid

IPv4 aadress on 32 bitti pikk, jagatuna 4 oktettiks. Inimesed kirjutavad seda decimal kujul (nt 192.168.1.10), aga arvuti loeb seda binary-na. Binary on oluline, sest võrguseadmed töötlevad elektrilisi signaale (0 ja 1).

IP aadresside klassid (A, B, C, D, E) määravad, kuidas IP jaguneb network ja host osaks. Class A on suurte võrkude jaoks, Class C väikeste jaoks. Tänapäeval kasutame rohkem CIDR-i, aga klassid on ikkagi olulised mõista.

Subnet mask näitab, kus läheb piir network ja host osa vahel. Ilma subnet mask-ita ei oska arvuti teha vahet, kas teine seade on samas võrgus või mitte. Default maskid on /8 (Class A), /16 (Class B), /24 (Class C).

Public IP aadressid on unikaalsed kogu internetis ja neid jagab IANA → RIR → ISP. Private IP aadressid on mõeldud kohalikeks võrkudeks ja neid saab korduvalt kasutada (10.x.x.x, 172.16.x.x, 192.168.x.x).

IPv4 aadressid said 2011. aastal otsa. Lahendused on NAT, private IP-d ja tulevikus IPv6. Tänu NAT-ile ja private IP-dele saavad miljonid seadmed jagada vaid mõned public IP aadressid.

Igas võrgus on kolm erilist aadressi: network address (võrgu nimi, kõik host bittid 0), broadcast address (saadab kõigile, kõik host bittid 1), ja host aadressid (nende vahel, määratavad seadmetele). Formula 2^n - 2 annab hostide arvu.

### Edasi 90min PAPER tunnis

Järgmine tund on praktika aeg! Me harjutame:
- Binary ↔ Decimal teisendamist
- IP klassi äratundmist
- Network aadressi leidmist
- Subnet mask-i binary analüüsi

See 30min andis sulle teoreetilise aluse. Järgmine tund paneb selle praktikasse.

### Edasi 120min SERVER LABis

Serveri tunnis me kasutame seda teooriat päriselt:
- Planeerime IP aadresse oma võrgule
- Seadistame routeritele ja arvutitele IP-sid
- Kontrollime, kas sama võrgu seadmed näevad teineteist
- Vaatame, kuidas router ühendab erinevaid võrke

---

**Valmis järgmiseks tunniks!** 🎯