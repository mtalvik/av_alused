# Lab 11: DHCP ja DNS

## Õpiväljundid

Selle labori lõpuks oskad:
- Konfigureerida ruuteri DHCP serverina
- Määrata DHCP pool ja excluded addresses
- Konfigureerida DNS serveri
- Kasutada `nslookup` käsku DNS testimiseks
- Seadistada DHCP, et jagaks klientidele DNS serveri aadressi

---

## Taust

Seni oleme kõik IP aadressid käsitsi seadistanud. Päris võrkudes oleks see võimatu - kujuta ette 500 arvutiga kontorit, kus iga IP tuleb käsitsi sisestada!

**DHCP** (Dynamic Host Configuration Protocol) lahendab selle probleemi - arvutid saavad IP aadressi automaatselt.

**DNS** (Domain Name System) lahendab teise probleemi - inimesed ei taha meeles pidada numbreid nagu 142.250.74.142, vaid nimesid nagu google.com.

Selles laboris seadistame ruuteri DHCP serverina ja DNS serveri, et arvutid saaksid:
1. IP aadressi automaatselt (DHCP-st)
2. DNS serveri aadressi (DHCP-st)
3. Kasutada nimesid IP aadresside asemel (DNS-ist)

---

## Vajalikud seadmed

- 1 × Router (2811)
- 1 × Switch (2960)
- 1 × Server (Generic Server)
- 2 × PC

---

# Osa 1: Võrgu ehitamine

## 1.1 Topoloogia

```
                    [Router]
                   Fa0/0 |
                    .1   |
                         |
            192.168.1.0/24
                         |
         +-------+-------+-------+
         |       |       |       |
       Fa0/1   Fa0/2   Fa0/3   Fa0/4
      [Switch]                    
         |       |       |
       Fa0/0   Fa0/0   Fa0/0
     [Server] [PC1]   [PC2]
       .10    DHCP    DHCP
       DNS
```

**Ühendused:**

| Ühendus | Liides ↔ Liides |
|---------|-----------------|
| Router ↔ Switch | Fa0/0 ↔ Fa0/1 |
| Server ↔ Switch | Fa0 ↔ Fa0/2 |
| PC1 ↔ Switch | Fa0 ↔ Fa0/3 |
| PC2 ↔ Switch | Fa0 ↔ Fa0/4 |

---

## 1.2 IP planeerimine

**Võrk:** 192.168.1.0/24

**Täida tabel:**

> **Vihje:** Gateway saab tavaliselt .1, server saab .10, DHCP pool algab .50-st

| Seade | Liides | IP aadress | Alamvõrgumask | Seadistus |
|-------|--------|------------|---------------|----------|
| Router | Fa0/0 | | | Staatiline (gateway) |
| Server | Fa0 | | | Staatiline (DNS) |
| PC1 | Fa0 | (saab DHCP-st) | (saab DHCP-st) | DHCP |
| PC2 | Fa0 | (saab DHCP-st) | (saab DHCP-st) | DHCP |

**DHCP seaded (täidad pärast Osa 2 lugemist):**

| Parameeter | Väärtus |
|------------|---------|
| Pool nimi | |
| Network | |
| Default-router (gateway) | |
| DNS-server | |
| Excluded addresses (algus) | |
| Excluded addresses (lõpp) | |

---

## 1.3 Ruuteri põhikonfiguratsioon

1. Nimeta ruuter: `Perekonnanimi-R1`
2. Konfigureeri Fa0/0 liides IP aadressiga 192.168.1.1/24
3. Ära unusta interface'i sisse lülitada!

---

## 1.4 Serveri IP seadistamine

Seadista serverile **staatiline IP** vastavalt oma tabelile.

> **NB!** Server kasutab iseennast DNS serverina - DNS serveri väljale sisesta serveri enda IP!

---

# Osa 2: DHCP konfigureerimine

## 2.1 DHCP Pool loomine

```
Perekonnanimi-R1(config)#ip dhcp pool KONTOR
Perekonnanimi-R1(dhcp-config)#network 192.168.1.0 255.255.255.0
Perekonnanimi-R1(dhcp-config)#default-router 192.168.1.1
Perekonnanimi-R1(dhcp-config)#dns-server 192.168.1.10
Perekonnanimi-R1(dhcp-config)#exit
```

**Mida käsud teevad:**

| Käsk | Selgitus |
|------|----------|
| `ip dhcp pool KONTOR` | Loo DHCP pool nimega "KONTOR" |
| `network 192.168.1.0 255.255.255.0` | Jaga IP-sid sellest võrgust |
| `default-router 192.168.1.1` | Ütle klientidele, et gateway on .1 |
| `dns-server 192.168.1.10` | Ütle klientidele, et DNS server on .10 |

