# Arvutivõrkude Alused - 16-nädalane Realistlik Õppekava

**Kursus:** Arvutivõrkude Alused  
**Maht:** 2 EKAP, 90 min/nädal (45+45)  
**Eeldused:** Põhilised arvutioskused  
**Eesmärk:** Mõista võrke praktiliselt, valmistuda CCNA-ks  
**Põhimõte:** PRAKTILINE > teooria, aeglasem tempo = parem õppimine

---

## HINDAMINE

| Komponent | Osakaal | Millal |
|-----------|---------|--------|
| **NetAcad moodulid** | 15% | Pidevalt |
| **Quizid (5x)** | 15% | Iga 2-3 nädala järel |
| **Kontrolltöö #1** | 10% | Nädal 8 |
| **Vahetest** | 15% | Nädal 12 |
| **Laboritööd** | 20% | Lab 6, 7, 8... |
| **Lõpp-Lab** | 25% | Nädal 15-16 |

**Läbimiseks:** 50%  
**Heaks hindeks:** 70%+

---

## BLOKK I: VÕRGU ALUSED (Nädalad 1-6)

### ✅ Nädal 1: Sissejuhatus võrkudesse
**Cisco:** Moodul 1 - Networking Today  
**Aeg:** 45 min teooria + 45 min praktika

**Teooria:**
- Mis on arvutivõrk?
- LAN, WAN, Internet
- Klient-server mudel
- Võrgu komponendid

**Praktika:**
- Packet Tracer installimine
- Esimene võrk: 2 PC + Switch
- Esimene ping test

**Kodutöö:** NetAcad Moodul 1

---

### ✅ Nädal 2: Seadmete põhikonfiguratsioon
**Cisco:** Moodul 2 - Basic Device Configuration  
**Aeg:** 45 min teooria + 45 min praktika

**Teooria:**
- Cisco IOS
- CLI režiimid (User > Privileged > Config)
- Põhikäsud: show, enable, configure terminal

**Praktika (PT):**
- Router/Switch CLI
- Hostname
- Enable secret
- Banner motd

**Kodutöö:** CLI harjutused PT-s

---

### ✅ Nädal 3: Protokollid ja mudelid
**Cisco:** Moodul 3 - Protocols and Models  
**Aeg:** 45 min teooria + 45 min praktika

**Teooria:**
- OSI mudel (7 kihti) - ÜLEVAADE
- TCP/IP mudel (4 kihti)
- Kapseldamine
- PDU-d (segment, packet, frame, bits)

**Praktika (PT):**
- Simulation mode
- Vaatame, kuidas andmed liiguvad
- Protocol inspection

**Kodutöö:** OSI vs TCP/IP võrdlus  
**📝 Quiz #1:** Nädalad 1-3 (10 min, online)

---

### ✅ Nädal 4: Füüsiline kiht
**Cisco:** Moodul 4-5 - Physical Layer  
**Aeg:** 45 min teooria + 45 min praktika

**Teooria:**
- Layer 1 funktsioonid
- Meediatüübid: UTP, fiber, wireless
- Kaablite standardid (T-568A, T-568B)
- Signaalid ja bitid

**Praktika (PT):**
- Kaabli tüüpide tutvustus
- Physical mode exploration
- Straight vs crossover

**Kodutöö:** Kaablite võrdlus

---

### ✅ Nädal 5: Kahendaritmeetika
**Cisco:** Moodul 5 - Number Systems  
**Aeg:** 45 min teooria + 45 min harjutused

**Teooria:**
- Binary vs Decimal
- Binary → Decimal teisendamine
- Decimal → Binary teisendamine
- Hexadecimal (põgusalt)

**Praktika:**
- Harjutused: teisendamised
- Kiired tehted (praktiline)
- MAC/IP aadresside lugemine

**Kodutöö:** Binary harjutused  
**📝 Quiz #2:** Layer 1 + Binary (10 min)

---

### ✅ Nädal 6: OSI Layer 1-2 (PÕHJALIK)
**Cisco:** Moodul 6-7 - Data Link + Ethernet  
**Aeg:** 45 min loeng + 45 min labor

