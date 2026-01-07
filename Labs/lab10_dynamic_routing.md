# Lab 10: Dünaamiline marsruutimine

## Õpiväljundid

Selle labori lõpuks oskad:
- Konfigureerida RIP, OSPF ja EIGRP dünaamilise marsruutimise protokolle
- Kasutada `tracert` käsku marsruudi jälgimiseks
- Selgitada, kuidas dünaamiline marsruutimine reageerib lingi katkemisele
- Võrrelda erinevaid marsruutimise protokolle

---

## Taust

Eelmises laboris kasutasid **staatilist marsruutimist** - lisasid käsitsi `ip route` käsuga marsruudid igale ruuterile. See toimib väikestes võrkudes, aga tekitab probleeme:

- Suurtes võrkudes (50+ ruuterit) on võimatu hallata sadu marsruute käsitsi
- Kui link katkeb, peab administraator käsitsi marsruudid ümber seadistama
- Võrgu muutused nõuavad alati käsitsi sekkumist

**Dünaamiline marsruutimine** lahendab need probleemid:
- Ruuterid õpivad marsruute **automaatselt** üksteiselt
- Kui link katkeb, leiavad ruuterid **automaatselt** uue tee
- Võrgu muutused levivad automaatselt kõigile ruuteritele

---

## Routing Information Protocol (RIP)

**RIP** on üks vanemaid ja lihtsamaid dünaamilise marsruutimise protokolle. RIP on *distance-vector* protokoll - ruuterid jagavad oma naabritega infot: "Mina tean teed võrku X ja see on Y hop-i kaugusel."

**Kuidas RIP töötab:**
1. Ruuter saadab iga **30 sekundi** järel oma naabritele nimekirja võrkudest, mida ta teab
2. Naabrid võrdlevad saadud infot oma tabeliga
3. Kui naaber leiab lühema tee, uuendab ta oma tabelit
4. Uuendused levivad läbi kogu võrgu

**RIP omadused:**

| Omadus | Väärtus |
|--------|---------|
| Metric | **hop count** (mitu ruuterit on teel) |
| Maksimaalne hop count | 15 (16 = unreachable) |
| Uuenduste intervall | 30 sekundit |
| Administrative Distance | 120 |
| Routing table kood | **R** |

---

## Vajalikud seadmed

- 3 x Router (2811)
- 3 x Switch (2960)
- 3 x PC

---

# Osa 1: Võrgu ehitamine

## 1.1 Topoloogia

```
                      [R2]
                     /    \
              Fa0/0 /      \ Fa0/1
    Link A        /        \        Link B
    10.0.0.0/8   /          \       11.0.0.0/8
                /            \
             Fa0/0          Fa0/1
              [R1]----------[R3]
                   Fa1/0  Fa1/0
                    Link C
                   12.0.0.0/8
```

Iga ruuteri küljes on ka LAN võrk:
- R1 → SW1 → PC1 (LAN: 192.168.1.0/24)
- R2 → SW2 → PC2 (LAN: 192.168.2.0/24)
- R3 → SW3 → PC3 (LAN: 192.168.3.0/24)

**Ühendused:**

| Ühendus | Liides ↔ Liides |
|---------|-----------------|
| R1 ↔ R2 | Fa0/0 ↔ Fa0/0 |
| R2 ↔ R3 | Fa0/1 ↔ Fa0/1 |
| R1 ↔ R3 | Fa1/0 ↔ Fa1/0 |
| R1 ↔ SW1 | Fa0/1 ↔ Fa0/1 |
| R2 ↔ SW2 | Fa1/0 ↔ Fa0/1 |
| R3 ↔ SW3 | Fa0/0 ↔ Fa0/1 |
| PC1 ↔ SW1 | Fa0 ↔ Fa0/2 |
| PC2 ↔ SW2 | Fa0 ↔ Fa0/2 |
| PC3 ↔ SW3 | Fa0 ↔ Fa0/2 |

---

## 1.2 IP planeerimine

**Võrgud:**

| Võrk | Võrguaadress | Prefiks |
|------|--------------|---------|
| LAN 1 | 192.168.1.0 | /24 |
| LAN 2 | 192.168.2.0 | /24 |
| LAN 3 | 192.168.3.0 | /24 |
| Link A | 10.0.0.0 | /8 |
| Link B | 11.0.0.0 | /8 |
| Link C | 12.0.0.0 | /8 |

**Täida tabel - Ruuterid:**

> **Vihje:** Link-võrkudes kasuta esimesi IP-aadresse (nt R1 saab .1, R2 saab .2). LAN-võrkudes ruuter saab tavaliselt .1 (gateway).

