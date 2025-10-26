# Lab 8: Router - Põhiseadistamine

**Eeldused:** Lab 7 (Switch), Loeng Week 6-7 (OSI Layer 1-2)  
**Asukoht:** Klass 310 + Serveriruum  
**Kestus:** 75 min  
**Grupitöö:** 2 inimest

## MIDA ME TÄNA ÕPIME?

### OSI Mudeli Perspektiiv

| Layer | Lab | Õpime |
|-------|-----|-------|
| Layer 1 (Physical) | Lab 6 | Kaablid, elektrisignaalid |
| Layer 2 (Data Link) | Lab 7 | Switch, MAC aadressid |
| **Layer 3 (Network)** | **Lab 8 - TÄNA** | **Router, IP aadressid, interface-id** |

### Põhiküsimus

**Probleem:** Kaks võrku on üksteisest eraldatud. Kuidas nad omavahel suhtlevad?  
**Lahendus:** Router ühendab võrke! Igal võrgul oma IP range, router teab mõlemaid!

> 💡 **Analoogia:** Switch = korterihoone (teab, kes millises korteris). Router = linnade vaheline bussijuht (teab, kuidas Tallinnast Tartusse jõuda)! 🚌

### Täna saame

1. Kuidas router erineb switchist
2. Mis on interface ja miks neil IP aadresse vaja
3. Kuidas seadistada routerit käsitsi
4. Kuidas testida routeri töövõimet

## EESMÄRGID

Selle labori lõpuks oskate:

**Praktiline:**
- Resetida routerit
- Navigeerida routeri CLI-s
- Konfigureerida interface-id IP aadressidega
- Aktiveerida interface-e (`no shutdown`)
- Kontrollida routeri seisundit

**Teoreetiline:**
- Selgitada router vs switch vahet
- Mõista, mis on interface
- Mõista, miks interface vajab IP aadressi
- Lugeda `show ip interface brief` väljundit

## ROUTER vs SWITCH

| Omadus | Switch (Lab 7) | Router (Lab 8) |
|--------|----------------|----------------|
| **OSI Layer** | Layer 2 | Layer 3 |
| **Õpib** | MAC aadresse | IP võrke (routes) |
| **Edastab** | Frame-e (MAC) | Pakette (IP) |
| **Ühendab** | Seadmeid SAMAS võrgus | ERINEVAID võrke |
| **Näide** | Kõik klassikaaslased | Tallinn ↔ Tartu |

> 🏢 **Real world:** Switch = elevator in building (connects floors). Router = highway between cities! 🛣️

## SETUP KIRJELDUS

**Täna AINULT router:**

```
PC1 ──[USB Console]──→ Router Console
```

**TÄHTIS:** Me EI ühenda routerit võrku (switchiga) täna!  
Täna = seadistamine ja testimine.  
Järgmine kord = ühendame kõik kokku!

## DOKUMENTEERIMINE

**Google Docs Template - Classroomist:**

1. Ava Google Classroom → Lab 8
2. Kopeeri template oma Drive'i
3. Jaga grupi liikmega (edit õigused)
4. Täida laabori ajal (koos)
5. Mõlemad esitate Classroomis

**Template struktuur:**

```
LAB 8: ROUTER PÕHISEADISTAMINE
Grupp: ______  Kuupäev: ______  Õpilased: ______

SEADME INFO (screenshot)
├─ Router mudel
├─ IOS versioon
├─ Mälu (RAM, Flash)
└─ Interface-id (loetelu)

KONFIGURATSIOON (screenshot)
├─ Hostname
└─ Banner tekst

INTERFACE-ID ( sreenshot)
└─ show ip interface brief output

ROUTING TABLE
└─ show ip route output (screenshot)

KONTROLLKÜSIMUSED (6 küsimust)
```

---

## OSA 1: FÜÜSILISED ÜHENDUSED (15 min)

### 1.1 Serveriruum - Leia Router

**Variant A:** Võta vaba router rackis

> 🎯 **ProTip:** Routerid on tavaliselt switch-idest suuremad ja neil on VÄHEM porte. Switch = 24-48 porti. Router = 2-4 porti! 🔢

**Google Docs:**
```
Router mudel: Cisco ____
Asukoht: Rack K1 / Klass / Muu
```

### 1.2 Ühenda Toide

```
Router ←[toitekaabel]→ Pistikupesa
```

