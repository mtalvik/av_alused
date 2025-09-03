# Labor 2: Serveriruum ja CLI põhialused

**Nädal:** 2  
**Aeg:** 45 minutit  
**Cisco moodul:** 2 - Basic Switch and End Device Configuration  
**Eeldused:** Labor 1 sooritatud, Packet Tracer tuttav

## Labori eesmärgid

Selle labori lõppedes oskad:
- Järgida serveriruu ohutusnõudeid ja etiketit
- Ühendada Cisco seadmega konsooli kaabli kaudu
- Navigeerida Cisco IOS käsureas (CLI)
- Eristada käsurea režiime (User EXEC, Privileged EXEC, Global Config)
- Seadistada seadme hostname ja banner sõnumit
- Salvestada seadme konfiguratsiooni
- Kasutada CLI abistavat funktsionaalsust

## Vajalikud materjalid

- Cisco 2960 lüliti (serveriruu)
- Konsooli kaabel (RJ-45 to DB-9 või USB)
- Arvuti terminali tarkvaraga (PuTTY, Tera Term)
- Notepad märkmete tegemiseks
- **TÄHTIS:** Kandke suletud jalatseid serveriruu

## OHUTUSNÕUDED JA ETIKETT

### Serveriruu ohutusnõuded
- **Kandke ALATI suletud jalatseid** (mitte sandaalid)
- **Ärge kunagi lülitage seadmeid välja** ilma õppejõu loata
- **Ärge eemaldage kaableid** teiste seadmete küljest
- **Liigutage ettevaatlikult** - serveriruu on kitsa ruumiga
- **Tõstke rasket kabinetit KOOS** - mitte üksinda
- **Hädaolukord:** Väljalüliti on ukse kõrval

### Serveriruu etikett
- **Vaikne töö** - teised rühmad töötavad samuti
- **Puhas töökoht** - jätke kõik algsesse kohta
- **Jagamine** - seadmed on kõigi jaoks
- **Küsimised** - küsige julgelt, kui midagi pole selge

## Samm 1: Seadmete tutvustus (10 minutit)

### 1.1 Cisco 2960 lüliti uurimine
**Füüsilise seadme vaatlus:**
- **Esiküljel:** 24x FastEthernet porti (Fa0/1 - Fa0/24)
- **Tagaküljel:** Toitejuhe, konsooliport
- **LED-id:** System, RPS, Status igal pordil
- **Ventilaatorid:** Kuulda tagant

**Portide tüübid:**
- **FastEthernet (Fa0/1-24):** 100 Mbps kiirusega
- **GigabitEthernet (Gi0/1-2):** 1000 Mbps uplink portid
- **Console port:** Halduseks (sinine RJ-45 port)

### 1.2 Konsooli kaabli uurimine
**Kaabli tüübid:**
- **RJ-45 to DB-9:** Vanem standard, nõuab adapter
- **RJ-45 to USB:** Uuem, otsene USB ühendus
- **"Rollover" kaabel:** Eripärane juhtmete järjestus

**Ühenduspunktid:**
- **Lüliti pool:** Console port (sinine)
- **Arvuti pool:** COM port või USB

## Samm 2: Ühenduse loomine (10 minutit)

### 2.1 Füüsiline ühendus
1. **Leia serveriruu Cisco 2960 lüliti** (küsi õppejõult)
2. **Ühenda konsooli kaabel:**
   - Lüliti poolne ots: Console porti (sinine RJ-45)
   - Arvuti poolne ots: USB või COM porti
3. **Kontrolli ühendust:** LED-id lülitil peavad põlema

### 2.2 Terminali tarkvara seadistamine
**PuTTY kasutamine (soovitatud):**
1. Käivita PuTTY
2. Vali "Serial" ühenduse tüüp
3. Seaded:
   - **Serial line:** COM3 (või teine, mida Windows näitab)
   - **Speed:** 9600
   - **Data bits:** 8
   - **Stop bits:** 1
   - **Parity:** None
   - **Flow control:** None
4. Kliki "Open"

**Alternatiiv - Windows Hyperterminal:**
1. Start → Programs → Accessories → Communications → HyperTerminal
2. Loo uus ühendus nimega "Cisco Console"
3. Vali COM port
4. Sama seaded nagu PuTTY puhul

### 2.3 Esimene ühendus
1. **Vajuta Enter** terminali aknas
2. Peaksid nägema:
   ```
   Switch>
   ```
   Kui midagi ei näe, vajuta Ctrl+Break või restartida ühendus.

## Samm 3: IOS CLI navigeerimine (15 minutit)

### 3.1 User EXEC režiim
**Märk:** `Switch>`

**Põhikäsud:**
```bash
Switch> ?                    # Kõik võimalikud käsud
Switch> show version         # Seadme info
Switch> show interfaces     # Võrgu liideste info
Switch> show ip interface brief  # Lühike liideste ülevaade
```

