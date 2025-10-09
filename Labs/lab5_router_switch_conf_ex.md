# Lab Ruuter, Switch, DHCP põhikonfiguratsioon
## Labori Eesmärk

Õppida Cisco seadmete põhikäske, lihtsat võrgu konfiguratsiooni ja tutvuda DHCP-ga Packet Tracer keskkonnas.

**Mida õpid:**
- Kuidas ühendada võrguseadmeid
- Ruuteri ja switch'i põhikonfiguratsioon
- Mis on DHCP ja kuidas see töötab
- IP-aadresside määramine (staatiline ja dünaamiline)
- Võrgu testimine ping käsuga
- Routing table põhitõed

---

## Labori Topoloogia

```
                    [Server-DHCP]
                          |
                       Fa0/5
                          |
[PC1]---[PC2]---[SW1]---[R1]---[SW2]---[PC3]---[PC4]
 .10     DHCP    |       |       |      .30    DHCP
              Fa0/1   Gi0/0   Gi0/1   Fa0/1
                     Gi0/1           Gi0/1
              
         192.168.1.0/24      192.168.2.0/24
```

**Vajalikud seadmed Packet Tracer-is:**
- 1x Router (2911)
- 2x Switch (2960)
- 4x PC
- 1x Server
- 8x Straight-through kaabel

---

## OSA 1: Topoloogia Loomine (15 min)

**Eesmärk:** Õpid, kuidas luua võrgutopoloogiat ja ühendada seadmeid Packet Tracer-is.

### Ülesanne 1.1: Lisa Seadmed

| # | Seade | Packet Tracer-is | Nimi |
|---|-------|------------------|------|
| 1 | Router | Network Devices → Routers → 2911 | R1 |
| 2 | Switch | Network Devices → Switches → 2960 | SW1 |
| 3 | Switch | Network Devices → Switches → 2960 | SW2 |
| 4 | PC | End Devices → PC | PC1 |
| 5 | PC | End Devices → PC | PC2 |
| 6 | PC | End Devices → PC | PC3 |
| 7 | PC | End Devices → PC | PC4 |
| 8 | Server | End Devices → Server | Server-DHCP |

**Näpunäide:** Paiguta seadmed arusaadavalt - vasakule Network 1, paremale Network 2, server üleval keskel.

---

### Ülesanne 1.2: Ühenda Seadmed

**Kaablite ühendamine (Copper Straight-Through):**

| Seade A | Port A | Seade B | Port B |
|---------|--------|---------|--------|
| PC1 | FastEthernet0 | SW1 | Fa0/1 |
| PC2 | FastEthernet0 | SW1 | Fa0/2 |
| Server-DHCP | FastEthernet0 | SW1 | Fa0/5 |
| SW1 | **GigabitEthernet0/1** | R1 | Gig0/0 |
| R1 | Gig0/1 | SW2 | **GigabitEthernet0/1** |
| SW2 | Fa0/1 | PC3 | FastEthernet0 |
| SW2 | Fa0/2 | PC4 | FastEthernet0 |

**Oluline:** 
- Kasuta switch'i **GigabitEthernet** porte ruuteri ühenduseks (kiirem: 1 Gbps vs 100 Mbps)
- Oota kuni kõik ühendused muutuvad roheliseks (🟢)
- Kui jäävad oranžiks, oota ~30 sekundit

---

### Ülesanne 1.3: Lisa Oma Nimi Topoloogiasse

1. Vali **Place Note** tööriist (täht "A" ikoon vasakul)
2. Klõpsa topoloogia ülaosas
3. Kirjuta: **"Nimi Perekonnanimi - Labor 1"**

**Miks?** Nii teab õpetaja, et see on sinu töö.

---

## OSA 2: Ruuteri Konfigureerimine (20 min)

**Eesmärk:** Õpid, kuidas seadistada ruuterit ja määrata IP-aadresse interface-idele.

**Mis on Router?** Seade, mis ühendab erinevaid võrke (Network 1 ja Network 2) ja suunab liiklust nende vahel.