**LED kontrolli:**
- Toite LED (roheline) = töötab ✅
- Oodake ~30-60 sekundit (boot protsess)

> ⏳ **Patience is virtue:** Router boot on aeglasem kui switch! Ära paanika! Lihtsalt oodata... nagu Windows update... 🐌

### 1.3 Ühenda PC1 (Konsool)

**SAMA nagu Lab 7!** (Vaata Lab 7, OSA 1.5)

```
PC1 USB ←[konsoolikaabel]→ Router Console port
```

**Console porti leidmine:**
- **SININE** RJ45 port
- Märgistus: "CONSOLE" või "CON"
- Tavaliselt TAGANT (mitte nagu switch, kus ees)

**Google Docs:** Console ühendus: ✓

## OSA 2: TERMINAL (10 min)

### 2.1 Leia COM Port

**SAMA nagu Lab 7!** (Vaata Lab 7, OSA 2.1)

Device Manager → Ports (COM & LPT) → USB Serial Port (COM_)

**Google Docs:** COM_____

### 2.2 PuTTY Ühendus

**SAMA nagu Lab 7!** (Vaata Lab 7, OSA 2.2)

| Parameeter | Väärtus |
|------------|---------|
| Connection type | Serial |
| Serial line | COM3 (või sinu number) |
| Speed | 9600 |

### 2.3 Kontrolli

Peaks nägema:
```
Router>
```
või
```
Press RETURN to get started!
```

> 🎮 **Boot screen:** Router boot võtab kauem kui switch - sa näed scrollimist, diagnoostikat, jne. See on NORMAALNE! Chill! ☕

## OSA 3: RESET ROUTER (10 min)

### 3.1 Miks Reset?

Võib-olla eelmine grupp jättis midagi pooleli või konfigureeritud. Me tahame PUHAS START!

### 3.2 Meetod 1: Mode Button (kiirem)

**SAMA nagu Lab 7 switch!**

1. Leia **MODE** või **RESET** nupp (tavaliselt tagant)
2. Hoia all **10 sekundit**
3. LED-id vilguvad
4. Vabasta nupp
5. Oota boot (~60 sek)

**Kui MODE nuppu EI OLE:**

### 3.3 Meetod 2: CLI Käsud (aeglasem)

```
Router> enable
Router# write erase
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm] 
[Enter]

Router# reload
Proceed with reload? [confirm]
[Enter]
```

Oodake 60 sekundit (boot).

**Kontrolli:**
```
Router>
```

Tühi hostname = reset õnnestus! ✅

**Google Docs:** Reset meetod: Mode button / CLI käsud

## OSA 4: DISCOVERY - UURIMiNE (15 min)

**TÄHTIS:** Enne konfiguratsiooni, vaatame, mis meil on!

### 4.1 Versioon ja Mudel

```
Router> enable
Router# show version
```

**Väljund näitab:**

```
Cisco IOS Software, 2800 Software (C2800NM-ADVENTERPRISEK9-M), Version 12.4(15)T1
...
Cisco 2811 (revision 53.51) with 249856K/12288K bytes of memory.
...
Configuration register is 0x2102
```

**Mida otsida:**

| Väli | Näide | Märgi Google Docs |
|------|-------|-------------------|
| IOS Version | 12.4(15)T1 | IOS: ________ |
| Router Model | Cisco 2811 | Mudel: _______ |
| RAM | 249856K (244 MB) | RAM: _______ |
| Flash | varies | Flash: _______ |

> 🔍 **Fun fact:** IOS = Internetwork Operating System (mitte Apple iOS!). Cisco oli esimene! 📱

**Google Docs:** Täida üleval tabel.

### 4.2 Interface-ide Loetelu

**KÕIGE TÄHTSAM KÄSK TÄNA:**

```
Router# show ip interface brief
```

**Väljund näeb välja umbes nii:**

```
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        unassigned      YES unset  administratively down down
FastEthernet0/1        unassigned      YES unset  administratively down down
Serial0/0/0            unassigned      YES unset  administratively down down
Serial0/0/1            unassigned      YES unset  administratively down down
```

**Analüüs:**

| Väli | Tähendus |
|------|----------|
| **Interface** | Pordi nimi (Fa0/0, Fa0/1...) |
| **IP-Address** | `unassigned` = pole veel IP-d |
| **Status** | `administratively down` = admin sulges (default!) |
| **Protocol** | `down` = ei tööta |