| Seade | Liides | Võrk | Prefiks | IP-aadress | Alamvõrgumask |
|-------|--------|------|---------|------------|---------------|
| R1 | Fa0/0 | Link A (→R2) | /8 | | |
| R1 | Fa1/0 | Link C (→R3) | /8 | | |
| R1 | Fa0/1 | LAN 1 | /24 | | |
| R2 | Fa0/0 | Link A (→R1) | /8 | | |
| R2 | Fa0/1 | Link B (→R3) | /8 | | |
| R2 | Fa1/0 | LAN 2 | /24 | | |
| R3 | Fa0/1 | Link B (→R2) | /8 | | |
| R3 | Fa1/0 | Link C (→R1) | /8 | | |
| R3 | Fa0/0 | LAN 3 | /24 | | |

**Täida tabel - PC-d:**

| Seade | Võrk | IP-aadress | Alamvõrgumask | Default Gateway |
|-------|------|------------|---------------|-----------------|
| PC1 | LAN 1 | | | |
| PC2 | LAN 2 | | | |
| PC3 | LAN 3 | | | |

---

## 1.3 Konfigureeri võrk

1. Konfigureeri ruuterite hostname: `Perekonnanimi-R1`, `Perekonnanimi-R2`, `Perekonnanimi-R3`
2. Konfigureeri IP aadressid kõigile ruuterite interface-idele
3. Konfigureeri PC-de IP aadressid

---

# Osa 2: Võrgu testimine (enne RIP-i)

Enne dünaamilise marsruutimise lisamist kontrolli, kas baasvõrk töötab.

## 2.1 Testi sama võrgu sees

```
PC1> ping [R1 LAN IP]
```
☐ Töötab

## 2.2 Testi ruuterite vahel

```
Perekonnanimi-R1#ping [R2 Link A IP]
Perekonnanimi-R1#ping [R3 Link C IP]
```
☐ R1 → R2 töötab
☐ R1 → R3 töötab

## 2.3 Testi teise võrku

```
PC1> ping [PC3 IP]
```
☐ **Ei tööta** (pole veel marsruute!)

### ❓ Küsimus 1
Miks ping PC1 → PC3 ei tööta, kuigi ruuterid on omavahel ühendatud?

**Vastus:** _______________________________________________________________

---

# Osa 3: RIP konfigureerimine

## 3.1 RIP käivitamine

Konfigureeri RIP **kõigil kolmel ruuteril**.

> **NB!** `network` käsk kasutab **võrgu ID-d** (võrguaadressi), mitte interface IP-d!
> Näiteks: `network 192.168.1.0`, mitte `network 192.168.1.1`

**R1:**
```
Perekonnanimi-R1(config)#router rip                ! käivita RIP protsess
Perekonnanimi-R1(config-router)#version 2          ! v2 toetab VLSM-i
Perekonnanimi-R1(config-router)#network 192.168.1.0   ! reklaami LAN võrku
Perekonnanimi-R1(config-router)#network 10.0.0.0      ! reklaami Link A
Perekonnanimi-R1(config-router)#network 12.0.0.0      ! reklaami Link C
Perekonnanimi-R1(config-router)#exit
```

**R2:**
```
Perekonnanimi-R2(config)#router rip
Perekonnanimi-R2(config-router)#version 2
Perekonnanimi-R2(config-router)#network _______________
Perekonnanimi-R2(config-router)#network _______________
Perekonnanimi-R2(config-router)#network _______________
Perekonnanimi-R2(config-router)#exit
```

**R3:**
```
Perekonnanimi-R3(config)#router rip
Perekonnanimi-R3(config-router)#version 2
Perekonnanimi-R3(config-router)#network _______________
Perekonnanimi-R3(config-router)#network _______________
Perekonnanimi-R3(config-router)#network _______________
Perekonnanimi-R3(config-router)#exit
```

---

## 3.2 RIP kontrollimine

Oota **~30-60 sekundit**, siis:

```
Perekonnanimi-R1#show ip protocols    ! näita aktiivsed routing protokollid
Perekonnanimi-R1#show ip route        ! näita routing table
```

Otsi **R**-tähega marsruute:

```
R    192.168.2.0/24 [120/1] via 10.0.0.2, 00:00:12, FastEthernet0/0
R    192.168.3.0/24 [120/1] via 12.0.0.2, 00:00:08, FastEthernet1/0
```

- **R** = RIP marsruut
- **[120/1]** = [Administrative Distance / hop count]
- **via 10.0.0.2** = next hop ruuter

### 📸 Screenshot 1
`show ip route` väljund R-koodidega.

> **Märgi ära:** R-tähega marsruudid, [120/X] väärtused, next hop IP-d

### ❓ Küsimus 2
Mis tähendab **[120/1]**?