**Loeng:**
- OSI Layer 1 detailselt
- OSI Layer 2 detailselt
- MAC aadressid (struktuur, OUI)
- Ethernet frame
- Switch vs Hub
- MAC address table

**Lab 6:** Ethernet kaablite tegemine
- RJ-45 crimping
- T-568B standard
- Testimine

**Lab 7:** Switch serveris
- ESIMENE KORD SERVERIS!
- Füüsilised ühendused
- Switch reset
- Hostname + paroolid
- MAC address table vaatamine

**Kodutöö:** MAC vs IP erinevused  
**📝 Quiz #3:** Layer 1-2, MAC (15 min, kirjalik)

---

## 📅 BLOKK II: VÕRGUKIHT JA IP (Nädalad 7-10)

### Nädal 7: Võrgukiht (Layer 3) - INTRO
**Cisco:** Moodul 8 - Network Layer  
**Aeg:** 45 min teooria + 45 min praktika

**Teooria:**
- Layer 3 funktsioonid
- IP aadress vs MAC aadress
- Routing põhimõte
- Router vs Switch

**Praktika (PT):**
- Router põhikonfiguratsioon
- IP aadresside lisamine liidestele
- `no shutdown`
- Ping testid

**Kodutöö:** Layer 2 vs Layer 3 võrdlus

---

### Nädal 8: IPv4 Adresseerimine
**Cisco:** Moodul 11 - IPv4 Addressing  
**Aeg:** 45 min teooria + 45 min harjutused

**Teooria:**
- IPv4 struktuuri (32 bitti, 4 oktetti)
- Klassid A, B, C (põgusalt)
- Privaatsed vs avalikud
- Subnet mask põhialused

**Praktika:**
- IP aadresside lugemine
- Network vs host osa
- /8, /16, /24 mask'id
- Harjutused

**Kodutöö:** IP harjutused  
**📋 KONTROLLTÖÖ #1:** Layer 1-3 (45 min, kirjalik)

---

### Nädal 9: Subnetting - BASICS
**Cisco:** Moodul 11 jätk  
**Aeg:** 45 min teooria + 45 min harjutused

**Teooria:**
- Mis on subnetting?
- Miks vaja?
- Lihtne matemaatika: 2^n
- Subnet mask calculation

**Praktika:**
- Ainult LIHTSAD: /8, /16, /24
- Network ID leidmine
- Broadcast address
- Usable IP range

**Kodutöö:** Subnetting harjutused (lihtsad)

---

### Nädal 10: Subnetting - PRACTICE
**Cisco:** Moodul 11 jätk + harjutused  
**Aeg:** 90 min harjutused

**Praktika:**
- Subnetting drillid
- Kiired ülesanded
- Troubleshooting IP problems
- PT: IP planning

**Lab 8 (PT):** IP aadresside planeerimine
- Väike võrk
- 3 subnetti
- Router + 2 Switchi

**Kodutöö:** Rohkem subnetting ülesandeid  
**📝 Quiz #4:** Subnetting (20 min, praktiline)

---

## 📅 BLOKK III: MARSRUUTIMINE JA PROTOKOLLID (Nädalad 11-14)

### Nädal 11: VLSM (Variable Length Subnet Mask)
**Cisco:** Moodul 11 advanced  
**Aeg:** 45 min teooria + 45 min harjutused

**Teooria:**
- Mis on VLSM?
- Miks parem kui fixed subnetting?
- Aadresside säästmine
- VLSM strateegia

**Praktika:**
- VLSM harjutused
- Efektiivne IP planeerimine
- Real-world näited

**Kodutöö:** VLSM planning

---

### Nädal 12: Routing ja ARP
**Cisco:** Moodul 9-10 - ARP + Routing Basics  
**Aeg:** 45 min teooria + 45 min praktika

**Teooria:**
- ARP protokoll (Layer 2 ↔ Layer 3)
- ARP table
- Static routing
- Default gateway
- Routing table

**Praktika (PT):**
- Static route'ide lisamine
- Default route
- ARP table vaatamine
- Troubleshooting