> 💡 **Cisco logic:** Router interface-id on DEFAULT VÄLJAS! Switch pordid on DEFAULT SEES! Miks? Security! Router on võrkude vahel = ohtlikum! 🔒

**Google Docs:** Copy-paste väljund VÕI kirjuta interface-ide nimed:

```
Interface-id:
- FastEthernet0/0
- FastEthernet0/1
- (teised, kui on)
```

### 4.3 Kontrolli Füüsiliselt

**Vaata routeri ette/taha:**

Näed porte:
- **FastEthernet** = 100 Mbps (RJ45)
- **GigabitEthernet** = 1000 Mbps (RJ45)
- **Serial** = WAN ühendused (tavaliselt tühi või DB60 connector)

**Märgi üles, mis sul TEGELIKULT on!**

> 🎨 **Router face:** FastEthernet pordid tavaliselt märgistatud **Fa0/0, Fa0/1**. Sama nimetus CLI-s! Cisco armastab järjepidevust! 🎯

**Google Docs:** Füüsilised pordid: Fa0/0 ✓, Fa0/1 ✓, Serial ✓, jne.

## OSA 5: KONFIGURATSIOON (20 min)

### 5.1 Privileged Mode

**SAMA nagu Lab 7 switch!**

```
Router> enable
Router#
```

### 5.2 Global Configuration Mode

```
Router# configure terminal
Router(config)#
```

> 🎯 **Mode check:** Prompt muutub! `>` → `#` → `(config)#`. See on su GPS! 🗺️

### 5.3 Hostname

```
Router(config)# hostname R1-Vikerkaar
R1-Vikerkaar(config)#
```

**Vormindus:**
```
[SEADE]-[GRUPINIMI]
Näiteks: R1-Vikerkaar, R1-Päike, R1-Äike
```

**Google Docs:** Hostname: ___________

### 5.4 Enable Secret

**SAMA nagu Lab 7!**

```
R1-Vikerkaar(config)# enable secret cisco123
```

**Test:**
```
R1-Vikerkaar(config)# exit
R1-Vikerkaar# exit
R1-Vikerkaar> enable
Password: [cisco123]
R1-Vikerkaar#
```

> 🔐 **Security note:** "cisco123" on KOHUTAV parool! Aga ok lab-is. Real world = use strong passwords! 💪🔑

**Google Docs:** Enable secret: cisco123

### 5.5 Console Password

**SAMA nagu Lab 7!**

```
R1-Vikerkaar(config)# line console 0
R1-Vikerkaar(config-line)# password console123
R1-Vikerkaar(config-line)# login
R1-Vikerkaar(config-line)# exit
R1-Vikerkaar(config)#
```

**Google Docs:** Console password: console123

### 5.6 Banner (Message of the Day)

```
R1-Vikerkaar(config)# banner motd #
Enter TEXT message. End with the character '#'.
****************************************************
*  HOIATUS: AUTORISEERIMATA LIGIPÄÄS KEELATUD!    *
*  Grupp: Vikerkaar                                 *
*  See on meie router!                             *
****************************************************
#
R1-Vikerkaar(config)#
```

> 📢 **Fun fact:** Suured ettevõtted kasutavad banner'it juriidiliseks kaitseks. "Unauthorized access prohibited" = sa oled hoiatatud! ⚖️

**Google Docs:** Banner tekst: (kirjuta oma versioon)

---

## OSA 6: INTERFACE KONFIGURATSIOON (20 min)

**UUS OSA - router spetsiifiline!**

### 6.1 Mis on Interface?

**Interface = port routeris, mis ühendab võrke!**

**Erinevalt switchist:**
- Switch port = Layer 2 (MAC)
- Router interface = Layer 3 (IP) ← Vajab IP aadressi!

**Analoogia:**

```
Switch port = postkast (ei vaja aadressi)
Router interface = maja (vajab tänavaadressi)
```

### 6.2 IP Aadresside Plaan

**TÄHTIS:** Kasutame SAMA pordi numbrit nagu Lab 7-s!

**Miks?** Hiljem (2-3 nädala pärast) ühendame routeri switchiga. Router saab GATEWAY võrgule, kus on PC1 ja PC2!

**Meie IP plaan:**

| Interface | Võrk | IP Address | Kasutus |
|-----------|------|------------|---------|
| **Fa0/0** | 192.168.[PORT].0/24 | 192.168.[PORT].1 | Gateway (hiljem switchile) |
| **Fa0/1** | 192.168.[PORT].0/24 | 192.168.[PORT].254 | Testimiseks (täna) |