- 120 = _______________________________________________________________
- 1 = _______________________________________________________________

---

## 3.3 Ping ja Traceroute

```
PC1> ping [PC3 IP]
```
☐ Töötab

```
PC1> tracert [PC3 IP]    ! näita millist teed paketid kasutavad
```

### 📸 Screenshot 2
Traceroute PC1 → PC3

> **Märgi ära:** iga hop (ruuteri IP), hop count kokku

### ❓ Küsimus 3
Millist teed paketid kasutavad? Mitu hop-i on PC1 ja PC3 vahel?

**Vastus:** _______________________________________________________________

---

# Osa 4: Link Failure

Dünaamilise marsruutimise **kõige olulisem omadus** - automaatne taastumine!

## 4.1 Salvesta esialgne tee

```
PC1> tracert [PC3 IP]
```

**Tulemus 1 (enne):**
_______________________________________________________________

## 4.2 Katkesta link R1 ↔ R3

Kustuta kaabel R1-R3 vahel või:
```
Perekonnanimi-R1(config)#interface fa1/0
Perekonnanimi-R1(config-if)#shutdown      ! keela interface
```

## 4.3 Oota ja testi

Oota **~60 sekundit** (RIP peab leidma uue tee).

```
PC1> ping [PC3 IP]
```
☐ Töötab (uue tee kaudu!)

```
PC1> tracert [PC3 IP]
```

**Tulemus 2 (pärast katkemist):**
_______________________________________________________________

### 📸 Screenshot 3
Traceroute pärast lingi katkemist (uus tee läbi R2).

> **Märgi ära:** uus tee (läbi millise ruuteri?), uus hop count

### ❓ Küsimus 4
Millist teed paketid nüüd kasutavad? Miks hop count suurem?

**Vastus:** _______________________________________________________________

## 4.4 Taasta link

```
Perekonnanimi-R1(config)#interface fa1/0
Perekonnanimi-R1(config-if)#no shutdown   ! lülita tagasi sisse
```

Oota ~30 sekundit:

```
PC1> tracert [PC3 IP]
```

**Tulemus 3 (pärast taastamist):**
_______________________________________________________________

### ❓ Küsimus 5
Mis juhtus pärast lingi taastamist?

**Vastus:** _______________________________________________________________

---

# Osa 5: OSPF

## OSPF ülevaade

| Omadus | Väärtus |
|--------|---------|
| Metric | **cost** (põhineb bandwidth-il) |
| Administrative Distance | 110 |
| Routing table kood | **O** |
| Standard | Avatud (kõik tootjad) |

**Wildcard mask** = subnet maski pöördväärtus:
- /24 (255.255.255.0) → **0.0.0.255**
- /8 (255.0.0.0) → **0.255.255.255**

---

## 5.1 RIP eemaldamine

Kõigil ruuteritel:
```
Perekonnanimi-R1(config)#no router rip    ! eemalda RIP
```

## 5.2 OSPF käivitamine

**R1:**
```
Perekonnanimi-R1(config)#router ospf 1                              ! käivita OSPF (1 = process ID)
Perekonnanimi-R1(config-router)#network 192.168.1.0 0.0.0.255 area 0      ! /24 wildcard
Perekonnanimi-R1(config-router)#network 10.0.0.0 0.255.255.255 area 0     ! /8 wildcard
Perekonnanimi-R1(config-router)#network 12.0.0.0 0.255.255.255 area 0
Perekonnanimi-R1(config-router)#exit
```

**R2:**
```
Perekonnanimi-R2(config)#router ospf 1
Perekonnanimi-R2(config-router)#network 192.168.2.0 ___________ area 0
Perekonnanimi-R2(config-router)#network 10.0.0.0 ___________ area 0
Perekonnanimi-R2(config-router)#network 11.0.0.0 ___________ area 0
Perekonnanimi-R2(config-router)#exit
```

**R3:**
```
Perekonnanimi-R3(config)#router ospf 1
Perekonnanimi-R3(config-router)#network 192.168.3.0 ___________ area 0
Perekonnanimi-R3(config-router)#network 11.0.0.0 ___________ area 0
Perekonnanimi-R3(config-router)#network 12.0.0.0 ___________ area 0
Perekonnanimi-R3(config-router)#exit
```

## 5.3 OSPF kontrollimine

```
Perekonnanimi-R1#show ip route          ! otsi O-koode
Perekonnanimi-R1#show ip ospf neighbor  ! näita OSPF naabrid
```

### 📸 Screenshot 4
`show ip route` O-koodidega.

> **Märgi ära:** O-tähega marsruudid, [110/X] väärtused (võrdle RIP-iga!)