### Ülesanne 2.1: R1 Põhikonfiguratsioon

**Kuidas CLI-sse pääseda:**
1. Klõpsa **R1** peal
2. Vali **CLI** tab
3. Vajuta ENTER mitu korda
4. Kui küsib "initial configuration dialog?" → kirjuta **no** ja vajuta ENTER

**R1 CLI:**
```
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# enable secret cisco123
R1(config)# banner motd #Configured by SINU_NIMI#
R1(config)#
```

**Mida tegid:**
- `enable` = Administraatori režiim (User → Privileged)
- `configure terminal` = Seadistusrežiim (kus saad muuta seadeid)
- `hostname R1` = Andsid ruuterile nime "R1"
- `enable secret cisco123` = Parool administraatori režiimi sisenemiseks
- `banner motd` = Tervitussõnum (lisa **OMA NIMI**!)

---

### Ülesanne 2.2: Interface Gig0/0 (Network 1 pool)

```
R1(config)# interface gigabitethernet 0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# description Connection to SW1 - Network1
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)#
```

**Mida tegid:**
- Valisid interface Gig0/0
- Määrasid IP-aadressi 192.168.1.1 (ruuter Network 1 võrgus)
- Lisasid kirjelduse (description)
- **`no shutdown`** = Lülitasid interface SISSE (vaikimisi on väljas!)

**Tähtis:** Ilma `no shutdown` käsuta interface ei tööta!

---

### Ülesanne 2.3: Interface Gig0/1 (Network 2 pool)

```
R1(config)# interface gigabitethernet 0/1
R1(config-if)# ip address 192.168.2.1 255.255.255.0
R1(config-if)# description Connection to SW2 - Network2
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)#
```

**Mida tegid:** Sama, aga Network 2 jaoks (192.168.2.1).

---

### Ülesanne 2.4: Kontrolli ja Salvesta

```
R1# show ip interface brief
R1# write memory
```

**Kontroll:** Mõlemad interface-id peavad olema **up up**. 

```
Interface              IP-Address      Status    Protocol
GigabitEthernet0/0     192.168.1.1     up        up
GigabitEthernet0/1     192.168.2.1     up        up
```

Kui "administratively down" → unustasid `no shutdown`!

**`write memory`** = Salvestab konfiguratsiooni. Ilma selleta kaob restart-il!

---

## OSA 3: DHCP Serveri Seadistamine (15 min)

**Eesmärk:** Õpid, mis on DHCP ja kuidas see automaatselt jagab IP-aadresse.

**Mis on DHCP?** Dynamic Host Configuration Protocol – teenus, mis jagab automaatselt IP-aadresse seadmetele. Kujuta ette: iga kord kui ühendad telefoni WiFi-ga, DHCP annab talle IP-aadressi. Ilma selleta peaks iga seadmele käsitsi IP määrama!

### Ülesanne 3.1: Server Staatiline IP

**Server-DHCP:**
1. Klõpsa **Server-DHCP** peal
2. **Desktop** → **IP Configuration**
3. Täida:

| Säte | Väärtus |
|------|---------|
| IPv4 Address | 192.168.1.100 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |

**Miks staatiline?** Server peab alati olema leitav samal aadressil!

---

### Ülesanne 3.2: DHCP Teenuse Konfigureerimine

**Server-DHCP:**
1. Klõpsa **Services** tab
2. Vali **DHCP**
3. Täida Network 1 jaoks:

| Säte | Väärtus | Selgitus |
|------|---------|----------|
| Pool Name | Network1 | Nimi sellele IP-de kogumile |
| Default Gateway | 192.168.1.1 | Ruuteri aadress Network 1-s |
| DNS Server | **8.8.8.8** | Google DNS (päris töötav DNS) |
| Start IP Address | 192.168.1.10 | Esimene jagamiseks olev IP |
| Subnet Mask | 255.255.255.0 | Võrgu mask |
| Maximum Number of Users | 50 | Kuni 50 seadet saavad IP |