**Näide - kui sinu port on 21:**

| Interface | IP Address | Subnet Mask |
|-----------|------------|-------------|
| **Fa0/0** | 192.168.21.1 | 255.255.255.0 |
| **Fa0/1** | 192.168.21.254 | 255.255.255.0 |

**Näide - kui sinu port on 23:**

| Interface | IP Address | Subnet Mask |
|-----------|------------|-------------|
| **Fa0/0** | 192.168.23.1 | 255.255.255.0 |
| **Fa0/1** | 192.168.23.254 | 255.255.255.0 |

> 💡 **Gateway = värav:** Fa0/0 IP (.1) saab hiljem PC-de gateway! PC-d kasutavad seda, et jõuda teistesse võrkudesse! 🚪

**Miks mõlemad sama võrgus täna?**

Täna me EI ÜHENDA switchiga veel - lihtsalt konfigureerime ja testima! Sama võrk = saame pingida oma routeri interface-e omavahel!

> 🎯 **Plaan:** Täna = preconfigure. 1-2 nädalat = ühendame switchiga ja PC-d saavad internetti läbi routeri! 🌐

### 6.3 Konfigureeri Fa0/0

**KASUTA OMA PORDI NUMBRIT!**

**Näide - kui port 21:**
```
R1-Vikerkaar(config)# interface fastethernet 0/0
R1-Vikerkaar(config-if)# ip address 192.168.21.1 255.255.255.0
R1-Vikerkaar(config-if)# description Gateway Lab7 võrgule
R1-Vikerkaar(config-if)# no shutdown
R1-Vikerkaar(config-if)#
```

**SINU käsud (asenda [PORT] oma numbriga):**
```
R1-Vikerkaar(config)# interface fastethernet 0/0
R1-Vikerkaar(config-if)# ip address 192.168.[PORT].1 255.255.255.0
R1-Vikerkaar(config-if)# description Gateway Lab7 võrgule
R1-Vikerkaar(config-if)# no shutdown
R1-Vikerkaar(config-if)#
```

**Väljund:**
```
*Mar 1 00:15:23.123: %LINK-3-UPDOWN: Interface FastEthernet0/0, changed state to up
*Mar 1 00:15:24.123: %LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up
```

**Selgitus:**

| Käsk | Tähendus |
|------|----------|
| `interface fastethernet 0/0` | Sisene interface-i konfiguratsioonirežiimi |
| `ip address X.X.X.X Y.Y.Y.Y` | Määra IP ja mask |
| `description Võrk A` | Kirjeldus (valikuline, aga hea tava!) |
| `no shutdown` | **TÄHTIS!** Aktiveerib interface (Cisco default = DOWN!) |

> 🔌 **"no shutdown" on nagu valguse lüliti:** Interface on olemas, aga välja lülitatud. `no shutdown` = lülitame sisse! 💡

**Google Docs:**
```
Fa0/0 IP: 192.168.__.1
Fa0/0 Status: UP
```

### 6.4 Konfigureeri Fa0/1

**TÄPSELT sama protsess!**

**Näide - kui port 21:**
```
R1-Vikerkaar(config)# interface fastethernet 0/1
R1-Vikerkaar(config-if)# ip address 192.168.21.254 255.255.255.0
R1-Vikerkaar(config-if)# description Testimine
R1-Vikerkaar(config-if)# no shutdown
R1-Vikerkaar(config-if)# exit
R1-Vikerkaar(config)#
```

**SINU käsud:**
```
R1-Vikerkaar(config)# interface fastethernet 0/1
R1-Vikerkaar(config-if)# ip address 192.168.[PORT].254 255.255.255.0
R1-Vikerkaar(config-if)# description Testimine
R1-Vikerkaar(config-if)# no shutdown
R1-Vikerkaar(config-if)# exit
R1-Vikerkaar(config)#
```

> 🎯 **Miks .254?** Lihtne meelde jätta! .1 = gateway (Fa0/0), .254 = kõrge number (Fa0/1). .10 ja .20 on juba PC-del! 🔢

**Google Docs:**
```
Fa0/1 IP: 192.168.__.254
Fa0/1 Status: UP
```

### 6.5 Salvesta Konfiguratsioon

**VÄGA TÄHTIS!** Muidu kaob kõik reboot-i peale!