---

## 2.2 Excluded Addresses

> **TÄHTIS:** Excluded addresses tuleb seadistada ENNE kui kliendid IP-d küsivad!

```
Perekonnanimi-R1(config)#ip dhcp excluded-address 192.168.1.1 192.168.1.49
```

See käsk ütleb: "Ära jaga aadresse .1 kuni .49!" 

Need jäävad ruuterile (.1) ja serverile (.10). DHCP hakkab jagama alates .50-st.

### ❓ Küsimus 1
Miks on vaja excluded addresses? Mis juhtuks, kui DHCP jagaks välja ruuteri IP aadressi?

**Vastus:** _______________________________________________________________

---

## 2.3 PC-de seadistamine DHCP kasutama

**PC1 ja PC2:**

1. Kliki PC-l → **Desktop** → **IP Configuration**
2. Vali **DHCP**
3. Oota kuni PC saab IP (näed "DHCP request successful")

### 📸 Screenshot 1
PC1 IP Configuration pärast DHCP-d (näita saadud IP, mask, gateway, DNS).

---

## 2.4 DHCP kontrollimine

**Ruuteril:**
```
Perekonnanimi-R1#show ip dhcp binding
```

See näitab, millised IP-d on välja jagatud ja kellele (MAC aadress).

Peaksid nägema välja jagatud IP-sid ja MAC aadresse.

### 📸 Screenshot 2
`show ip dhcp binding` väljund.

### ❓ Küsimus 2
Mis IP aadressi sai PC1? Mis IP aadressi sai PC2? Miks just need numbrid?

**Vastus:** _______________________________________________________________

---

## 2.5 Ping test

Testi, et võrk töötab:

```
PC1> ping 192.168.1.1       (gateway)
PC1> ping 192.168.1.10      (DNS server)
PC1> ping [PC2 IP]          (vaata PC2 IP Configuration-ist)
```

☐ Gateway töötab
☐ DNS server töötab
☐ PC2 töötab

---

# Osa 3: DNS konfigureerimine

## 3.1 DNS serveri seadistamine

Packet Traceris on serveri DNS funktsionaalsus lihtsustatud.

1. Kliki serveril → **Services** → **DNS**
2. Lülita DNS **On**
3. Lisa kirjed:

**Lisa kolm kirjet:**

| Name | Type | Address |
|------|------|----------|
| www.firma.lan | A Record | 192.168.1.10 |
| server.firma.lan | A Record | 192.168.1.10 |
| gateway.firma.lan | A Record | 192.168.1.1 |

Iga kirje jaoks:
1. Täida väljad (Name, Type, Address)
2. Kliki **Add**

### 📸 Screenshot 3
DNS serveri konfiguratsioon (näita lisatud kirjeid).

---

## 3.2 DNS testimine - nslookup

**PC1-l:**

1. Kliki → **Desktop** → **Command Prompt**
2. Sisesta:

```
C:\> nslookup www.firma.lan
```

Peaksid nägema oma DNS serveri IP-d ja www.firma.lan lahendatud IP-d.

### 📸 Screenshot 4
`nslookup www.firma.lan` väljund.

---

## 3.3 Ping nimega

```
PC1> ping www.firma.lan
PC1> ping gateway.firma.lan
```

Peaksid nägema, et nimi lahendati IP-ks ja ping õnnestus.

### 📸 Screenshot 5
`ping www.firma.lan` väljund (näita, et nimi lahendati IP-ks).

### ❓ Küsimus 3
Mis juhtus, kui tegid `ping www.firma.lan`? Kuidas PC teadis, mis IP aadressile pöörduda?

**Vastus:** _______________________________________________________________

---

# Osa 4: Veebilehe testimine

## 4.1 HTTP serveri seadistamine

1. Serveril → **Services** → **HTTP**
2. HTTP peaks olema juba **On**
3. Võid muuta index.html sisu (valikuline)

## 4.2 Veebilehe avamine nimega

**PC1-l:**

1. **Desktop** → **Web Browser**
2. Sisesta aadressiribale: `http://www.firma.lan`
3. Vajuta **Go**

### 📸 Screenshot 6
Veebibrauser näitab www.firma.lan lehte.

### ❓ Küsimus 4
Kirjelda, mis samme PC1 läbis, et veebileht avaneks:
1. PC1 küsis __________ serverilt, mis on www.firma.lan IP
2. DNS vastas: www.firma.lan = __________
3. PC1 ühendus veebiserveriga aadressil __________

---

# Osa 5: Troubleshooting

## 5.1 DNS probleemi simuleerimine

**Katkesta DNS teenus:**

1. Serveril → **Services** → **DNS**
2. Lülita DNS **Off**

