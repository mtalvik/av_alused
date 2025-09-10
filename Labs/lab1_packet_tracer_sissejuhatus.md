# Labor 1: Packet Tracer ja esimene võrk

**Nädal:** 1  
**Aeg:** 45 minutit  
**Cisco moodul:** 1 - Networking Today  
**Eeldused:** Arvuti põhioskused

## Labori eesmärgid

Selle labori lõppedes oskad:
- Installeerida ja käivitada Cisco Packet Tracer tarkvara
- Navigeerida PT Logical ja Physical vaadetes
- Luua lihtsat võrku kasutades PT seadmeid (PT Lab 1.1)
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
2. Laadi alla PT installer fail (umbes 300MB)
3. Käivita installer ja järgi juhiseid
4. **NB!** Kasuta NetAcad konto andmeid PT sisselogimiseks

### 1.3 Esimene käivitamine
1. Käivita Packet Tracer
2. Logi sisse oma NetAcad kontoga
3. Tutvu avaekraaniga - näed tööriistade paletti ja workspace'i

## Samm 2: PT Lab 1.1 - Logical and Physical Mode Exploration (15 minutit)

### 2.1 Logical Workspace tutvustus
**Logical vaade** näitab võrgu loogilist struktuuri:
- Kliki "Logical" tab (vaikimisi aktiivne)
- Vaata seadmete paletti ekraani allosas
- Uuri tööriistade riba paremal pool

**Seadmete kategooriad:**
- **Routers** - marsruuterid (1841, 2811, 4321)
- **Switches** - lülitid (2960, 3560)  
- **End Devices** - lõppseadmed (PC, laptop, server)
- **WAN Emulation** - internetiühendused
- **Wireless Devices** - WiFi seadmed

### 2.2 Physical Workspace tutvustus
1. Kliki "Physical" tab
2. Näed "Intercity" vaadet - linnade vahelisi ühendusi
3. **Uuri Physical vaate võimalusi:**
   - Zoom sisse/välja hiire rattaga
   - Lohista vasakult paremale liikumiseks
   - Topeltkliki linnale sisse minemiseks

**Physical vaate tasemed:**
- **Intercity** - linnade vaheline vaade
- **City** - linna vaade (hooned)
- **Building** - hoone vaade (korrused)
- **Floor** - korruse vaade (ruum seadmetega)

### 2.3 Physical ja Logical vaadete erinevused
**Physical vaade:**
- Realistlik 3D keskkond
- Seadmed on ruumides ja riiulites
- Füüsilised kaablid nähtavad
- Hea visualiseerimiseks ja esitluseks

**Logical vaade:**
- Skemaatiline diagramm
- Kiire konfigureerimine
- Kõik seadmed ühel vaatel
- Igapäevane töövahend

## Samm 3: Lihtsa võrgu loomine Logical vaates (15 minutit)

### 3.1 Seadmete lisamine
**Lisa 2 arvutit:**
1. Lülitu tagasi "Logical" vaatesse
2. Kliki "End Devices" ikoonil
3. Vali "PC" (Generic PC)
4. Kliki workspace'is kahte eri kohta
5. Seadmed saavad automaatselt nimed: PC0 ja PC1

**Lisa lüliti:**
1. Kliki "Switches" ikoonil
2. Vali "2960" (Cisco Catalyst 2960)
3. Kliki workspace'i keskele
4. Lüliti saab nime: Switch0

### 3.2 Seadmete ühendamine
**Ühenda arvutid lülitiga:**
1. Kliki "Connections" ikoonil (välgusümbol)
2. Vali "Copper Straight-Through" (sirge vaskkaabel)
3. **PC0 ühendamine:**
   - Kliki PC0 peal
   - Vali "FastEthernet0" port
   - Kliki Switch0 peal  
   - Vali "FastEthernet0/1" port
4. **PC1 ühendamine:**
   - Korda sama: PC1 → Switch0 port Fa0/2

**Tulemused:**
- Näed kollaseid/rohelisi jooni seadmete vahel
- Ühenduste ringikesed muutuvad roheliseks (umbes 30 sekundi pärast)