4. Klõpsa **Save**
5. Veendu et **Service = On** (roheline märge)

**Mida tegid:** Lõid "basseini" IP-aadresse (.10 kuni ~.60), mida DHCP saab seadmetele jagada.

**Märkus DNS kohta:** Kasutame `8.8.8.8` (Google DNS), kuna meie server pole päris DNS server. Päris võrgus töötaks see DNS!

---

### Ülesanne 3.3: Lisa Teine DHCP Pool (Network 2)

1. Klõpsa **Add** (uus pool)
2. Täida Network 2 jaoks:

| Säte | Väärtus |
|------|---------|
| Pool Name | Network2 |
| Default Gateway | 192.168.2.1 |
| DNS Server | 8.8.8.8 |
| Start IP Address | 192.168.2.10 |
| Subnet Mask | 255.255.255.0 |
| Maximum Number of Users | 50 |

3. Klõpsa **Save**

**Miks kaks pooli?** Meil on kaks erinevat võrku, igaühel vajab oma IP-aadresside vahemikku.

---

## OSA 4: DHCP Helper Address (10 min)

**Eesmärk:** Õpid, kuidas ruuter aitab DHCP päringuid edastada võrkude vahel.

**Probleem:** DHCP server on Network 1-s (192.168.1.100). Kuidas Network 2 PC-d saavad temalt IP-d?

**Lahendus:** IP Helper Address käsib ruuteril edastada DHCP päringud serverile.

**R1 CLI:**
```
R1# configure terminal
R1(config)# interface gigabitethernet 0/1
R1(config-if)# ip helper-address 192.168.1.100
R1(config-if)# exit
R1(config)# exit
R1# write memory
```

**Kuidas see täpsemalt töötab:**

1. **PC4** (Network 2) karjub: "Ma vajan IP-d!" (DHCP Discover)
2. **Ruuter R1** kuuleb seda Gig0/1 pordis (tänu helper-address)
3. **Ruuter edastab** päringu DHCP serverile Network 1-s (192.168.1.100)
4. **DHCP server** vaatab "Network2" pooli ja valib IP (nt 192.168.2.11)
5. **Server saadab** IP tagasi ruuteri kaudu PC4-le
6. **PC4 saab** oma IP-aadressi!

**Analoogia:** Nagu postkast - kui Network 2 PC karjub "Ma vajan IP-d!", ruuter kuuleb ja edastab selle DHCP serverile Network 1-s, siis toob vastuse tagasi.

**Oluline:** Ilma helper-address'ita DHCP server EI KUULE Network 2 päringuid!

---

## OSA 5: PC-de Konfigureerimine (15 min)

**Eesmärk:** Õpid vahet staatilise ja dünaamilise IP vahel.

### Ülesanne 5.1: PC1 - Staatiline IP

**Mis on Staatiline IP?** Sina määrad IP käsitsi ja see ei muutu kunagi.

**PC1:**
1. Klõpsa **PC1** peal
2. **Desktop** → **IP Configuration**
3. Täida:

| Säte | Väärtus |
|------|---------|
| IPv4 Address | 192.168.1.10 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |

**Millal kasutada?** Serveritele, printeritele, võrguseadmetele - asjad, mida teised peavad leidma.

---

### Ülesanne 5.2: PC2 - DHCP

**PC2:**
1. Klõpsa **PC2** peal
2. **Desktop** → **IP Configuration**
3. Vali **DHCP** (mitte Static!)
4. Oota ~5 sekundit
5. PC2 saab automaatselt IP (näiteks 192.168.1.11)

**Vaata, mis juhtus:** DHCP server andis PC2-le:
- IP-aadressi (nt 192.168.1.11)
- Subnet mask (255.255.255.0)
- Default Gateway (192.168.1.1)
- DNS Server (8.8.8.8)

**Millal kasutada?** Tavalistele seadmetele (laptopid, telefonid) - lihtsam ja kiirem.

---

### Ülesanne 5.3: PC3 - Staatiline IP

**PC3:**