```
R1-Vikerkaar(config)# exit
R1-Vikerkaar# copy running-config startup-config
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
R1-Vikerkaar#
```

**Lühike versioon:**
```
R1-Vikerkaar# copy run start
```

> 💾 **Save like your life depends on it!** Running-config = RAM (ajutine). Startup-config = NVRAM (püsiv). ALATI salvesta! 🆘

**Google Docs:** Konfiguratsioon salvestatud: ✓

---

## OSA 7: KONTROLLIMINE (10 min)

### 7.1 Interface-ide Staatus

```
R1-Vikerkaar# show ip interface brief
```

**Peaks nägema:**

```
Interface              IP-Address      OK? Method Status                Protocol
FastEthernet0/0        192.168.21.1    YES manual up                    up
FastEthernet0/1        192.168.21.254  YES manual up                    up
```

**Kontrolli:**
- ✅ IP-Address = õige (sinu port!)
- ✅ Status = `up` (mitte `administratively down`!)
- ✅ Protocol = `up`

**Google Docs:** Screenshot või copy-paste output.

> 🚦 **Status check:** Mõlemad UP UP = roheline tuli! Head sõita! 🟢

### 7.2 Routing Table

```
R1-Vikerkaar# show ip route
```

**Väljund näitab:**

```
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       ...

Gateway of last resort is not set

C    192.168.21.0/24 is directly connected, FastEthernet0/0
C    192.168.21.0/24 is directly connected, FastEthernet0/1
```

**Analüüs:**

| Väli | Tähendus |
|------|----------|
| **C** | Connected = otse ühendatud võrk |
| **192.168.21.0/24** | Võrk (CIDR notatsioon) |
| **directly connected** | Otse selle routeri küljes |

> 📋 **Routing table = kaart:** Router teab, millised võrgud on tema küljest otse kätte saadavad! Hiljem lisanduvad teiste routerite võrgud! 🗺️

**Google Docs:** Screenshot või copy-paste output.

### 7.3 Ping Test - Router Omavahel

**Pingida Fa0/1 IP-d Fa0/0-lt:**

**Näide - kui port 21:**
```
R1-Vikerkaar# ping 192.168.21.254
```

**SINU käsk:**
```
R1-Vikerkaar# ping 192.168.[PORT].254
```

**Väljund:**
```
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 192.168.21.254, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
```

**Kontroll:**
- ✅ `!!!!!` = 5 edukat pingi
- ✅ Success rate = 100%

**Kui näed `.....` (punktid) = FAIL!**

> 🏓 **Ping-pong:** Router pingib iseennast! Vasakust taskust paremasse! Funny, aga töötab! 😄

**Google Docs:** Ping tulemus: Success / Fail

### 7.4 Detailne Interface Info

**Vaata üksikasju:**

```
R1-Vikerkaar# show interfaces fastethernet 0/0
```

**Näitab:**
- MAC address
- MTU (Maximum Transmission Unit)
- Bandwidth
- Encapsulation
- Statistika (pakette, vigu)

**Lihtsalt vaata läbi!** Palju infot, hiljem õpime täpsemalt!

---

## KONTROLLKÜSIMUSED

**Google Docs:**

1. **Mis on sinu routeri mudel ja IOS versioon?**  
   Vihje: `show version`

2. **Miks router interface-id on vaikimisi DOWN?**  
   Vihje: Security!

3. **Mis on `no shutdown` käsu eesmärk?**

4. **Mis vahe on `running-config` ja `startup-config`?**  
   Vihje: Kus need asuvad? (RAM vs NVRAM)

5. **Mida tähendab "C" routing tabelis (`show ip route`)?**  
   Vihje: Connected

6. **Miks me kasutame .1 ja .254 IP aadresse?**  
   Vihje: Gateway ja testimine

---

## REFLEKTSIOON

**Arutage grupis (Google Docs):**

### Router vs Switch Kogemus

Lab 7: Switch õppis MAC aadresse automaatselt  
Lab 8: Router vajab käsitsi IP konfigureerimist

**Miks?**

Switch = Layer 2 (lihtsam, automaatne)  
Router = Layer 3 (keerulisem, vaja täpset kontrolli)

### Gateway Kontseptsioon

**Fa0/0 = 192.168.[PORT].1**

See saab hiljem PC-de **default gateway**!

```
PC1 konfiguratsioonis (tulevikus):
IP: 192.168.21.10
Mask: 255.255.255.0
Gateway: 192.168.21.1 ← See on meie router!
```