### 3.3 IP-aadresside konfigureerimine
**PC0 seadistamine:**
1. Kliki PC0 peal
2. Vali "Desktop" tab
3. Kliki "IP Configuration"
4. Vali "Static" radio button
5. Sisesta:
   - **IP Address:** `192.168.1.10`
   - **Subnet Mask:** `255.255.255.0`
   - **Default Gateway:** `192.168.1.1`
6. Sulge aken

**PC1 seadistamine:**
1. Kliki PC1 peal, "Desktop" → "IP Configuration"
2. Vali "Static"
3. Sisesta:
   - **IP Address:** `192.168.1.20`
   - **Subnet Mask:** `255.255.255.0`
   - **Default Gateway:** `192.168.1.1`
4. Sulge aken

## Samm 4: Esimene ping test (5 minutit)

### 4.1 Ühenduvuse testimine
1. Kliki PC0 peal
2. Vali "Desktop" → "Command Prompt"
3. Sisesta käsk: `ping 192.168.1.20`
4. Vajuta Enter

**Oodatav tulemus:**
```
C:\>ping 192.168.1.20

Pinging 192.168.1.20 with 32 bytes of data:

Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128
Reply from 192.168.1.20: bytes=32 time<1ms TTL=128

Ping statistics for 192.168.1.20:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms
```

**Kui ping töötab:** 🎉 Õnnitleme! Oled loonud oma esimese töötava võrgu.

### 4.2 Võrguliikluse visualiseerimine
1. Kliki "Add Simple PDU" tööriista (ümbrik)
2. Kliki PC0 → kliki PC1
3. Vaata animatsiooni paremal pool
4. Kliki "Play" nupule
5. Jälgi, kuidas andmepakett liigub: PC0 → Switch0 → PC1

## Faili salvestamine

1. Vajuta `Ctrl+S` või "File" → "Save As"
2. Anna failile nimi: `Lab1_Esimene_Vork_[SinuNimi].pkt`
3. Salvesta määratud kausta
4. **Kontrolli:** Fail peaks olema .pkt laiendiga

## Kontrolliküsimused

1. **Millised on kolm peamist võrgu komponenti selles laboris?**
2. **Miks kasutame Straight-Through kaablit PC ja Switch vahel?**
3. **Mida tähendab IP-aadress 192.168.1.10/24?**
4. **Mis juhtub, kui annad PC-dele sama IP-aadressi?**
5. **Kuidas erineb Physical vaade Logical vaatest?**

## Tõrkeotsing

### Probleem: Ping ei tööta
**Kontrollimiseks:**
1. Kas kaablid on rohelised? (oota 30-60 sekundit)
2. Kas IP-aadressid on õigesti sisestatud?
3. Kas mõlemad PC-d on samas alamvõrgus? (192.168.1.x)
4. Kas Subnet Mask on 255.255.255.0?

### Probleem: Kaablid jäävad punaseks
- **Oota 1-2 minutit** - PT vajab aega ühenduste tuvastamiseks
- Kliki seadme peal ja vaata "Port Status"
- Proovi eemaldada ja ühendada kaabel uuesti

### Probleem: IP Configuration ei salvesta
- Veendu, et valid "Static" radio button
- Kliki "Apply" nuppe enne akna sulgemist
- Kontrolli õigekirja IP-aadressides

## Kodutöö

1. **NetAcad Moodul 1:** Loe läbi "Networking Today" moodul NetAcad platvormil
2. **PT harjutus lõpetada:** Lisa võrka kolmas PC (IP: 192.168.1.30) ja testi kõigi vahelist ühenduvust
3. **Physical vaate uurimine:** Vaata, kuidas su võrk näeb välja Physical vaates
4. **Küsimus järgmiseks tunniks:** Miks on vaja erinevaid võrgu topoloogiaid? (star, bus, ring, mesh)

## Järgmine nädal

**Nädal 2:** Lähme serveriruumi ja töötame päris Cisco seadmetega! Õpime CLI (käsurida) kasutamist.