**Testi PC1-l:**
```
PC1> nslookup www.firma.lan
```

**Mis juhtub?** _______________________________________________________________

```
PC1> ping www.firma.lan
```

**Mis juhtub?** _______________________________________________________________

```
PC1> ping 192.168.1.10
```

**Mis juhtub?** _______________________________________________________________

### ❓ Küsimus 5
Miks ping IP aadressiga töötab, aga ping nimega ei tööta?

**Vastus:** _______________________________________________________________

---

## 5.2 DNS taastamine

1. Serveril → **Services** → **DNS** → **On**
2. Kontrolli, et kirjed on alles
3. Testi uuesti: `nslookup www.firma.lan`

☐ DNS töötab jälle

---

## 5.3 DHCP probleemi simuleerimine

**Lisa uus PC3 võrku:**

1. Lisa PC3, ühenda switchiga
2. Seadista DHCP
3. Kontrolli, mis IP ta sai

```
Perekonnanimi-R1#show ip dhcp binding
```

**Kolmas IP peaks olema:** _______________

---

# Osa 6: Kokkuvõte ja esitamine

## 6.1 Kontrolltabel

Veendu, et kõik töötab:

| Test | Tulemus |
|------|---------|
| PC1 sai DHCP-st IP | ☐ |
| PC1 sai DHCP-st DNS serveri | ☐ |
| PC1 saab pingida gateway-d | ☐ |
| PC1 saab pingida DNS serverit | ☐ |
| nslookup www.firma.lan töötab | ☐ |
| ping www.firma.lan töötab | ☐ |
| Veebibrauser avab www.firma.lan | ☐ |

---

## 6.2 Täida tabel

**DHCP seaded:**

| Parameeter | Väärtus |
|------------|---------|
| Pool nimi | |
| Võrk | |
| Gateway | |
| DNS server | |
| Excluded addresses | |

**DNS kirjed:**

| Nimi | Tüüp | IP aadress |
|------|------|------------|
| | | |
| | | |
| | | |

---

### ❓ Küsimus 6
Mis on DORA? Kirjelda iga sammu ühe lausega.

- **D** = _______________________________________________________________
- **O** = _______________________________________________________________
- **R** = _______________________________________________________________
- **A** = _______________________________________________________________

### ❓ Küsimus 7
Mida tähendab TTL DNS kontekstis?

**Vastus:** _______________________________________________________________

### ❓ Küsimus 8
Nimeta 2 põhjust, miks serveritele antakse staatiline IP, mitte DHCP.

1. _______________________________________________________________
2. _______________________________________________________________

---

## Salvesta

```
Perekonnanimi-R1#copy running-config startup-config
```

Salvesta: `Perekonnanimi_DHCP_DNS.pkt`

---

# Esitamine

**Google Docs:**
- Kopeeri juhend Google Docs'i
- Täida tabelid ja küsimused
- Lisa screenshots õigesse kohta (kohe pärast vastavat ülesannet)
- Nimeta: `Perekonnanimi_DHCP_DNS`

**Packet Tracer:** `Perekonnanimi_DHCP_DNS.pkt`

**Kontrollnimekiri:**
- ☐ IP tabel (Osa 1.2)
- ☐ DHCP tabel (Osa 1.2)
- ☐ Screenshot 1: PC1 IP Configuration
- ☐ Screenshot 2: `show ip dhcp binding`
- ☐ Küsimus 1-2 vastatud
- ☐ Screenshot 3: DNS konfiguratsioon
- ☐ Screenshot 4: `nslookup`
- ☐ Screenshot 5: `ping www.firma.lan`
- ☐ Küsimus 3 vastatud
- ☐ Screenshot 6: Veebibrauser
- ☐ Küsimus 4-5 vastatud
- ☐ Küsimus 6-8 vastatud
- ☐ DNS kirjete tabel (Osa 6.2)

---

## Hindamine

| Kriteerium | Punktid |
|------------|---------|
| Topoloogia ja ühendused | 2p |
| DHCP konfiguratsioon | 4p |
| DNS konfiguratsioon | 4p |
| Testimine (ping, nslookup) | 3p |
| Veebibrauser test | 2p |
| Screenshots | 3p |
| Küsimused | 4p |
| Troubleshooting | 3p |
| **KOKKU** | **25p** |

---

## Boonusülesanne (+3p)

Lisa DNS serverisse kirje välise veebilehe jaoks:

| Nimi | IP |
|------|-----|
| www.google.lan | 8.8.8.8 |

Testi `nslookup www.google.lan` ja dokumenteeri tulemus.

> **Märkus:** See ei ava tegelikku Google'it (Packet Tracer ei ole ühendatud internetti), aga näitab, et DNS töötab.