| Säte | Väärtus |
|------|---------|
| IPv4 Address | 192.168.2.30 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.2.1 |

---

### Ülesanne 5.4: PC4 - DHCP

**PC4:**
1. **Desktop** → **IP Configuration** → **DHCP**
2. PC4 saab IP Network 2-st (näiteks 192.168.2.11)

**Märkad?** Kuigi DHCP server on Network 1-s, PC4 sai IP tänu **helper-address**'ile ruuteris!

---

## OSA 6: Switch Konfigureerimine (10 min)

**Eesmärk:** Õpid switch'i põhikonfiguratsiooni.

**Mis on Switch?** Seade, mis ühendab mitu seadet ÜHTE võrku. Switch teab, millises pordis on milline seade (MAC aadressi järgi) ja saadab andmed õigesse kohta.

### Ülesanne 6.1: SW1 Põhikonfiguratsioon

**SW1:**
1. Klõpsa **SW1** peal
2. **CLI** tab
3. Kirjuta:

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW1
SW1(config)# enable secret switch123
SW1(config)# banner motd #SW1 by SINU_NIMI#
SW1(config)# exit
SW1# write memory
```

---

### Ülesanne 6.2: SW2 Põhikonfiguratsioon

**SW2:** Korda sama, aga `hostname SW2` ja `banner motd #SW2 by SINU_NIMI#`

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname SW2
SW2(config)# enable secret switch123
SW2(config)# banner motd #SW2 by SINU_NIMI#
SW2(config)# exit
SW2# write memory
```

---

### Ülesanne 6.3: Vaata Porte (SW1)

```
SW1# show interfaces status
```

**Mida näed:** 
- Millised pordid on **connected** (seadmed ühendatud)
- Millised on **notconnect** (tühjad)
- Nende kiirus (100Mbps või 1000Mbps)
- VLAN info

---

### Ülesanne 6.4: MAC Address Table (SW1)

```
SW1# show mac address-table
```

**Mis on MAC Address Table?** Switch "mäletab", et näiteks PC1 MAC aadress on pordis Fa0/1. Kui keegi tahab PC1-le andmeid saata, switch teab, kuhu.

**Analoogia:** Nagu postiljon, kes teab, et Juku elab majas nr 5 - pole vaja kõiki maju külastada!

**Mida näed tabelis:**
- PC1 MAC aadress → Fa0/1
- PC2 MAC aadress → Fa0/2
- Server MAC aadress → Fa0/5
- R1 MAC aadress → Gi0/1

---

## OSA 7: Testimine (20 min)

**Eesmärk:** Õpid testima võrgu töökindlust.

### Ülesanne 7.1: Ping Testid Network 1 Sees

**Mis on Ping?** Lihtne test: "Hei, kas sa oled seal?" Kui vastab, ühendus töötab!

**PC1:**
1. **Desktop** → **Command Prompt**
2. Kirjuta:

```
C:\> ping 192.168.1.1
C:\> ping 192.168.1.100
C:\> ping 192.168.1.11
```

**Kui töötab:** 
```
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
Reply from 192.168.1.1: bytes=32 time<1ms TTL=255
...
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```
✅ Edukas!

**Kui ei tööta:** 
```
Request timed out.
Request timed out.
...
Packets: Sent = 4, Received = 0, Lost = 4 (100% loss)
```
❌ Kontrolli IP-aadresse ja kaableid!

---

### Ülesanne 7.2: Ping Võrkude Vahel (PC1 → PC3)

**PC1:**
```
C:\> ping 192.168.2.30
```

**Kui see töötab:** Ruuter suunab edukalt pakette Network 1 ↔ Network 2! 🎉

**Mida see tähendab:** 
- Pakett läheb PC1 → SW1 → R1 (Gig0/0 → Gig0/1) → SW2 → PC3
- Ruuter "teab", et 192.168.2.x on Gig0/1 taga
- Tagasi tuleb sama teed

---

### Ülesanne 7.3: Ping Network 2 → Network 1

**PC3:**
```
C:\> ping 192.168.1.10
C:\> ping 192.168.1.100
```

Mõlemad peavad töötama!

---

### Ülesanne 7.4: DHCP Renewal Test

**Mida õpid:** Kuidas DHCP "üürileping" töötab.

**PC2:**
1. **Desktop** → **Command Prompt**
2. Kirjuta:

```
C:\> ipconfig /release
C:\> ipconfig /renew
C:\> ipconfig
```

**Mis juhtus:** 
1. `ipconfig /release` - PC2 "tagastas" IP-aadressi serverile
2. `ipconfig /renew` - PC2 küsis uue IP
3. DHCP server võib anda sama või uue IP
4. `ipconfig` - Vaata uut IP-d

**Analoogia:** Nagu hotellituba - checkout ja siis uue toa küsimine. Võid saada sama või uue.

---

## OSA 8: Lisakäsud ja Routing (15 min)

**Eesmärk:** Õpid, kuidas ruuter "teab", kuhu võrke saata.

### Ülesanne 8.1: Vaata Routing Table

**R1:**
```
R1# show ip route
```

**Mida näed:**
```
Gateway of last resort is not set

     192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.1.0/24 is directly connected, GigabitEthernet0/0
