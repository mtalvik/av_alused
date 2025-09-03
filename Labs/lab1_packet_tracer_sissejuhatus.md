# Labor 1: Packet Tracer sissejuhatus ja esimene võrk

**Nädal:** 1  
**Aeg:** 45 minutit  
**Cisco moodul:** 1 - Networking Today  
**Eeldused:** Arvuti põhioskused

## Labori eesmärgid

Selle labori lõppedes oskad:
- Installeerida ja käivitada Cisco Packet Tracer tarkvara
- Navigeerida PT kasutajaliideses (logical ja physical vaated)
- Luua lihtsat võrku kasutades PT seadmeid
- Konfigureerida PC-de IP-aadresse
- Testida ühenduvust ping käsuga
- Salvestada ja avada PT failid

## Vajalikud materjalid

- Arvuti Windows 10/11, macOS või Ubuntu
- Internetiühendus (Packet Tracer allalaadimiseks)
- Cisco NetAcad konto (õppejõult saadud)
- Umbes 1GB vaba ruumi kõvakettal

## Samm 1: Packet Tracer installeerimine (10 minutit)

### 1.1 NetAcad'i sisselogimine
1. Mine aadressile [netacad.com](https://www.netacad.com)
2. Logi sisse oma konto andmetega (küsi õppejõult kui pole)
3. Navigeeri "Resources" → "Download Packet Tracer"

### 1.2 Allalaadimine ja installeerimine
1. Vali oma operatsioonisüsteemile sobiv versioon
2. Laadi alla PT installer fail (umbes 200MB)
3. Käivita installer ja järgi juhiseid
4. **NB!** Kasuta NetAcad konto andmeid PT sisselogimiseks

### 1.3 Esimene käivitamine
1. Käivita Packet Tracer
2. Logi sisse oma NetAcad kontoga
3. Tutvugus avaekraaniga - näed menüüd ja töövahendeid

## Samm 2: PT kasutajaliides (10 minutit)

### 2.1 Põhi-vaated
PT-l on kaks peamist vaadet:

**Logical Workspace (loogiline vaade):**
- Näitab võrgu skeemi/topoloogiat
- Siin konfigureerid seadmeid ja ühendusi
- Kasutad enamiku ajast seda vaadet

**Physical Workspace (füüsiline vaade):**
- Näitab seadmeid ruumides/riiulites nagu päris serveriruum
- Saad liigutada seadmeid, vaadata kaableid
- Hea visualiseerimiseks

### 2.2 Seadmete palett
Ekraani allosas on seadmete palett:
- **Routers** - marsruuterid 
- **Switches** - lülitid/kommutaatorid
- **End Devices** - arvutid, telefonid, printerid
- **WAN Emulation** - internetiühenduse simulatsioonid
- **Custom Made Devices** - kohandatud seadmed

### 2.3 Tööriistad
Paremal pool on tööriistad:
- **Select** - vaikimisi valiku tööriist
- **Move Layout** - elementide liigutamine
- **Place Note** - märkuste lisamine
- **Delete** - elementide kustutamine
- **Inspect** - seadmete detailne uurimine
- **Add Simple PDU** - andmepakettide saatmine

## Samm 3: Esimese võrgu loomine (20 minutit)

### 3.1 Seadmete lisamine (5 min)

**Lisa 2 arvutit:**
1. Kliki "End Devices" ikoonil
2. Vali "PC" (Generic PC)
3. Kliki workspace'is kahte eri kohta
4. Näed nüüd kahte arvutit: PC0 ja PC1

**Lisa lüliti:**
1. Kliki "Switches" ikoonil
2. Vali "2960" (Cisco 2960 Switch)
3. Kliki workspace'i keskele
4. Näed lülitit: Switch0

### 3.2 Seadmete ühendamine (5 min)

**Ühenda arvutid lülitiga:**
1. Kliki "Connections" ikoonil (kaablite sümbol)
2. Vali "Copper Straight-Through" (sirge vaskkaabel)
3. Kliki PC0 peal → vali "FastEthernet0" port
4. Kliki Switch0 peal → vali "FastEthernet0/1" port
5. Korda sama PC1 jaoks: PC1 → Switch0 port Fa0/2

Näed nüüd kollaseid jooni (kaableid) seadmete vahel.

### 3.3 IP-aadresside seadistamine (10 min)

**PC0 konfigureerimine:**
1. Kliki PC0 peal
2. Vali "Desktop" tab
3. Kliki "IP Configuration"
4. Sisesta:
   - IP Address: `192.168.1.10`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.1.1` (praegu tühi, aga hiljem vaja)
5. Sulge aken

**PC1 konfigureerimine:**
1. Kliki PC1 peal
2. Korda samu samme
3. Sisesta:
   - IP Address: `192.168.1.20`  
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.1.1`
4. Sulge aken

## Samm 4: Ühenduvuse testimine (5 minutit)

### 4.1 Ping test
1. Kliki PC0 peal
2. Vali "Desktop" → "Command Prompt"
3. Sisesta käsk: `ping 192.168.1.20`
4. Peaksid nägema vastust:
   ```
   Pinging 192.168.1.20 with 32 bytes of data:
   Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
   Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
   ```

**Kui ping töötab:** Õnnitleme! Oled loonud oma esimese töötava võrgu.

**Kui ping ei tööta:** Vaata tõrkeotsingut all pool.

### 4.2 PDU (Protocol Data Unit) uurimine
1. Kliki "Add Simple PDU" tööriista (ümbrik)
2. Kliki PC0 → kliki PC1
3. Näed animatsiooni, kuidas andmepakett liigub
4. Kliki "Capture/Forward" nupule korduvalt
5. Vaata, kuidas pakett liigub PC0 → Switch0 → PC1

## Faili salvestamine

1. Vajuta `Ctrl+S` või "File" → "Save"
2. Anna failile nimi: `Lab1_Esimene_Vork_[SinuNimi]`
3. Salvesta kursuse kausta

## Kontrolliküsimused

1. **Mida tähendab Logical Workspace PT-s?**
2. **Milline on erinevus Copper Straight-Through ja Crossover kaabli vahel?**
3. **Miks peavad PC-del olema erinevad IP-aadressid?**
4. **Mida näitab PDU animatsioon?**
5. **Millal muutuvad ühenduste indikaatorid roheliseks?**

## Tõrkeotsing

### Probleem: Ping ei tööta
**Kontrollimiseks:**
- Kas kaablid on õigesti ühendatud? (rohelised ringid seadmete juures)
- Kas IP-aadressid on õigesti sisestatud?
- Kas IP-aadressid on samas võrgus? (192.168.1.x)

### Probleem: Kaablid on punased
- Oota 1-2 minutit - PT vajab aega ühenduste tuvastamiseks
- Või vajuta "Power Cycle Devices" (toite restart)

### Probleem: Ei leia seadmeid palettidest
- Veendu, et oled valinud õige kategooria (End Devices, Switches)
- Kliki kategooria ikoonil uuesti

## Kodutöö

1. **PT täiustamine:** Lisa võrka kolmas PC (IP: 192.168.1.30) ja testi ühenduvust
2. **Uurimine:** Uuri PT Physical Workspace't - kuidas näeb välja serveriruum?
3. **ENNE JÄRGMIST TUNDI - NetAcad kursus:**
   - Mine https://www.netacad.com/courses/networking-basics?courseLang=en-US
   - Loo konto Google kontoga 
   - Liitu "Networking Basics" kursusega
   - **Tee läbi Module 1: Communication in a Connected World**
   - Vaata kõik videod (1.1.1, 1.2.1, 1.3.3)
   - Tee "Check Your Understanding" testid
4. **Küsimus:** Järgmisel tunnil arutame NetAcad Module 1 sisu

## Järgmine tund

Järgmisel nädalal läheme serveriruu ja töötame päris Cisco seadmetega!

---

**Õppejõu märkused:**
- Kontrolli, et kõigil tudengitel PT edukalt installitud
- Aita tõrkeotsinguga, kui kaablid ei muutu roheliseks
- Rõhuta IP-aadresside planeerimise tähtsust
- Tutvu tudengite PT failidega enne järgmist tundi