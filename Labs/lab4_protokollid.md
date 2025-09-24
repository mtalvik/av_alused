# Labor: Protokollid ja mudelid praktikas

**Aeg:** 45 minutit
**Teema:** Kuidas protokollid töötavad OSI/TCP-IP mudelites

---

## Mida me täna õpime?

- Näeme kuidas DNS, DHCP ja HTTP töötavad
- Jälgime pakette läbi OSI kihtide
- Mõistame kapseldamist (kuidas info "pakkitakse")

---

## Osa 1: Ehitame võrgu protokollide testimiseks (10 min)

### Võrgu plaan:

```
[PC1] ----[Switch]---- [PC2]
            |
      [DHCP Server]
            |
       [DNS Server]  
            |
       [Web Server]
```

### Sammud:

**1. Lisa seadmed:**
- 2x PC (End Devices → PC)
- 3x Server (End Devices → Server)
- 1x Switch 2960

**2. Miks kasutame Straight-Through kaablit?**

**OSI Layer 1 (Physical) selgitus:**
- Straight kaabel = erinevate seadmete vahel (PC ↔ Switch)
- Crossover kaabel = samade seadmete vahel (Switch ↔ Switch)
- See on füüsiline kiht - elektrisignaalid!

**Ühenda:**
- Connections → Copper Straight-Through
- Kõik seadmed switchi külge

**3. Nimeta serverid** (et aru saada):
- Server0 → Config → Display Name: "DHCP-Server"
- Server1 → Config → Display Name: "DNS-Server"
- Server2 → Config → Display Name: "Web-Server"

---

## Osa 2: Layer 3 (Network) - IP aadressid (5 min)

### Miks serveritel staatilised IP-d?

**OSI Layer 3 = IP aadressid = kuhu pakk läheb**

Serverid peavad alati samas kohas olema (nagu pood - ei liigu ringi)

**DHCP Server:** 192.168.1.5
**DNS Server:** 192.168.1.10
**Web Server:** 192.168.1.20

**Seadista:**
- Kliki Server → Desktop → IP Configuration
- Sisesta IP ja Subnet Mask (255.255.255.0)

---

## Osa 3: DHCP protokoll - Layer 7 (Application) (10 min)

### Mis on DHCP?

**DHCP = Dynamic Host Configuration Protocol**
- Töötab Application Layer (Layer 7)
- Kasutab UDP porti 67/68 (Layer 4)
- Jagab automaatselt IP aadresse

### Seadista DHCP:

1. DHCP-Server → Services → DHCP
2. **Service:** ON
3. **Pool seaded:**
   - Start IP: 192.168.1.100
   - Subnet: 255.255.255.0
   - DNS Server: 192.168.1.10 (meie DNS!)
   - Save

### DHCP 4-sammu protsess:

```
PC → DHCP DISCOVER → "Kas keegi siin?"
DHCP → OFFER → "Võid saada 192.168.1.100"
PC → REQUEST → "Tahan seda!"
DHCP → ACK → "Ole lahke, sinu oma!"
```

### Test PC1 DHCP-ga:

1. PC1 → IP Configuration → DHCP
2. Simulation Mode sisse!
3. Capture → näed 4 DHCP paketti

**Küsimus:** Mis värvi on DHCP paketid? (kollased)

---

## Osa 4: DNS protokoll - nimede tõlkimine (10 min)

### Mis on DNS?

**DNS = Domain Name System**
- Layer 7 (Application)
- Port 53 UDP
- Tõlgib: www.google.ee → 8.8.8.8

### Seadista DNS:

1. DNS-Server → Services → DNS
2. **Service:** ON
3. Lisa kirjed:

| Protokoll näide | Name | IP |
|-----------------|------|-----|
| HTTP veebileht | www.kool.ee | 192.168.1.20 |
| FTP server | ftp.kool.ee | 192.168.1.30 |
| Mail server | mail.kool.ee | 192.168.1.40 |

### Kuidas DNS töötab?

