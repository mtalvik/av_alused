# Arvutivõrkude Alused - 16-nädalane õppeplaan

**Kursuse info:** 2 EKAPit, 90 minutit nädalas (45+45 min)  
**Eeldused:** Põhilised arvutioskused, motivatsioon õppida võrgutehnoloogiaid  
**Eesmärk:** Cisco ITN kursuse läbimine ja valmistumine CCNA sertifikaadiks

## Kursuse üldstruktuur

| Blokk | Nädalad | Teema | Cisco moodulid |
|-------|---------|-------|----------------|
| **I** | 1-4 | Võrkude alused ja Ethernet | Moodulid 1-7 |
| **II** | 5-8 | Marsruutimine ja IP-adresseerimine | Moodulid 8-13 |
| **III** | 9-12 | Rakenduste kommunikatsioon | Moodulid 14-15 + kordamine |
| **IV** | 13-16 | Võrguturvalisus ja integratsioon | Moodulid 16-17 + projekt |

---

## Blokk I: Võrkude alused ja Ethernet (Nädalad 1-4)

### Nädal 1: Võrkude sissejuhatus
**Cisco Moodul 1: Networking Today**

**Teooria (45 min):**
- Võrkude roll tänapäeva maailmas
- Võrgu komponendid (hostid, serverid, kliendid)
- Võrgu topoloogiad (star, bus, ring, mesh)
- LAN, WAN, MAN, PAN erinevused

**Praktika (45 min):**
- Cisco Packet Tracer tutvustus ja installeerimine
- PT Lab 1.1: "Logical and Physical Mode Exploration"
- Lihtsa võrgu loomine (2 arvutit + lüliti)
- Esimene ping test

**Kodutöö:** NetAcad Moodul 1 läbi lugeda, PT harjutus lõpetada

---

### Nädal 2: Seadmete konfigureerimine
**Cisco Moodul 2: Basic Switch and End Device Configuration**

**Teooria (45 min):**
- Cisco IOS operatsioonisüsteem
- Käsurea režiimid (User/Privileged/Global Config)
- Põhilised show käsud
- Seadmete nimetamine ja paroolid

**Praktika (45 min):**
- **SERVERIRUUM:** Esimene kord päris seadmetega
- Konsooli kaabli ühendamine
- PT Lab 2.1: "Basic Switch Configuration"
- CLI navigeerimine, hostname seadistamine

**Kodutöö:** CLI käskude harjutamine, NetAcad Moodul 2

---

### Nädal 3: Protokollid ja mudelid
**Cisco Moodul 3: Protocols and Models**

**Teooria (45 min):**
- OSI mudel vs TCP/IP mudel
- Kapseldamine ja PDU-d
- Protokollid (HTTP, FTP, DNS, DHCP)
- Standardiorganisatsioonid (IEEE, IETF)

**Praktika (45 min):**
- PT Lab 3.1: "Protocol and Model"
- Wireshark sissejuhatus (demo)
- Võrguliikluse jälgimine Packet Traceris

**Kodutöö:** Mudelite võrdlus, NetAcad Moodul 3

---

### Nädal 4: Füüsiline kiht ja meedia
**Cisco Moodulid 4-5: Physical Layer + Number Systems**

**Teooria (45 min):**
- Füüsilise kihi funktsioonid
- Võrgumeedia: vask, fiber, wireless
- Kaabelite tüübid ja standardid
- Kahendaritmeetika põhialused

**Praktika (45 min):**
- **LAB:** Ethernet kaabli ehitamine (RJ-45)
- Kaablite testimine
- PT Lab 4.1: "Physical Layer Exploration"

**Kodutöö:** Kahendaritmeetika harjutused

---

## Blokk II: Marsruutimine ja IP-adresseerimine (Nädalad 5-8)

### Nädal 5: Andmelingi kiht
**Cisco Moodulid 6-7: Data Link Layer + Ethernet Switching**

**Teooria (45 min):**
- Data Link Layer funktsioonid
- MAC aadressid ja struktuuri
- Ethernet raamide struktuur
- Kommutaatori töö põhimõtted

**Praktika (45 min):**
- PT Lab 6.1: "Data Link Layer"
- MAC-aadresside tabel uurimine
- Kommutaatori konfigureerimine serveris

**Kodutöö:** MAC vs IP aadresside erinevused

---

### Nädal 6: Võrgu kiht
**Cisco Moodul 8: Network Layer**

**Teooria (45 min):**
- Network Layer funktsioonid
- IPv4 ja IPv6 protokollid
- Marsruutimise põhimõtted
- Ruuteri operatsioonid

**Labor 6 (45 min):** Ruuteri põhikonfiguratsioon
- PT Lab 6.1: "Network Layer and Routing"
- **SERVERIRUUM:** Ruuteri liideste konfigureerimine
- IP-aadresside määramine liidestele
- Basic routing table uurimine

**Kodutöö:** Marsruutimistabelite analüüs

---

### Nädal 7: IPv4 adresseerimine
**Cisco Moodul 11: IPv4 Addressing**

**Teooria (45 min):**
- IPv4 aadressi struktuur
- Aadressi klassid (A, B, C)
- Subnet Mask ja CIDR
- Privaatsed vs avalikud aadressid

**Praktika (45 min):**
- PT Lab 11.1: "IPv4 Addresses and Subnets"
- Subnetting harjutused
- IP-aadresside planeerimine

**Kodutöö:** Subnetting ülesanded

---

