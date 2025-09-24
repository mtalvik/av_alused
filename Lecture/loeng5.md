# Protokollid ja mudelid - Kuidas arvutid tegelikult suhtlevad

*Tere! Täna õpime, kuidas info liigub arvutist arvutisse. See on nagu postiteenuse reeglid - igaüks peab teadma, kuidas pakki pakkida ja kuhu saata.*

## Osa 1: Miks me vajame protokolle? (10 min)

### Protokoll = ühised reeglid

**Näide päris elust:**
- Telefonikõne: "Hallo?" → "Tere, kas..." → vestlus → "Nägemist!"
- Email: Kellele → Teema → Sisu → Saada
- Post: Aadress → Mark → Postkast → Sorteerimine → Kohaletoimetamine

**Võrgus sama asi:** Arvutid peavad kokku leppima, KUIDAS rääkida!

### Ilma protokollideta...

```
Arvuti A: "01101000 01101001"
Arvuti B: "???"
Arvuti C: "Что это?"
```

Kaos! Keegi ei saa aru.

## Osa 2: OSI mudel - 7 kihti (15 min)

### OSI = Open Systems Interconnection

Mõelge sellest kui 7-korruselisest majast. Info alustab ülalt ja liigub alla:

```
7. RAKENDUS      - Brauser, Skype (kasutaja näeb)
6. ESITUS        - Krüpteerimine, pakkimine
5. SEANSS        - Ühenduse haldamine
4. TRANSPORT     - TCP/UDP (kindel kohaletoimetamine)
3. VÕRK          - IP aadressid, ruutimine
2. ANDMESIDE     - Switch, MAC aadressid
1. FÜÜSILINE     - Kaablid, elekter, valgus
```

### Lihtsam viis meelde jätta

**Ülalt alla:** "**A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing"

Või eesti keeles mõelge nii:
```
7. Arvuti ekraan (mida NÄETE)
      ↓
4. Kuller (TCP - kindel kohaletoimetamine)
      ↓  
3. GPS (IP - kuhu saata)
      ↓
1. Auto/tee (kaablid)
```

## Osa 3: TCP/IP mudel - päris elu (10 min)

### TCP/IP = Internet kasutab seda!

OSI on teooria, TCP/IP on praktika. Ainult 4 kihti:

```
4. RAKENDUS     (HTTP, FTP, DNS, DHCP)
3. TRANSPORT    (TCP, UDP)
2. INTERNET     (IP)
1. VÕRGULIIDES  (Ethernet, WiFi)
```

### OSI vs TCP/IP

| OSI (7 kihti) | TCP/IP (4 kihti) | Mida teeb |
|---------------|------------------|-----------|
| 7-6-5 | Rakendus | Programmid suhtlevad |
| 4 | Transport | Kindel kohaletoimetamine |
| 3 | Internet | Aadressimine ja teekond |
| 2-1 | Võrguliides | Füüsiline edastus |

## Osa 4: Kapseldamine - Kuidas info "pakkitakse" (10 min)

### Nagu vene nukk (Matrjoška)

Iga kiht lisab oma "ümbrise":

```
DATA
[TCP päis][DATA]
[IP päis][TCP päis][DATA]
[Ethernet päis][IP päis][TCP päis][DATA][Ethernet jalus]
```

### PDU nimed igas kihis

**PDU = Protocol Data Unit** (paketi nimi selles kihis)

| Kiht | PDU nimi | Näide |
|------|----------|-------|
| Rakendus | Data | "Tere!" |
| Transport | Segment | [Port 80][Tere!] |
| Võrk | Packet | [IP 192.168.1.1][Port 80][Tere!] |
| Andmeside | Frame | [MAC AA:BB:CC][IP][Port][Tere!][CRC] |
| Füüsiline | Bits | 010110101... |

## Osa 5: Olulisemad protokollid (15 min)

### HTTP/HTTPS - Veebilehed

```
Teie brauser: "GET index.html"
Server: "200 OK, siin on leht"
```
- **Port:** 80 (HTTP) või 443 (HTTPS)
- **HTTPS** = krüpteeritud (rohelise lukuga)

### DNS - Nimeserver

**Probleem:** Keegi ei mäleta IP aadressi!
**Lahendus:** DNS tõlgib nimed numbriteks

```
Sina: "Tahan google.com"
DNS: "See on 142.250.74.46"
```

**Packet Traceri näide:**
1. PC → DNS Server: "Mis on google.com IP?"
2. DNS → PC: "142.250.74.46"
3. PC → Google: Nüüd saab ühenduda!