L       192.168.1.1/32 is directly connected, GigabitEthernet0/0
     192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.2.0/24 is directly connected, GigabitEthernet0/1
L       192.168.2.1/32 is directly connected, GigabitEthernet0/1
```

**Mis see tähendab:**

| Märk | Tähendus | Näide |
|------|----------|-------|
| **C** | Connected (otseselt ühendatud võrgud) | 192.168.1.0/24 on Gig0/0 taga |
| **L** | Local (ruuteri enda IP-d) | 192.168.1.1 on Gig0/0 IP |

**Miks oluline:**
- Ruuter "teab", et Network 1 (192.168.1.0/24) on Gig0/0 taga
- Ruuter "teab", et Network 2 (192.168.2.0/24) on Gig0/1 taga
- Kui pakett tuleb 192.168.2.30-le, ruuter saadab selle Gig0/1-sse

**Analoogia:** Nagu kaart, kus on märgitud, et Tartu on lõuna pool ja Tallinn põhja pool.

---

### Ülesanne 8.2: Vaata Running Config

**R1:**
```
R1# show running-config
```

**Mis on Running Config?** Praegu aktiivne konfiguratsioon seadme mälus (RAM). Kui restart, see kaob, kui ei salvesta `write memory` käsuga.

**Mida näed:** 
- Hostname
- Banner (sinu nimega!)
- Interface IP-d
- Helper-address
- Paroolid (krüpteeritud)

**Vaata esimesed 30 rida**, kus on näha:
```
!
version 15.1
!
hostname R1
!
enable secret 5 $1$mERr$...
!
banner motd ^CConfigured by JUKU TAMM^C
!
interface GigabitEthernet0/0
 description Connection to SW1 - Network1
 ip address 192.168.1.1 255.255.255.0
 ip helper-address 192.168.1.100
 duplex auto
 speed auto