```
Layer 7: PC küsib "Mis on www.kool.ee?"
Layer 4: UDP port 53
Layer 3: Pakk läheb 192.168.1.10 (DNS)
Layer 2: Switch edastab MAC aadressi järgi
Layer 1: Elektrisignaalid kaablis
```

---

## Osa 5: HTTP protokoll - veebilehed (10 min)

### Mis on HTTP?

**HTTP = HyperText Transfer Protocol**
- Layer 7 (Application)
- TCP port 80
- Veebilehtede protokoll

### Seadista Web Server:

1. Web-Server → Services → HTTP
2. **Service:** ON
3. Muuda index.html:

```html
<html>
<body>
<h1>OSI Mudel</h1>
<p>Layer 7: Application - HTTP (see leht)</p>
<p>Layer 4: Transport - TCP port 80</p>
<p>Layer 3: Network - IP 192.168.1.20</p>
<p>Layer 2: Data Link - MAC aadress</p>
<p>Layer 1: Physical - Kaabel</p>
</body>
</html>
```

### Test:
PC1 → Web Browser → www.kool.ee

---

## Osa 6: Kapseldamine - Simulation Mode (10 min)

### Vaatame kuidas pakk "kasvab"

1. **Simulation Mode** sisse
2. PC1 → Web Browser → www.kool.ee
3. Kliki paketil → PDU Details

### Kapseldamise protsess:

```
APPLICATION: HTTP GET request
     ↓ (lisab TCP päise)
TRANSPORT: [TCP port 80][HTTP data]
     ↓ (lisab IP päise)
NETWORK: [IP 192.168.1.20][TCP][HTTP]
     ↓ (lisab Ethernet päise)
DATA LINK: [MAC][IP][TCP][HTTP][CRC]
     ↓ (muudab bittideks)
PHYSICAL: 010110101...
```

### PDU nimed:

| Layer | PDU nimi | Packet Trackeris |
|-------|----------|------------------|
| 7-5 | Data | HTTP request |
| 4 | Segment | TCP segment |
| 3 | Packet | IP packet |
| 2 | Frame | Ethernet frame |
| 1 | Bits | Signal |

---

## Osa 7: Protokollide port numbrid (5 min)

### Miks pordid olulised?

**Layer 4 (Transport) kasutab porte:**

| Protokoll | Port | TCP/UDP | Kasutus |
|-----------|------|---------|---------|
| HTTP | 80 | TCP | Veebilehed |
| HTTPS | 443 | TCP | Turvaline veeb |
| DNS | 53 | UDP | Nime tõlge |
| DHCP | 67/68 | UDP | IP jagamine |
| FTP | 21 | TCP | Failid |

### Test Command Prompt:
```
netstat -an
```
Näed avatud porte!

---

## Kokkuvõte: OSI vs TCP/IP

### Meie labor näitas:

**OSI mudel:**
- Layer 7: HTTP, DNS, DHCP (rakendused)
- Layer 4: TCP/UDP (pordid)
- Layer 3: IP (aadressid)
- Layer 2: Ethernet (MAC)
- Layer 1: Kaablid (signaalid)

**TCP/IP mudel (lihtsam):**
- Application: HTTP, DNS, DHCP
- Transport: TCP/UDP
- Internet: IP
- Network Access: Ethernet

---

## Salvesta:
File → Save As → Labor3_Protokollid_[Nimi].pkt

---

## Küsimused aruteluks:

1. **Miks DNS kasutab UDP, aga HTTP kasutab TCP?**
   (Vihje: kiirus vs usaldusväärsus)

2. **Mis juhtub kui DNS server maas?**
   (Proovi: lülita DNS server välja)

3. **Mitu kihti läbib email saatmisel?**
   (Kõik 7!)

---

## Boonus: Protokollide järjekord

Õige järjekord PC käivitumisel:
1. DHCP (saan IP)
2. DNS (tõlgin nimed)
3. HTTP (avan veebilehe)

Ilma DHCP → pole IP → ei saa võrku
Ilma DNS → pean teadma IP → ebamugav
Ilma HTTP → ei näe veebilehti