### 📸 Screenshot 5
`show ip ospf neighbor`

> **Märgi ära:** naabrite IP-d, State (peaks olema FULL)

### ❓ Küsimus 6
Mis vahe on [120/1] (RIP) ja [110/2] (OSPF)?

**Vastus:** _______________________________________________________________

---

# Osa 6: EIGRP

## EIGRP ülevaade

| Omadus | Väärtus |
|--------|---------|
| Metric | **bandwidth + delay** |
| Administrative Distance | 90 |
| Routing table kood | **D** |
| Standard | Cisco proprietary |

---

## 6.1 OSPF eemaldamine

Kõigil ruuteritel:
```
Perekonnanimi-R1(config)#no router ospf 1
```

## 6.2 EIGRP käivitamine

**AS number (100) peab olema SAMA kõigil ruuteritel!**

**R1:**
```
Perekonnanimi-R1(config)#router eigrp 100         ! 100 = AS number
Perekonnanimi-R1(config-router)#network 192.168.1.0
Perekonnanimi-R1(config-router)#network 10.0.0.0
Perekonnanimi-R1(config-router)#network 12.0.0.0
Perekonnanimi-R1(config-router)#exit
```

**R2:**
```
Perekonnanimi-R2(config)#router eigrp 100
Perekonnanimi-R2(config-router)#network _______________
Perekonnanimi-R2(config-router)#network _______________
Perekonnanimi-R2(config-router)#network _______________
Perekonnanimi-R2(config-router)#exit
```

**R3:**
```
Perekonnanimi-R3(config)#router eigrp 100
Perekonnanimi-R3(config-router)#network _______________
Perekonnanimi-R3(config-router)#network _______________
Perekonnanimi-R3(config-router)#network _______________
Perekonnanimi-R3(config-router)#exit
```

## 6.3 EIGRP kontrollimine

```
Perekonnanimi-R1#show ip route    ! otsi D-koode
```

### 📸 Screenshot 6
`show ip route` D-koodidega.

> **Märgi ära:** D-tähega marsruudid, [90/X] väärtused (võrdle AD väärtusi!)

### ❓ Küsimus 7
Mis on EIGRP Administrative Distance?

**Vastus:** _______________

---

# Osa 7: Võrdlus

**Täida tabel** (vaata tagasi RIP, OSPF, EIGRP ülevaate tabeleid):

| Omadus | RIP | OSPF | EIGRP |
|--------|-----|------|-------|
| Routing table kood | | | |
| Administrative Distance | | | |
| Metric tüüp | | | |
| Kas vajab wildcard maski? | | | |

---

### ❓ Küsimus 8
Kui routing table-s on sama võrgu kohta RIP ja OSPF marsruut, kumba ruuter kasutab?

> **Vihje:** Võrdle Administrative Distance väärtusi. Madalam võidab.

**Vastus:** _______________________________________________________________

### ❓ Küsimus 9
Nimeta 2 eelist dünaamilise marsruutimise kohta.

> **Vihje:** Mõtle, mis juhtus Osa 4 link failure testis.

1. _______________________________________________________________
2. _______________________________________________________________

### ❓ Küsimus 10
Millist protokolli kasutaksid ettevõttes, kus on erinevate tootjate seadmed?

> **Vihje:** Vaata OSPF ja EIGRP tabelitest "Standard" rida.

**Vastus:** _______________________________________________________________

---

## Salvesta

```
Perekonnanimi-R1#copy running-config startup-config
```

Salvesta: `Perekonnanimi_DynamicRouting.pkt`

---

# Esitamine

**Failid:**
1. `Perekonnanimi_DynamicRouting.pkt`
2. `Perekonnanimi_DynamicRouting.pdf`

**Screenshots (6 tk):**
- ☐ Screenshot 1: `show ip route` RIP
- ☐ Screenshot 2: Traceroute enne link failure
- ☐ Screenshot 3: Traceroute pärast link failure
- ☐ Screenshot 4: `show ip route` OSPF
- ☐ Screenshot 5: `show ip ospf neighbor`
- ☐ Screenshot 6: `show ip route` EIGRP

**Küsimused:** 1-10 vastatud

**Tabelid:** IP tabelid + võrdlustabel täidetud

**Traceroute tulemused:** 1, 2, 3

---

## Hindamine

| Kriteerium | Punktid |
|------------|---------|
| Topoloogia ja IP | 3p |
| RIP + screenshots | 3p |
| Link failure test | 4p |
| OSPF + screenshots | 3p |
| EIGRP + screenshots | 3p |
| Küsimused | 5p |
| Võrdlustabel | 2p |
| Dokumentatsioon | 2p |
| **KOKKU** | **25p** |