!
```

---

## OSA 9: Troubleshooting (15 min)

**Eesmärk:** Õpid vigu leidma ja parandama - IT-inimese kõige tähtsam oskus!

### Ülesanne 9.1: Lõhu Midagi (õppeotstarbel!)

**R1:**
```
R1# configure terminal
R1(config)# interface gigabitethernet 0/1
R1(config-if)# shutdown
R1(config-if)# exit
R1(config)# exit
```

**Mis sa tegid?** Lülitasid Gig0/1 välja (shutdown = lülita välja)

**Testi PC1:**
```
C:\> ping 192.168.2.30
```

**Tulemus:** 
```
Request timed out.
Request timed out.
...
```
❌ Ei tööta!

**Miks?** Lülitasid Gig0/1 välja → Network 2 on nüüd isoleeritud!

---

### Ülesanne 9.2: Diagnoseeri Probleemi

**Kuidas leida viga?**

**R1:**
```
R1# show ip interface brief
```

**Mida näed:**
```
Interface              IP-Address      Status          Protocol
GigabitEthernet0/0     192.168.1.1     up              up
GigabitEthernet0/1     192.168.2.1     administratively down  down
```

**AHA!** Gig0/1 on "administratively down" → Interface on välja lülitatud!

**Õppetund:** Alati kontrolli `show ip interface brief` kui ühendus ei tööta.

---

### Ülesanne 9.3: Paranda Viga

**R1:**
```
R1# configure terminal
R1(config)# interface gigabitethernet 0/1
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# exit
```

**Testi uuesti:**
```
R1# show ip interface brief
```

**Nüüd näed:**
```
Interface              IP-Address      Status    Protocol
GigabitEthernet0/0     192.168.1.1     up        up
GigabitEthernet0/1     192.168.2.1     up        up
```
✅ Mõlemad UP!

**PC1:**
```
C:\> ping 192.168.2.30
```

**Tulemus:** Töötab! ✅

**Õppetund:** `no shutdown` on väga tavaline lahendus! Paljud probleemid tulenevad sellest, et interface on välja lülitatud.

---

## Esitamisnõuded Google Classroomi

### Esita 2 faili:

| # | Fail | Kirjeldus |
|---|------|-----------|
| 1 | **Nimi_Perekonnanimi_Labor1.pkt** | Packet Tracer fail töötava võrguga |
| 2 | **Nimi_Perekonnanimi_Labor1.pdf** | PDF 3 screenshotiga + päis |

**KRIITILINE:** Failinimedes PEAB olema **SINU TÄISNIMI**!

**Näide:** `Juku_Tamm_Labor1.pkt` ja `Juku_Tamm_Labor1.pdf`

---

### PDF Struktuur

**Esimene leht (päis):**
```
=============================================
     CISCO PACKET TRACER LABOR 1
     DHCP ja Routing Põhitõed
=============================================

Nimi: Juku Tamm
Klass: IT-22
Kuupäev: 15.01.2025
Õpetaja: [Õpetaja nimi]

=============================================
```

**Järgnevad lehed - 3 screenshoti:**

---

### Screenshot 1: Topoloogia

**Mida peab sisaldama:**
- ✅ Kõik 8 seadet nähtavad
- ✅ Kõik ühendused rohelised (🟢)
- ✅ Sinu nimi note-na topoloogia üleval (Place Note tööriist)
- ✅ Seadmete nimed (R1, SW1, SW2, PC1-4, Server-DHCP)

**Kuidas teha:**
1. Packet Tracer-is vajuta `Alt + Print Screen` (ainult aktiivne aken)
2. Kleebi Wordis või Paint-is
3. Veendu, et kõik on selgelt näha

**Mida see tõestab:** Sina ehitasid võrgu õigesti ja kõik seadmed on ühendatud.

---

### Screenshot 2: R1 Running Config (Banner)

**R1 CLI:**
```
R1# show running-config
```

**Mida peab sisaldama screenshot:**
- ✅ Esimesed 30-40 rida running-config-st
- ✅ Näha peab olema: `banner motd #Configured by SINU_NIMI#`
- ✅ Hostname R1
- ✅ Interface IP-d (Gig0/0 ja Gig0/1)

**Kuidas teha:**
1. Kirjuta `show running-config`
2. Vajuta `Spacebar` et liikuda alla
3. Tee screenshot esimestest ridadest (kus banner nähtav)

**Mida see tõestab:** Sina konfigreerisid ruuteri ja lisasid oma nime bannerisse.

---

### Screenshot 3: PC1 → PC3 Ping (Võrkude Vahel)

**PC1 Command Prompt:**
```
C:\> ping 192.168.2.30
```

**Mida peab sisaldama screenshot:**
- ✅ Käsk: `ping 192.168.2.30`
- ✅ Tulemus: `Reply from 192.168.2.30...`
- ✅ Statistika: `Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)`
- ✅ Näha peab olema PC1 aken (et teada, kust ping tehti)