### DHCP - Automaatne IP

**Ilma DHCP:** Pead käsitsi sisestama IP, mask, gateway, DNS
**DHCP-ga:** Arvuti saab kõik automaatselt!

```
Uus arvuti: "DHCP DISCOVER - Kas keegi siin?"
DHCP server: "DHCP OFFER - Võid saada 192.168.1.50"
Arvuti: "DHCP REQUEST - Tahan seda!"
Server: "DHCP ACK - Pane kirja, kehtib 24h"
```

### FTP - Failide saatmine

- **Port:** 21 (kontroll), 20 (andmed)
- Kasutajanimi ja parool
- Suurte failide jaoks

### TCP vs UDP

**TCP** = Kindel, aeglane (nagu tähitud kiri)
- Kontrollib kas jõudis kohale
- Saadab uuesti kui kadus
- Näide: veebilehed, email

**UDP** = Kiire, ebakindel (nagu hõikamine)
- Ei kontrolli
- Kui kadus, kadus
- Näide: video stream, online mängud

## Osa 6: Packet Tracer praktika (20 min)

### Harjutus 1: Vaatame DNS-i tööd

**Ehitame:**
```
[PC] --- [Switch] --- [Router] --- [DNS Server]
```

**Seadistame:**
1. PC: IP 192.168.1.10, DNS 192.168.1.100
2. DNS Server: IP 192.168.1.100
3. Lisa DNS kirje: www.kool.ee → 192.168.1.100

**Testme:**
1. PC → Web Browser
2. Kirjuta: www.kool.ee
3. Simulation mode - vaata pakette!

### Harjutus 2: DHCP serveriga

**Lisa DHCP server:**
1. Pool: 192.168.1.50 - 192.168.1.150
2. Gateway: 192.168.1.1
3. DNS: 192.168.1.100

**PC seadistus:**
1. IP Configuration → DHCP
2. Vaata kuidas saab automaatselt IP!

## Osa 7: Wireshark demo (15 min)

### Mis on Wireshark?

"Võrgu mikroskoop" - näete KÕIKI pakette!

**Demo:**
1. Avan Wiresharki
2. Külastan veebilehte
3. Näitan:
   - DNS päringuid
   - TCP handshake (SYN, SYN-ACK, ACK)
   - HTTP päringuid

### Packet Tracer Simulation Mode

**Sarnane Wiresharkiga, aga lihtsam!**

1. Lülita Simulation mode sisse
2. Saada ping
3. Vaata samm-sammult:
   - ARP (MAC aadressi leidmine)
   - ICMP (ping pakett)
   - Vastus tagasi

### Filtrid mida teada

| Filter | Mida näitab |
|--------|-------------|
| `http` | Ainult veebiliiklus |
| `dns` | DNS päringud |
| `tcp.port==80` | Port 80 liiklus |
| `ip.addr==192.168.1.1` | Konkreetne IP |

## Osa 8: Standardiorganisatsioonid (5 min)

### Kes otsustab reegleid?

**IEEE** (Institute of Electrical and Electronics Engineers)
- WiFi standardid (802.11)
- Ethernet (802.3)
- Füüsilised standardid

**IETF** (Internet Engineering Task Force)
- TCP/IP
- HTTP, DNS, DHCP
- Internet protokollid

**W3C** (World Wide Web Consortium)
- HTML, CSS
- Veebi standardid

## Kokkuvõte

### Mida õppisime:

✅ **OSI mudel** - 7 kihti teoorias
✅ **TCP/IP mudel** - 4 kihti praktikas
✅ **Kapseldamine** - iga kiht lisab oma info
✅ **DNS** - nimed → numbrid
✅ **DHCP** - automaatne IP
✅ **TCP vs UDP** - kindel vs kiire

### Meelde jätta:

```
Kasutaja → HTTP → TCP → IP → Ethernet → Kaabel
         ↓      ↓     ↓    ↓         ↓
       Port 80  Kindel  Aadress  MAC    Bitid
```

## Küsimused kontrolliks

1. **Mitu kihti on OSI mudelis?** (7)
2. **Mis port kasutab HTTPS?** (443)
3. **Mis protokoll annab automaatselt IP?** (DHCP)
4. **Mis vahe on TCP ja UDP vahel?** (kindel vs kiire)
5. **Mis teeb DNS?** (tõlgib nimed IP-deks)

---

*💡 Vihje: Kui tahad häkker olla, alusta Wiresharki õppimisest!*
