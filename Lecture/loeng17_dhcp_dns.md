# Loeng 17 - DHCP ja DNS

---

## SISUKORD

1. [Sissejuhatus](#1-sissejuhatus)
2. [DHCP - Dynamic Host Configuration Protocol](#2-dhcp---dynamic-host-configuration-protocol)
3. [DORA protsess](#3-dora-protsess)
4. [DHCP konfigureerimine ruuteril](#4-dhcp-konfigureerimine-ruuteril)
5. [DNS - Domain Name System](#5-dns---domain-name-system)
6. [DNS hierarhia ja päringud](#6-dns-hierarhia-ja-päringud)
7. [DHCP ja DNS koostöö](#7-dhcp-ja-dns-koostöö)
8. [Kokkuvõte](#8-kokkuvõte)

---

## 1. SISSEJUHATUS

### Mida me juba teame?

Eelmistes tundides oleme käsitsi seadistanud igale seadmele IP aadressi, alamvõrgumaski ja default gateway. See toimib hästi väikestes laborivõrkudes, kus on 3-4 arvutit. Aga mis juhtub päris maailmas?

### Mida me täna õpime?

Selles tunnis õpime kahte protokolli, mis teevad võrguadministraatori elu palju lihtsamaks:

**DHCP** - automaatne IP aadresside jagamine. Arvuti küsib "anna mulle IP" ja saab selle automaatselt.

**DNS** - nimede tõlkimine IP aadressideks. Inimene kirjutab "google.com" ja arvuti teab, et see tähendab 142.250.74.142.

Need kaks protokolli töötavad koos: DHCP annab arvutile mitte ainult IP aadressi, vaid ka DNS serveri aadressi, et arvuti teaks, kellelt nimesid küsida.

---

## 2. DHCP - DYNAMIC HOST CONFIGURATION PROTOCOL

### Probleem: käsitsi seadistamine ei skaleeru

Kujuta ette, et töötad IT-osakonna juhina suures ettevõttes. Sul on 500 töötajat, igaühel arvuti. Igale arvutile on vaja seadistada:

- IP aadress (unikaalne!)
- Alamvõrgumask
- Default gateway
- DNS serveri aadress

See tähendab 500 × 4 = **2000 seadistust**. Kui üks number läheb valesti, siis see arvuti ei tööta võrgus.

Aga see pole veel kõik. Mis juhtub, kui:
- Uus töötaja tuleb tööle? Pead talle IP leidma ja seadistama.
- Töötaja vahetab arvutit? Pead vana IP vabastama ja uuele seadistama.
- DNS server vahetub? Pead KÕIK 500 arvutit üle käima!
- Keegi sisestab kogemata sama IP mis kolleegil? Mõlemad arvutid lakkavad töötamast.

See on haldamise õudusunenägu.

### Lahendus: DHCP

**DHCP** (Dynamic Host Configuration Protocol) lahendab kõik need probleemid. Idee on lihtne:

1. Võrgus on üks DHCP **server** (võib olla ruuter, eraldiseisev server või isegi koduruuter)
2. Server teab, milliseid IP aadresse võib välja jagada
3. Kui arvuti lülitub sisse, **küsib** ta serverilt IP aadressi
4. Server **annab** arvutile IP + kõik muud vajalikud seaded

Arvuti kasutaja ei pea midagi tegema - kõik toimub automaatselt.

### Elunäide: hotelli võtmekaart

Mõtle hotelli peale. Kui tuled hotelli:

**Ilma DHCP-ta** (staatiline IP) oleks nagu:
- Sa valid ise toa numbri
- Loodad, et keegi teine pole seda valinud
- Kui valid vale numbri, ei saa sisse

**DHCP-ga** on nagu:
- Lähed vastuvõttu
- Administraator annab sulle võtmekaardi
- Kaardil on toa number, WiFi parool, hommikusöögi aeg
- Administraator teab, millised toad on vabad

DHCP server on nagu hotelli administraator - ta teab, millised "toad" (IP aadressid) on vabad ja jagab neid külalistele.

### DHCP põhimõisted

**DHCP Server** - seade, mis jagab IP aadresse. Võib olla:
- Cisco ruuter (konfigureerime täna)
- Windows Server
- Linux server
- Koduruuter (juba sisseehitatud)

**DHCP Client** - seade, mis küsib IP aadressi. Praktiliselt iga tänapäevane seade:
- Arvutid
- Telefonid
- Tahvelarvutid
- Printerid
- Nutikellad
- IoT seadmed

**DHCP Pool** - vahemik IP aadresse, mida server saab välja jagada. Näiteks "jaga aadresse vahemikus 192.168.1.50 kuni 192.168.1.200".

**Lease** - "rent" ehk kui kaua klient saab IP-d kasutada. Tüüpiliselt 8 tundi kuni 7 päeva. Kui lease lõpeb, peab klient uuendama või saab uue IP.

**Excluded Addresses** - IP aadressid, mida server EI JAga. Need on reserveeritud serveritele, ruuteritele ja teistele seadmetele, mis vajavad püsivat IP-d.

---

## 3. DORA PROTSESS

Kui arvuti lülitub sisse ja vajab IP aadressi, toimub neljasammuline "vestlus" arvuti ja DHCP serveri vahel. Seda nimetatakse **DORA** protsessiks.

### D - Discover (Avastamine)

Arvuti lülitub sisse. Tal pole veel IP aadressi - ta ei tea isegi, kas võrgus ON DHCP serverit.

Arvuti saadab **broadcast** sõnumi kogu võrku:
- Sihtaadress: 255.255.255.255 (kõigile!)
- Sisu: "Hei! Kas keegi jagab siin IP aadresse? Palun vastake!"

Kuna see on broadcast, kuulevad seda KÕIK võrgus olevad seadmed. Aga ainult DHCP server vastab.

### O - Offer (Pakkumine)

DHCP server kuuleb Discover sõnumit ja kontrollib oma pooli:
- "Hmm, mul on vabad aadressid 192.168.1.50 kuni 192.168.1.200..."
- "Annan sellele arvutile 192.168.1.50"

Server saadab **Offer** sõnumi:
- "Tere! Mul on sulle pakkuda 192.168.1.50"
- "Gateway on 192.168.1.1"
- "DNS server on 192.168.1.10"
- "Saad seda kasutada 8 tundi"

### R - Request (Päring)

Arvuti saab pakkumise. Teoreetiliselt võiks võrgus olla MITU DHCP serverit ja arvuti võiks saada mitu pakkumist.

Arvuti valib ühe (tavaliselt esimese) ja saadab **broadcast** kinnituse:
- "Jah, ma tahan seda IP-d, mida server 192.168.1.1 mulle pakkus!"

Miks broadcast? Et TEISED DHCP serverid teaksid - "aa, see klient võttis teise serveri pakkumise, ma ei pea talle IP-d reserveerima".

### A - Acknowledgment (Kinnitus)

Server saadab lõpliku kinnituse:
- "Selge, 192.168.1.50 on nüüd SINU!"
- "Lease algab nüüd, kestab 8 tundi"

Arvuti seadistab oma võrgukaardi saadud seadetega ja on valmis võrku kasutama.

### DORA visuaalselt

```
Arvuti (klient)                              DHCP Server
      |                                           |
      |                                           |
   [Lülitub sisse, IP puudub]                     |
      |                                           |
      |------- DISCOVER (broadcast) ------------->|
      |        "Kas keegi jagab IP-sid?"          |
      |                                           |
      |                              [Kontrollib pooli]
      |                                           |
      |<-------------- OFFER ---------------------|
      |        "Pakun sulle 192.168.1.50"         |
      |        "Gateway: 192.168.1.1"             |
      |        "DNS: 192.168.1.10"                |
      |        "Lease: 8 tundi"                   |
      |                                           |
      |------- REQUEST (broadcast) -------------->|
      |        "Jah, tahan 192.168.1.50!"         |
      |                                           |
      |<-------------- ACK -----------------------|
      |        "Kinnitatud! See on sinu."         |
      |                                           |
   [Seadistab võrgukaardi]                        |
      |                                           |
   [Valmis võrku kasutama!]                       |
```

### Lease uuendamine

Mis juhtub, kui 8 tundi saab täis? Kas arvuti kaotab ühenduse?

Ei! Arvuti alustab lease uuendamist ENNE tähtaega:
- 50% lease ajast: arvuti proovib uuendada sama serveriga
- 87.5% lease ajast: kui eelmine ei õnnestunud, proovib mis tahes serveriga
- 100%: alles nüüd kaotab IP ja alustab uuesti DORA-ga

Praktikas, kui arvuti on pidevalt võrgus, kasutab ta sama IP-d aastaid.

---

## 4. DHCP KONFIGUREERIMINE RUUTERIL

Cisco ruuterit saab kasutada DHCP serverina. See on mugav väikestes võrkudes, kus pole eraldi serverit.

### Pool loomine

```
Router(config)# ip dhcp pool KONTOR
Router(dhcp-config)# network 192.168.1.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.1.1
Router(dhcp-config)# dns-server 192.168.1.10
Router(dhcp-config)# exit
```

**Mida need käsud teevad:**

| Käsk | Selgitus |
|------|----------|
| `ip dhcp pool KONTOR` | Loo uus pool nimega "KONTOR" |
| `network 192.168.1.0 255.255.255.0` | Jaga IP-sid sellest võrgust |
| `default-router 192.168.1.1` | Ütle klientidele, et gateway on see |
| `dns-server 192.168.1.10` | Ütle klientidele DNS serveri aadress |

### Excluded Addresses

Mõned IP aadressid EI TOHI kunagi DHCP-ga välja jagada:
- Ruuteri aadress (gateway) - kui keegi saab selle, võrk lakkab töötamast
- Serverite aadressid - serverid vajavad püsivat IP-d
- Printerite aadressid - et kasutajad teaksid, kuhu printida

```
Router(config)# ip dhcp excluded-address 192.168.1.1 192.168.1.49
```

See käsk ütleb: "Ära kunagi jaga aadresse vahemikus .1 kuni .49". Need jäävad serveritele ja infrastruktuurile.

**Hea praktika:**
- .1 - .10: Ruuterid ja võrguseadmed
- .11 - .30: Serverid
- .31 - .49: Printerid, reserv
- .50 - .200: DHCP pool (tavalised arvutid)
- .201 - .254: Reserv tulevikuks

### DHCP seisundi kontrollimine

```
Router# show ip dhcp binding
```

See näitab kõiki välja jagatud IP aadresse:

```
IP address       Client-ID/              Lease expiration
                 Hardware address
192.168.1.50     0100.1234.5678.90       Mar 15 2024 08:30 AM
192.168.1.51     0100.ABCD.EF01.23       Mar 15 2024 09:15 AM
```

```
Router# show ip dhcp pool
```

See näitab pooli statistikat - mitu aadressi on jagatud, mitu vaba.

---

## 5. DNS - DOMAIN NAME SYSTEM

### Probleem: inimesed ei mäleta numbreid

Proovi meelde jätta:
- 142.250.74.142
- 31.13.76.36
- 151.101.1.140

Raske, eks? Aga mis siis, kui ütlen:
- google.com
- facebook.com
- reddit.com

Palju lihtsam! Inimesed mõtlevad nimedes, arvutid numbritena. Keegi peab nende vahel tõlkima.

### Lahendus: DNS

**DNS** (Domain Name System) on nagu interneti telefoniraamat. Kui sa tahad helistada sõbrale, otsid telefoniraamatust tema nime ja leiad numbri. DNS teeb sama:

```
Sina: "Tahan minna google.com"
DNS:  "google.com = 142.250.74.142"
Sina: "Aitäh!" → arvuti ühendub 142.250.74.142
```

Ilma DNS-ita peaksid iga veebilehe aadressi numbrina meeles pidama. Internet oleks praktiliselt kasutamatu.

### Elunäide: telefoniraamat

Mõtle vanale paberkandjal telefoniraamatule:

**DNS** on nagu telefoniraamat:
- Otsid nime (google.com)
- Leiad numbri (142.250.74.142)
- Helistad numbrile

**DNS server** on nagu infotelefon:
- Helistad ja küsid "Mis on Mardi number?"
- Operaator vaatab registrist
- Ütleb sulle numbri

### DNS põhimõisted

**Domain** - domeeninimi, inimloetav aadress. Näiteks:
- google.com
- hkhk.ee
- mail.google.com

**DNS Server** - server, mis teab nimede ja IP-de seoseid

**DNS Record** - üks kirje DNS andmebaasis. Erinevad tüübid:

| Tüüp | Nimi | Otstarve | Näide |
|------|------|----------|-------|
| **A** | Address | Seob nime IPv4 aadressiga | google.com → 142.250.74.142 |
| **AAAA** | Address (IPv6) | Seob nime IPv6 aadressiga | google.com → 2607:f8b0:4004:800::200e |
| **CNAME** | Canonical Name | Alias, teine nimi samale serverile | www.google.com → google.com |
| **MX** | Mail Exchange | E-posti server | google.com mail → mail.google.com |
| **NS** | Name Server | Kes vastutab selle domeeni eest | google.com → ns1.google.com |

**TTL** (Time To Live) - kui kaua vastust "meeles hoida" (cache-da). Näiteks 3600 sekundit = 1 tund.

---

## 6. DNS HIERARHIA JA PÄRINGUD

### DNS hierarhia

DNS on organiseeritud puustruktuurina. Tipus on "juur" (root), sealt hargnevad tippdomeenid (TLD), sealt alamdomeenid.

```
                         [.]  (root - juur)
                          |
         +----------------+----------------+
         |                |                |
       [com]            [ee]             [org]
         |                |
    +----+----+      +----+----+
    |    |    |      |    |    |
 google fb amazon  hkhk   ut  delfi
    |
+---+---+
|       |
www   mail
```

Täielik domeeninimi loetakse **paremalt vasakule**:
- `www.google.com.` (punkt lõpus tähistab juurt)
- Juur → com → google → www

### Kuidas DNS päring töötab?

Kui sisestad brauserisse `www.hkhk.ee`, toimub järgmine:

**1. Arvuti küsib oma DNS serverilt**
- "Mis on www.hkhk.ee IP?"
- Sinu DNS server (selle said DHCP-st!) on tavaliselt ISP server või firma sisemine server

**2. DNS server ei tea, küsib juur-serverilt**
- "Kes teab .ee domeene?"
- Juur-server: ".ee eest vastutavad need serverid: 193.0.0.236"

**3. DNS server küsib .ee serverilt**
- "Kes teab hkhk.ee?"
- .ee server: "hkhk.ee eest vastutab see server: X.X.X.X"

**4. DNS server küsib hkhk.ee serverilt**
- "Mis on www.hkhk.ee IP?"
- hkhk.ee server: "www.hkhk.ee = 194.X.X.X"

**5. DNS server vastab sulle**
- "www.hkhk.ee = 194.X.X.X"
- Ja salvestab vastuse cache-i, et järgmine kord kiiremini vastata

### DNS cache

Et mitte iga kord sama asja küsida, hoiavad DNS serverid ja ka sinu arvuti vastuseid **cache-s** (vahemälus).

Kui küsid teist korda `www.google.com`:
- Sinu arvuti: "Mul on see juba meeles! 142.250.74.142"
- Pole vaja DNS serverit üldse tülitada

**TTL** määrab, kui kaua cache kehtib:
- TTL = 3600 → 1 tund
- TTL = 86400 → 24 tundi
- TTL = 300 → 5 minutit (kiiresti muutuvate kirjete jaoks)

### DNS testimine käsurealt

**nslookup** - kõige levinum DNS testimise käsk:

```
C:\> nslookup google.com

Server:  dns.elion.ee
Address:  194.126.115.18

Non-authoritative answer:
Name:    google.com
Address: 142.250.74.142
```

**Mida see tähendab:**
- `Server: dns.elion.ee` - sinu DNS server
- `Non-authoritative answer` - vastus tuli cache-st, mitte otse google.com serverist
- `Address: 142.250.74.142` - google.com IP aadress

**Interaktiivne režiim:**
```
C:\> nslookup
> set type=MX
> google.com
   google.com    MX preference = 10, mail exchanger = smtp.google.com
> exit
```

### DNS troubleshooting

Kui veebileht ei avane, on hea kontrollida, kas probleem on DNS-is:

```
C:\> nslookup probleem.ee
*** Can't find probleem.ee: Non-existent domain
```
→ DNS probleem! Nimi ei lahendu.

```
C:\> nslookup probleem.ee
Name: probleem.ee
Address: 1.2.3.4

C:\> ping 1.2.3.4
Request timed out.
```
→ DNS töötab, aga server ise ei vasta. Probleem on serveris, mitte DNS-is.

**DNS cache tühjendamine** (kui kahtlustad vananenud cache-i):
```
C:\> ipconfig /flushdns
Successfully flushed the DNS Resolver Cache.
```

---

## 7. DHCP JA DNS KOOSTÖÖ

### Kuidas nad koos töötavad?

DHCP ja DNS on erinevad protokollid, aga nad täiendavad teineteist:

1. **DHCP annab arvutile:**
   - IP aadressi
   - Alamvõrgumaski
   - Default gateway
   - **DNS serveri aadressi** ← see on oluline!

2. **Arvuti kasutab saadud DNS serverit** nimede lahendamiseks

3. **DNS server** tõlgib nimed IP aadressideks

```
[Arvuti]                                          
    |                                             
    |--DHCP Discover-->  [DHCP Server/Ruuter]    
    |                                             
    |<--DHCP Offer------                          
    |   IP: 192.168.1.50                          
    |   Gateway: 192.168.1.1                      
    |   DNS: 192.168.1.10  ←─────────────────────┐
    |                                             │
    |                                             │
    | Kasutaja: "www.firma.lan"                   │
    |                                             │
    |--DNS query: www.firma.lan? -->  [DNS Server]
    |                                  192.168.1.10
    |<--DNS response: 192.168.1.10 --            
    |                                             
    |--HTTP request -->  [Veebiserver 192.168.1.10]
    |<--Veebileht ------                          
```

### Praktiline näide: kontori võrk

Kujuta ette väikest kontorit:

**Võrk:** 192.168.1.0/24

**Seadmed:**
- Ruuter (gateway + DHCP server): 192.168.1.1
- DNS/Veebiserver: 192.168.1.10
- Printer: 192.168.1.20
- Töötajate arvutid: DHCP (saavad .50 - .200)

**Ruuteri DHCP konfiguratsioon:**
```
ip dhcp excluded-address 192.168.1.1 192.168.1.49
ip dhcp pool KONTOR
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 192.168.1.10
```

**DNS serveri kirjed:**
```
www.firma.lan      A    192.168.1.10
mail.firma.lan     A    192.168.1.10
printer.firma.lan  A    192.168.1.20
gateway.firma.lan  A    192.168.1.1
```

**Mis juhtub, kui Mati tuleb hommikul tööle:**

1. Mati lülitab arvuti sisse
2. Arvuti saadab DHCP Discover
3. Ruuter vastab: IP=192.168.1.57, DNS=192.168.1.10
4. Mati avab brauseri, kirjutab "mail.firma.lan"
5. Arvuti küsib DNS serverilt (192.168.1.10): "Mis on mail.firma.lan?"
6. DNS vastab: "192.168.1.10"
7. Brauser avab meiliserveri

Mati ei pea teadma ühtegi IP aadressi - kõik toimib nimede põhjal.

---

## 8. KOKKUVÕTE

### DHCP kokkuvõte

| Mõiste | Selgitus |
|--------|----------|
| **DHCP** | Protokoll IP aadresside automaatseks jagamiseks |
| **DORA** | Discover → Offer → Request → Ack |
| **Pool** | IP aadresside vahemik, mida DHCP jagab |
| **Lease** | Kui kaua klient saab IP-d kasutada |
| **Excluded** | Aadressid, mida DHCP ei jaga (serverid, ruuterid) |

**DHCP käsud:**
```
ip dhcp pool [NIMI]
 network [VÕRK] [MASK]
 default-router [GATEWAY]
 dns-server [DNS]
ip dhcp excluded-address [ALGUS] [LÕPP]
show ip dhcp binding
show ip dhcp pool
```

### DNS kokkuvõte

| Mõiste | Selgitus |
|--------|----------|
| **DNS** | Protokoll nimede tõlkimiseks IP aadressideks |
| **A record** | Seob nime IPv4 aadressiga |
| **CNAME** | Alias (teine nimi samale serverile) |
| **MX** | E-posti serveri kirje |
| **TTL** | Kui kaua vastust cache-s hoida |
| **Cache** | Vahemälu, hoiab varasemaid vastuseid |

**DNS käsud:**
```
nslookup [NIMI]
nslookup -type=MX [NIMI]
ipconfig /all          (näita DNS serverit)
ipconfig /flushdns     (tühjenda DNS cache)
ipconfig /displaydns   (näita DNS cache sisu)
```

### Miks mõlemad on olulised?

**DHCP ilma DNS-ita:**
- Arvutid saavad IP automaatselt ✓
- Aga kasutajad peavad teadma numbreid ✗
- "Mine aadressile 192.168.1.10" - kes seda mäletab?

**DNS ilma DHCP-ta:**
- Nimed töötavad ✓
- Aga iga arvuti tuleb käsitsi seadistada ✗
- Ja käsitsi tuleb DNS serveri aadress sisestada

**DHCP + DNS koos:**
- Arvuti saab IP automaatselt ✓
- Arvuti saab DNS serveri aadressi automaatselt ✓
- Kasutaja kasutab nimesid ✓
- Kõik töötab ilma käsitsi seadistamata ✓

---

## KONTROLLKÜSIMUSED

Vasta neile küsimustele enne laborit:

1. Mida tähendab DORA ja mis järjekorras need sammud toimuvad?

2. Miks on vaja excluded addresses? Too näide, mis võiks juhtuda kui ruuteri IP jagatakse DHCP-ga välja.

3. Mis vahe on A record ja CNAME vahel? Millal kumbagi kasutada?

4. Kui klient saab DHCP-st lease ajaga 8 tundi, kas ta kaotab ühenduse täpselt 8 tunni pärast? Põhjenda.

5. Mida näitab käsk `nslookup google.com` ja kuidas aru saada, kas vastus tuli cache-st?

---

*Õppematerjal põhineb Cisco NetAcad CCNA materjalidel (Moodul 15).*