### Nädal 8: ARP ja ruuteri konfiguratsioon
**Cisco Moodulid 9-10: Address Resolution + Basic Router Configuration**

**Teooria (45 min):**
- ARP protokolli toimimine
- Staatilised marsruudid
- Default gateway kontseptsioon

**Praktika (45 min):**
- PT Lab 9.1: "Address Resolution"
- PT Lab 10.1: "Basic Router Configuration"
- **VAHETEST:** Praktilised oskused

**Kodutöö:** Kordamine vaheeksamiks

---

## Blokk III: Rakenduste kommunikatsioon (Nädalad 9-12)

### Nädal 9: IPv6 ja ICMP
**Cisco Moodulid 12-13: IPv6 Addressing + ICMP**

**Teooria (45 min):**
- IPv6 aadressi formaadid
- IPv6 konfigureerimine
- ICMP protokoll
- Ping ja traceroute käsud

**Praktika (45 min):**
- PT Lab 12.1: "IPv6 Configuration"
- Võrgu testimine ping/traceroute
- **VAHEEKSAM:** Teooria test

---

### Nädal 10: Transpordikiht
**Cisco Moodul 14: Transport Layer**

**Teooria (45 min):**
- TCP vs UDP
- Pordinumbrid ja multipleksing
- Three-way handshake
- Flow control ja error detection

**Praktika (45 min):**
- PT Lab 14.1: "Transport Layer"
- Wireshark TCP analüüs
- Pordidnimede uurimine

---

### Nädal 11: Rakenduste kiht
**Cisco Moodul 15: Application Layer**

**Teooria (45 min):**
- HTTP/HTTPS protokollid
- DNS toimimine
- DHCP konfigureerimine
- E-posti protokollid (SMTP, POP3, IMAP)

**Praktika (45 min):**
- PT Lab 15.1: "Application Protocols"
- DNS serveri seadistamine
- DHCP konfigureerimine

---

### Nädal 12: Integratsioon ja kordamine
**Kordamine + praktilised harjutused**

**Teooria (45 min):**
- Kõigi kihtide koostöö
- Andmete liikumine võrgus
- Tõrkeotsingud

**Praktika (45 min):**
- **SUUR LABOR:** Kompleksse võrgu ehitamine PT-s
- Kõigi oskuste integreerimine

---

## Blokk IV: Võrguturvalisus ja projekt (Nädalad 13-16)

### Nädal 13: Võrguturvalisuse alused
**Cisco Moodul 16: Network Security Fundamentals**

**Teooria (45 min):**
- Võrgu turvaohud
- Tulemüürid ja ACL-id
- Krüpteerimise põhialused
- Autentimine ja autorisatsioon

**Praktika (45 min):**
- SSH konfigureerimine
- Paroolide turvalisus
- PT Lab 16.1: "Network Security"

---

### Nädal 14: SSH ja kaugkonfiguratsioon
**Jätk Cisco Moodul 16**

**Teooria (45 min):**
- SSH vs Telnet
- VTY ligipääsu seadistamine
- ACL-ide põhialused

**Praktika (45 min):**
- **SERVERIRUUM:** SSH ühendused
- VTY liinide konfigureerimine
- Kaugligipääsu testimine

---

### Nädal 15: Väikese võrgu ehitamine
**Cisco Moodul 17: Build a Small Network**

**Teooria (45 min):**
- Võrgu disaini põhimõtted
- Redundantsus ja skaleeritavus
- Jõudluse optimeerimine

**Praktika (45 min):**
- **LÕPUPROJEKT:** Väikese ettevõtte võrgu disain
- PT Skills Assessment (PTSA)
- Dokumentatsiooni koostamine

---

### Nädal 16: Projekt ja eksam
**Lõpetamine**

**Esitlused (45 min):**
- Projektide esitlused
- Rühmatöö tagasiside
- Kursuse kokkuvõte

**Lõpueksam (45 min):**
- Teooria test (ITN Final Exam)
- Praktiliste oskuste test
- Kursuse lõpetamine

---

## Hinded ja hindamine

| Komponent | Osakaal | Kirjeldus |
|-----------|---------|-----------|
| **NetAcad moodulid** | 25% | Online testid ja harjutused |
| **Laborid** | 30% | PT harjutused + füüsilised seadmed |
| **Vaheeksam** | 15% | Nädal 9 teoreetiline test |
| **PTSA** | 15% | Packet Tracer Skills Assessment |
| **Lõpueksam** | 15% | ITN Final Exam |

## Nõutud materjalid

- **NetAcad ligipääs:** Cisco kursusele registreerimine
- **Packet Tracer:** Tasuta allalaadimine Cisco NetAcad kaudu
- **Õpik:** "Introduction to Networks Course Booklet" (valikuline)
- **Seadmed:** Koolis olemas (päris Cisco seadmed)

## Soovitused edukaks õppimiseks

1. **Iseseisev töö:** 3-4 tundi nädalas NetAcad platvormil
2. **Packet Tracer:** Igapäevane harjutamine 30-60 minutit
3. **Rühmatöö:** Laboritöödes koostöö teiste tudengitega
4. **Küsimised:** Aktiivselt küsige, kui midagi jääb arusaamatuks
5. **CCNA sertifikaat:** Kursuse lõppedes valmis esimeseks CCNA eksamiks

---

*See õppeplaan järgib Cisco ITN (Introduction to Networks) v7.0 õppekava ja on kohandatud 16-nädalaseks kursuseks Eesti kõrgkoolide jaoks.*