**Kuidas teha:**
1. PC1 → Desktop → Command Prompt
2. Kirjuta `ping 192.168.2.30`
3. Oota kuni lõpeb (4 reply-d)
4. Tee screenshot

**Mida see tõestab:** 
- Võrk töötab (ühendus Network 1 ↔ Network 2)
- Ruuter suunab pakette õigesti
- DHCP ja helper-address töötavad

---

## Hindamiskriteeriumid (100p)

| Kriteerium | Punktid | Mida kontrollib õpetaja | Kuidas kontrollida |
|------------|---------|-------------------------|--------------------|
| **Failinimed** | 10p | Sinu nimi failinames (.pkt ja .pdf) | Faili nimi = "Nimi_Perekonnanimi_Labor1.pkt/pdf" |
| **PDF päis** | 5p | Esimene leht sisaldab nime, klassi, kuupäeva | Vaata PDF esimest lehte |
| **Topoloogia** | 20p | 8 seadet, õiged ühendused (Gi0/1!), rohelised tulukesed, note sinu nimega | Screenshot 1 |
| **Ruuteri Config** | 25p | Mõlemad interface-id up/up, helper-address, banner sinu nimega | Screenshot 2 + .pkt fail |
| **DHCP Server** | 15p | 2 pooli (Network1, Network2), Service = On, DNS = 8.8.8.8 | .pkt fail |
| **PC Konfiguratsioon** | 10p | PC2 ja PC4 said DHCP IP-d automaatselt | .pkt fail |
| **Ping Test** | 10p | PC1 → PC3 töötab (0% loss) | Screenshot 3 |
| **Switch Config** | 5p | SW1 ja SW2 hostname, paroolid, banner | .pkt fail |
| **KOKKU** | **100p** | | |

**Miinuspunktid:**
- ❌ -10p: Nimi puudub failinimedes
- ❌ -5p: PDF päis puudub või ebatäielik
- ❌ -5p: Nimi puudub topoloogial (note)
- ❌ -5p: Screenshot'id ei ole selged või puuduvad elemendid

---

## Kiirjuhend - Kõige Olulisemad Käsud

### Ruuter:

| Käsk | Mida teeb | Näide | Režiim |
|------|-----------|-------|--------|
| `enable` | Administraatori režiim | `Router>` → `Router#` | User |
| `configure terminal` | Seadistusrežiim | `Router#` → `Router(config)#` | Privileged |
| `hostname R1` | Määra nimi | | Global Config |
| `banner motd #TEKST#` | Lisa tervitus | `banner motd #by Juku#` | Global Config |
| `interface gig0/0` | Vali interface | | Global Config |
| `ip address IP MASK` | Määra IP | `ip address 192.168.1.1 255.255.255.0` | Interface Config |
| `ip helper-address IP` | DHCP relay | `ip helper-address 192.168.1.100` | Interface Config |
| `description TEKST` | Lisa kirjeldus | `description Connection to SW1` | Interface Config |
| **`no shutdown`** | **Lülita interface SISSE** | **VÄGA OLULINE!** | Interface Config |
| `shutdown` | Lülita interface välja | | Interface Config |
| `exit` | Tagasi üks samm | | Igal pool |
| `end` | Tagasi Privileged režiimi | | Config režiimidest |
| **`show ip interface brief`** | **Vaata interface-sid** | **Kasuta vigu otsides!** | Privileged |
| `show ip route` | Vaata routing table | C = Connected | Privileged |
| `show running-config` | Vaata konfiguratsiooni | Näitab banner, IP-d jne | Privileged |
| **`write memory`** | **Salvesta konfiguratsioon** | **Ära unusta!** | Privileged |

---

### Switch:

| Käsk | Mida teeb | Režiim |
|------|-----------|--------|
| `show interfaces status` | Vaata porte (speed, status, VLAN) | Privileged |
| `show mac address-table` | Vaata MAC aadressi tabelit | Privileged |

---

### PC Command Prompt:

| Käsk | Mida teeb |
|------|-----------|
| `ping IP` | Testi ühendust (Kas seade vastab?) |
| `ipconfig` | Vaata oma IP seadeid |
| `ipconfig /release` | Vabasta DHCP IP tagasi serverile |
| `ipconfig /renew` | Küsi uus DHCP IP |
| `ipconfig /all` | Vaata detailset infot (DNS, DHCP server jne) |

---

## Tüüpilised Vead ja Lahendused

| Probleem | Sümptom | Põhjus | Lahendus | Kontrollimise Käsk |
|----------|---------|--------|----------|-------------------|
| **Interface on "down"** | Ping ei tööta, ühendus puudub | Unustasid `no shutdown` | Lisa `no shutdown` käsk | `show ip interface brief` |
| **PC2 ei saa DHCP IP-d** | DHCP jääb tühjaks | DHCP Service = Off serveris | Lülita serveris DHCP Service ON | Vaata Services → DHCP |
| **PC4 ei saa DHCP IP-d** | DHCP jääb tühjaks Network 2-s | Puudub helper-address | Lisa R1 Gig0/1: `ip helper-address 192.168.1.100` | `show running-config` |
| **Ping Network 1→2 ei tööta** | "Request timed out" | Vale IP, gateway või interface down | Kontrolli IP-sid ja interface-sid | `show ip int brief`, `ipconfig` |
| **Unustasid salvestada** | Pärast restart kõik kadus | Ei teinud `write memory` | Tee alati `wr` pärast muudatusi! | `show startup-config` |
| **Vale kaabel kasutatud** | Punased tulukesed | Kasutasid Crossover või Rollover | Kasuta Copper Straight-Through | Vaata visuaalselt |
| **Oranžid tulukesed** | Ei muutu roheliseks | Ei ole veel valmis | Oota 30 sekundit | Oota natuke |
| **"% Invalid input"** | Käsk ei tööta | Valesti kirjutatud | Kasuta TAB auto-complete | Vajuta TAB |
| **Ei pääse CLI-sse** | Tühi ekraan | Terminal pole aktiivne | Vajuta ENTER mitu korda | - |

---

## Mida Sa Õppisid?

✅ **Võrgu ehitamine:** Kuidas luua topoloogiat ja ühendada seadmeid  
✅ **Ruuteri seadistamine:** IP-d, interface-id, hostname, banner  
✅ **DHCP:** Kuidas automaatne IP jagamine töötab  
✅ **DHCP Relay:** Kuidas helper-address edastab päringuid  
✅ **Staatiline vs Dünaamiline IP:** Millal kasutada kumba  
✅ **Switch:** Kuidas switch ühendab seadmeid ja kasutab MAC tabelit  
✅ **Routing:** Kuidas ruuter "teab", kuhu võrke saata  
✅ **Testimine:** Ping käsu kasutamine  
✅ **Troubleshooting:** Vigade leidmine ja parandamine  

---

## Lisaülesanded (Vabatahtlikud, +10p Boonust)

### Boonusülesanne 1: Lisa kolmas võrk (+5p)

- Lisa **Network 3** (192.168.3.0/24)
- Vajad: 1 switch, 2 PC
- Konfigureeri R1 Gig0/2 interface
- Lisa DHCP pooli "Network3"
- Testi ping Network 1 → Network 3

### Boonusülesanne 2: Telnet ruuterisse (+5p)

- Sea R1-le VTY parool
- Ühenda PC1-st Telnet kaudu R1-sse
- Tee screenshot Telnet ühendusest

**Käsud:**
```
R1(config)# line vty 0 4
R1(config-line)# password telnet123
R1(config-line)# login
R1(config-line)# exit
```

**PC1:**
```
C:\> telnet 192.168.1.1
```

---

**Edu labori sooritamisel!** 

**Kui midagi ei õnnestu:**
1. Loe juhend uuesti
2. Kontrolli `show ip interface brief`
3. Kontrolli DHCP Service = On
4. Küsi õpetajalt abi!

**Meeldetuletus:** Esimesed 5-10 korda on rasked, aga pärast seda hakkab minema lihtsamaks! 🌐🚀