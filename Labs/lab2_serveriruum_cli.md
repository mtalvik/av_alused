# Labor 2: Serveriruum ja CLI põhialused

**Nädal:** 2  
**Aeg:** 45 minutit  
**Cisco moodul:** 2 - Basic Switch and End Device Configuration  
**Eeldused:** Labor 1 sooritatud, Packet Tracer tuttav

## Labori eesmärgid

Selle labori lõppedes oskad:
- Ühendada Cisco seadmega konsooli kaabli kaudu
- Navigeerida Cisco IOS käsureas (CLI) kolmes režiimis
- Seadistada seadme hostname ja banner sõnumit
- Salvestada seadme konfiguratsiooni püsivalt
- Kasutada CLI abistavat funktsionaalsust
- Läbi viia PT Lab 2.1: "Basic Switch Configuration"

## Vajalikud materjalid

**Serveriruu töö:**
- Cisco 2960 lüliti (serveriruu)
- Konsooli kaabel (RJ-45 to USB)
- Laptop terminali tarkvaraga (PuTTY)

**PT võrdlus:**
- Packet Tracer avatud eelmisest laborist
- Labor 1 PT fail

## Samm 1: Füüsilised seadmed ja ühendused (10 minutit)

### 1.1 Cisco 2960 lüliti tutvustus
**Leia oma lüliti serveriruu** (õppejõu juhendamisel):

**Esikülje portid:**
- **24x FastEthernet porti** (Fa0/1 kuni Fa0/24) - 100 Mbps
- **2x GigabitEthernet porti** (Gi0/1, Gi0/2) - 1000 Mbps uplink
- **LED indikaatorid:** System (roheline = töötab), portide staatused

**Tagakülje ühendused:**
- **Console port** (sinine RJ-45) - meie täna kasutame
- **Toitejuhe** - 110-240V AC
- **Ventilaatorid** - kuuled neid töötamas

### 1.2 Konsooli kaabli ühendamine
**1. Leia õige kaabel:**
- **RJ-45 to USB** kaabel (sinine ots)
- **"Rollover" tüüp** - eripärane juhtmete järjestus

**2. Ühenda füüsiliselt:**
- **Lüliti pool:** Console port (sinine RJ-45)
- **Laptop pool:** USB port
- **Kontrolli:** Windows peaks "USB Serial" draiveri leidma

**3. Vaata LED-e:**
- **System LED:** Roheline = lüliti töötab
- **Portide LED-id:** Kollased/rohelised = aktiivsed

## Samm 2: PuTTY ühenduse seadistamine (10 minutit)

### 2.1 PuTTY konfigureerimine
**Käivita PuTTY ja seadista:**

1. **Connection type:** Serial (vali radio button)
2. **Serial line:** COM3 (või mis Windows Device Manager näitab)
3. **Speed (baud):** 9600
4. **Data bits:** 8
5. **Stop bits:** 1
6. **Parity:** None
7. **Flow control:** None

**Kliki "Open"** - avaneb must terminali aken

### 2.2 Esimene kontakt CLI-ga
**Terminali aknas:**
1. **Vajuta Enter mitu korda**
2. Peaksid nägema:
   ```
   Switch>
   ```
3. **Kui midagi ei näe:** Vajuta Ctrl+Break või restart PuTTY

**Kui näed Switch> märki - oled edukalt ühendunud!**

## Samm 3: CLI kolm režiimi (15 minutit)

### 3.1 User EXEC režiim
**Märk:** `Switch>`  
**Võimalused:** Ainult põhilised vaatamise käsud

**Harjutus 1 - Seadme info:**
```bash
Switch> show version
```
**Leia ja märgi üles:**
- IOS-i versioon
- Seadme mudel  
- MAC aadress

**Harjutus 2 - Liideste info:**
```bash
Switch> show interfaces brief
Switch> show ip interface brief
```

### 3.2 Privileged EXEC režiim
**Sisenemine:** `enable` käsk  
**Märk:** `Switch#`  
**Võimalused:** Kõik show käsud + konfiguratsioon

```bash
Switch> enable
Switch# 
```

**Harjutus 3 - Konfiguratsioonide vaatamine:**
```bash
Switch# show running-config
Switch# show startup-config
```

**Võrdle tulemusi:** Mida märkad kahe konfiguratsiooni vahel?

### 3.3 Global Configuration režiim
**Sisenemine:** `configure terminal`  
**Märk:** `Switch(config)#`  
**Võimalused:** Seadme konfigureerimine