**📋 VAHETEST:** Kõik kuni siia (90 min)
- 45 min teooria (OSI, IP, Subnetting, Routing)
- 45 min praktiline (PT ülesanne)

---

### Nädal 13: DHCP
**Cisco:** Moodul 15 osa - DHCP  
**Aeg:** 45 min teooria + 45 min labor

**Teooria:**
- DHCP toimimine
- DORA protsess (Discover, Offer, Request, Ack)
- DHCP server vs relay
- IP lease time

**Lab 9 (PT):**
- DHCP serveri konfigureerimine
- DHCP pool
- Excluded addresses
- Testimine

**Kodutöö:** DHCP troubleshooting

---

### Nädal 14: DNS
**Cisco:** Moodul 15 osa - DNS  
**Aeg:** 45 min teooria + 45 min labor

**Teooria:**
- DNS toimimine
- DNS hierarchy
- A record, CNAME, MX
- nslookup / dig

**Lab 10 (PT):**
- DNS serveri seadistamine
- Testimine ping hostname
- Troubleshooting DNS issues

**Kodutöö:** DNS + DHCP koostöö  
**📝 Quiz #5:** DHCP + DNS (15 min)

---

## 📅 BLOKK IV: VLANS JA LÕPP-LAB (Nädalad 15-16)

### Nädal 15: VLANs (kui jõuame) + Lõpp-Lab ALGUS
**Cisco:** Moodul 17 osa - VLANs  
**Aeg:** 45 min VLANs + 45 min lõpp-lab planeerimine

**Teooria (kui aega):**
- Mis on VLAN?
- Trunk vs Access port
- Inter-VLAN routing (põgusalt)

**LÕPP-LAB ALGUS:**
- Ülesande tutvustus
- Grupid (2 inimest)
- IP planeerimine
- Topoloogia joonistamine

**Ülesanne:** Ehitada väikeettevõtte võrk serveris
- 2+ subnet
- DHCP (1 pool)
- Static IP (serverid)
- Kõik PC-d peavad pingima
- Router + Switch + 4 PC

---

### Nädal 16: LÕPP-LAB SERVERIS
**Aeg:** 90 min praktiline eksam

**Tegevused:**
- Füüsilised ühendused serveris
- IP konfigureerimine
- DHCP seadistamine
- Testimine
- Troubleshooting
- Dokumenteerimine

**Hindamine:**
- Kas võrk töötab? (60%)
- IP plaan korrektne? (20%)
- Dokumentatsioon? (20%)

**See ON lõpueksam!**

---

## 📚 MATERJALID

**Kohustuslik:**
- Cisco NetAcad konto
- Packet Tracer
- Vihik dokumenteerimiseks

**Soovituslik:**
- "Introduction to Networks" booklet
- Subnetting calculator (alguses)

---

## 🎯 ÕPIVÄLJUNDID

Kursuse lõpus õpilane:

✅ **Teab:**
- OSI mudeli kõik 7 kihti
- MAC vs IP aadresside erinevust
- Subnetting matemaatikat
- VLSM põhimõtteid
- DHCP ja DNS toimimist
- Routing põhialused

✅ **Oskab:**
- Seadistada Cisco switch-i ja routerit
- Planeerida IP aadresside skeemi
- Teha subnetting'ut (sh VLSM)
- Konfigureerida DHCP ja DNS
- Luua static route'e
- Troubleshootida võrguprobleeme
- Töötada päris seadmetega serveris

✅ **On valmis:**
- CCNA 200-301 eksami ettevalmistuseks
- Praktiliseks võrguadministraatoriks
- Edasi õppima (routing protocols, security)

---

## ⏱️ AJAKAVA PAINDLIKKUS

**Kui jääme maha:**
- VLANs → valikuline
- VLSM → lihtsustatud
- PRIORITEET: OSI, IP, Subnetting, DHCP, DNS

**Kui jõuame ette:**
- Transport Layer (TCP/UDP)
- Security basics (SSH, ACL)
- Packet Tracer Skills Assessment

---

*Uuendatud: 2025-10-13*  
*Põhineb tegelikul kursuse käigul, mitte ideaalsel Cisco tempol*