**Harjutus:**
1. Sisesta `show version` 
2. Leia IOS-i versioon ja seadme mudel
3. Märgi üles MAC aadressi

### 3.2 Privileged EXEC režiim
**Sisenemine:** `enable` käsk  
**Märk:** `Switch#`

```bash
Switch> enable
Switch# 
```

**Uued käsud:**
```bash
Switch# show running-config    # Aktiivne konfiguratsioon
Switch# show startup-config    # Salvestatud konfiguratsioon
Switch# copy running-config startup-config  # Konfiguratsiooni salvestamine
```

**Harjutus:**
1. Mine Privileged EXEC režiimi
2. Vaata `show running-config`
3. Märka erinevust User EXEC-iga

### 3.3 Global Configuration režiim
**Sisenemine:** `configure terminal`  
**Märk:** `Switch(config)#`

```bash
Switch# configure terminal
Switch(config)# 
```

**Konfiguratsioon käsud:**
```bash
Switch(config)# hostname [nimi]           # Seadme nime muutmine
Switch(config)# banner motd #[sõnum]#     # Tervitussõnum
Switch(config)# enable password [parool]  # Privileged EXEC parool
Switch(config)# exit                      # Tagasi eelmisse režiimi
```

## Samm 4: Põhikonfiguratsioon (10 minutit)

### 4.1 Hostname seadistamine
```bash
Switch# configure terminal
Switch(config)# hostname Labor2-SW1
Labor2-SW1(config)# 
```

**Tulemus:** Käsurida muutub - näitab nüüd uut nime!

### 4.2 Banner sõnumi lisamine
```bash
Labor2-SW1(config)# banner motd #
Sisesta sõnum, lõpeta '#'-ga:
HOIATUS: Ainult volitatud kasutajad!
Kooli serveriruu - Labor 2
#
Labor2-SW1(config)# 
```

### 4.3 Parool seadistamine (valikuline)
```bash
Labor2-SW1(config)# enable password cisco
Labor2-SW1(config)# exit
Labor2-SW1# 
```

**Test:** 
1. Sisesta `exit` (tagasi User EXEC)
2. Sisesta `enable` 
3. Peaks küsima parooli

### 4.4 Konfiguratsiooni salvestamine
```bash
Labor2-SW1# copy running-config startup-config
Destination filename [startup-config]? [vajuta Enter]
Building configuration...
[OK]
```

**TÄHTIS:** Ilma salvestamata kaob konfiguratsioon restartiga!

## CLI abistavad funktsioonid

### Tab täiendamine
```bash
Switch# sh[Tab]          # Laiendab "show"-ks
Switch# show int[Tab]    # Laiendab "interfaces"-ks
```

### Käsu ajalugu
- **Üles nool (↑):** Eelmine käsk
- **Alla nool (↓):** Järgmine käsk

### Konteksti abi
```bash
Switch# show ?           # Kõik "show" alamkäsud
Switch# show int ?       # "show interface" valikud
```

### Vea sõnumid
```bash
Switch# shwo version
           ^
% Invalid input detected at '^' marker.
```

## Kontrolliküsimused

1. **Mis on erinevus User EXEC ja Privileged EXEC vahel?**
2. **Kuidas salvestada konfiguratsiooni püsivalt?**
3. **Mida tähendab `Switch(config)#` märk?**
4. **Kuidas väljuda Global Configuration režiimist?**
5. **Mis juhtub kui ei salvesta konfiguratsiooni?**

## Tõrkeotsing

### Probleem: Terminalis pole teksti näha
**Lahendused:**
- Vajuta Enter või Ctrl+Break
- Kontrolli konsooli kaabli ühendusi
- Restartita PuTTY ühendus
- Proovi teist COM porti

### Probleem: Käsud ei tööta
- Veendu, et oled õiges režiimis
- Kasuta Tab täiendamist
- Sisesta `?` käsu konteksti abi saamiseks

### Probleem: "% Invalid input" viga
- Kontrolli käsu õigekirja
- Kasuta `?` võimalike valikute nägemiseks
- Proovi käsk sammhaaval (kasuta Tab)

## PT võrdlus

**Packet Traceris sama tegevus:**
1. Loo lüliti topoloogiasse
2. Kliki lüliti peal → "CLI" tab
3. Same käsud töötavad!

**Erinevused:**
- **Päris seade:** Füüsilised ühendused, aeglasem
- **PT:** Simulatsioon, kiirem, alati kättesaadav

## Kodutöö

1. **Harjutamine:** Korda CLI käske Packet Traceri lülitil
2. **Uurimine:** Loe `show version` väljundi kohta - mis info on oluline?
3. **NetAcad:** Lõpeta Moodul 2 NetAcad platvormil
4. **Küsimus:** Miks on vaja erinevaid käsurežiime? (järgmisel tunnil arutame)

## Järgmine tund

Järgmisel nädalal uurime protokolle ja mudeleid ning kasutame Wireshark tarkvara!