> 🚪 **Gateway = värav teise maailma:** PC-d ei tea, kuidas jõuda internetti. Router teab! PC-d saadavad kõik "võõrad" paketid gateway-le! 🌐

### Järgmine Samm

**Täna:** Router configured ja testimine  
**2-3 nädala pärast:** Ühendame routeri switchiga

```
Future topology:
PC1 ──→ Switch ──→ Router Fa0/0 (.1) ──→ Internet
PC2 ──→        ──→ Router Fa0/1 (.254) ──→ Other network
```

Router hakkab **forwarding** pakette võrkude vahel!

---

## TROUBLESHOOTING

### Probleem: Interface jääb DOWN

**Debug:**

```
R1# show ip interface brief
```

**Kui Status = `administratively down`:**
→ Unustasid `no shutdown`!

**Kui Status = `down` (mitte "administratively"):**
→ Kaabel pole ühendatud VÕI port rikki

**Lahendus:**
```
R1(config)# interface fa0/0
R1(config-if)# no shutdown
```

### Probleem: Ping ei tööta

**Debug järjekorras:**

1. **Kontrolli IP aadresse:**
   ```
   show ip interface brief
   ```
   Kas mõlemad õiged?

2. **Kontrolli routing table:**
   ```
   show ip route
   ```
   Kas C-connected network's näha?

3. **Kas salvestasid konfi?**
   ```
   show running-config
   ```
   vs
   ```
   show startup-config
   ```

> 🔍 **90% of problems:** Unustasid `no shutdown` või ei salvestanud! Check these first! ✅

---

## PUHASTAMINE (5 min)

### Kontrolli Google Docs

- ✓ Seadme info (mudel, IOS, RAM)
- ✓ Interface-id (loetelu)
- ✓ Konfiguratsioon (hostname, passwords, banner)
- ✓ Interface IP-d (Fa0/0, Fa0/1)
- ✓ Screenshots (`show ip interface brief`, `show ip route`)
- ✓ Kontrollküsimustele vastatud
- ✓ Reflektsioon kirjutatud

### Esita

1. Google Classroom → Lab 8
2. Attach Google Doc
3. Turn In

### Lõpeta

**OLULINE:** EI eemalda routeri konfiguratsiooni!

Me kasutame seda 2-3 nädala pärast edasi!

**Salvesta VEEL KORD:**
```
R1# copy run start
```

**Logi välja:**
```
R1# exit
```

**Sulge PuTTY**

**Jäta router SISSE** (toitega) VÕI eemalda ettevaatlikult.

> 🎒 **Pack up:** Konsoolikaabel tagasi kasti. Router tagasi kohale. Tühista kõik, mida sa puutusid! 🧹

---

## 🎁 LISAÜLESANNE

Kui jõudsid siia, oled juba routing guru! 🏆

> 🌐 **Küsi Rainilt:** "Mis oli sinu esimene router?"

Kirjuta vastus Google Docsisse (lisapunktid võimalikud! 🎯)

**Vihje:** Rain on tavaliselt serveriruumi lähedal, klassis 302 või 310, või õpetajate toas.

**Ja ära unusta Raini kiita!** 🌟

---

**Raini esimese routeri lugu:**
```
Mudel: ________________
Aasta: ________________
Lugu (kui Rain jagas): ________________
```

**Kas kiitsid Raini?** ☐ JAH ☐ Unustasin 😅

*(PS: Kui Rain pole kohal, saad küsida järgmisel korral!)* 🔌


## JÄRGMINE KORD

**Lab 9-10 (2-3 nädala pärast):**

```
[PC1] ──┐
        ├──→ [Switch] ──→ [Router Fa0/0] ──→ ???
[PC2] ──┘
```

**Õpime:**
- Default gateway seadistamine PC-del
- Routing kahe võrgu vahel
- NAT (Network Address Translation)
- Internet access läbi routeri!

**Valmistuge:**
- Mäleta oma pordi numbrit!
- Mäleta Lab 7 switch konfiguratsiooni!
- See router on valmis! ✅

---

**Lab 8 DONE! 🎉**

**Õnnitlused - sa oskad nüüd:**
- ✅ Router CLI
- ✅ Interface konfigureerimine
- ✅ IP addressing
- ✅ Basic routing concepts
- ✅ Gateway kontseptsioon

**Next level unlocked!** 🎮