```bash
Switch# configure terminal
Switch(config)# 
```

**Pane tähele:** Käsurida muutus! Nüüd saad seadistada.

## Samm 4: Esimesed konfiguratsioonid (10 minutit)

### 4.1 Seadme nime muutmine
```bash
Switch(config)# hostname RUHM[X]-SW1
RUHM[X]-SW1(config)# 
```
**Asenda [X] oma rühma numbriga** (nt. RUHM2-SW1)

**Tulemus:** Käsurida näitab nüüd uut nime!

### 4.2 Banner sõnumi seadistamine
```bash
RUHM[X]-SW1(config)# banner motd #
Enter TEXT message. End with the character '#'.
=== HOIATUS ===
Ainult volitatud kasutajad!
Labor 2 - [Sinu nimi]
===============
#
RUHM[X]-SW1(config)# 
```

### 4.3 Konfiguratsioon testimine
**1. Välju config režiimist:**
```bash
RUHM[X]-SW1(config)# exit
RUHM[X]-SW1# 
```

**2. Vaata oma muudatusi:**
```bash
RUHM[X]-SW1# show running-config
```

**3. Leia oma hostname ja banner seaded**

### 4.4 KRIITILISELT OLULINE - Salvestamine
```bash
RUHM[X]-SW1# copy running-config startup-config
Destination filename [startup-config]? [vajuta Enter]
Building configuration...
[OK]
```

**MIKS OLULINE:** Ilma salvestamata kaob kõik restart korral!

## CLI abistavad funktsioonid

### Tab täiendamine
```bash
Switch# sh[Tab]          → "show"
Switch# show int[Tab]    → "show interfaces"
```

### Käsuajalugu
- **Üles nool (↑):** Eelmine käsk
- **Alla nool (↓):** Järgmine käsk  
- **Ctrl+A:** Rea algusesse
- **Ctrl+E:** Rea lõppu

### Konteksti abi
```bash
Switch# show ?           # Kõik "show" valikud
Switch# show interfaces ?    # "show interfaces" valikud
```

### Veateated
```bash
Switch# shwo version
           ^
% Invalid input detected at '^' marker.
```

## PT Lab 2.1: Basic Switch Configuration

**Nüüd harjuta sama PT-s:**

1. **Ava PT** ja loo uus fail
2. **Lisa 2960 lüliti** workspace-i
3. **Kliki lüliti peal** → "CLI" tab
4. **Korda samu käske:**
   - `enable`
   - `configure terminal`
   - `hostname PT-SW1`
   - `banner motd # Tere PT! #`
   - `copy running-config startup-config`

**Võrdle:** Kuidas on PT lihtsam kui päris seade?

## Kontrolliküsimused

1. **Miks on kolm erinevat CLI režiimi?**
2. **Mis juhtub kui ei salvesta konfiguratsiooni?**
3. **Kuidas erineb running-config startup-config-ist?**
4. **Mida tähendab Switch(config)# märk?**
5. **Millal kasutad Tab klahvi CLI-s?**

## Tõrkeotsing

### Probleem: PuTTY ei näita teksti
**Lahendused:**
- Kontrolli konsooli kaabli ühendusi
- Proovi teist COM porti (Device Manager-ist)
- Vajuta Ctrl+Break
- Restart PuTTY ja proovi uuesti

### Probleem: "% Invalid input" viga
**Lahendused:**
- Kontrolli käsu õigekirja
- Kasuta Tab täiendamist
- Sisesta `?` võimalike valikute nägemiseks
- Veendu õiges režiimis olemises

### Probleem: Ei pääse Privileged režiimi
**Lahendused:**
- Veendu, et sisestasid `enable` õigesti
- Mõnel seadmel võib olla parool (küsi õppejõult)

## Kodutöö

1. **CLI harjutamine PT-s:** Korda täna õpitud käske Packet Traceris
2. **NetAcad Moodul 2:** Lõpeta "Basic Switch and End Device Configuration" 
3. **Uurimine:** Vaata `show version` väljundit - mis info on oluline süsteemiadministraatorile?
4. **Küsimus järgmiseks korraks:** Miks on vaja CLI-d, kui on ka graafiline liides?

## Järgmine nädal

**Nädal 3:** Protokollid ja mudelid + Wireshark sissejuhatus!

**Edu laboriga!** Küsi julgelt abi, kui midagi on ebaselge.
