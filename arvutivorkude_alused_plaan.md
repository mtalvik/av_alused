# Arvutivõrkude Alused - 38h Praktiline Kursus

**Kursus:** Arvutivõrkude Alused  
**Maht:** 38h (19 nädalat × 2h)  
**Eeldused:** Põhilised arvutioskused  
**Eesmärk:** Mõista võrke praktiliselt, valmistuda CCNA-ks  
**Põhimõte:** PRAKTILINE > teooria, max 15-20min loengut, ülejäänud SERVER LAB

---

## KURSUSE SISU

| Nädal | Kuupäev | Tunnid | Teema | Key Points | Teooria | Praktika | Homework | Test/Kontroll | Hindamine |
|-------|---------|--------|-------|------------|---------|----------|----------|---------------|-----------|
| 1 | 04.09.2025 | 2h | Sissejuhatus võrkudesse | **Network types** (LAN/WAN), **Components** (router, switch, PC), **Physical connections**, **Console access** | 15min: LAN/WAN, komponendid | **105min SERVER LAB**: Füüsiline setup - 2 Switch + 1 Router | NetAcad M1 | | Lab osalemine |
| 2 | 11.09.2025 | 2h | CLI ja seadmete konfig | **Cisco IOS**, **CLI modes** (user/privileged/config), **Basic commands** (enable, conf t, exit), **Passwords**, **Hostname**, **Banner** | 20min: Cisco IOS, CLI režiimid | **100min SERVER LAB**: Hostname, passwords, banner | NetAcad M2 | | Lab osalemine |
| 3 | 25.09.2025 | 2h | OSI ja TCP/IP | **OSI 7 layers**, **TCP/IP 4 layers**, **Encapsulation**, **Show commands** (show version, show running-config) | 15min: OSI 7 kihti, TCP/IP | **105min SERVER LAB**: Show käsud, dokumenteerimine | NetAcad M3 | **QUIZ #1**: CLI, OSI (10min) | 5% |
| 4 | 02.10.2025 | 2h | Füüsiline kiht (L1) | **Cable types** (UTP, Fiber), **Ethernet standards** (100BASE-T, 1000BASE-T), **Cable testing**, **Physical layer troubleshooting** | 15min: Kaablid, standardid | **105min SERVER LAB**: Kaablite kontrollimine | NetAcad M4 | | Lab osalemine |
| 5 | 09.10.2025 | 2h | Kahendaritmeetika | **Binary system**, **Binary ↔ Decimal conversion**, **Powers of 2**, **8-bit octets** | 15min: Binary basics | **105min PAPER**: Binary ↔ Decimal teisendamine | Binary harjutused | | Lab osalemine |
| 6 | 16.10.2025 | 4h | Data Link Layer (L2) | **MAC addresses**, **Ethernet frame**, **Switch operation**, **MAC address table**, **Switch commands** (show mac address-table) | 30min: MAC, Ethernet frame, Switch | **210min SERVER LAB**: MAC table, show käsud | NetAcad M6-7 | **QUIZ #2**: L1-2, MAC, Binary (15min) | 5% |
| 7 | 30.10.2025 | 2h | Võrgukiht intro (L3) | **Layer 3 concept**, **IP vs MAC**, **Router function**, **Default gateway**, **Router interfaces**, **Interface commands** (show ip interface brief) | 15min: Layer 3, IP vs MAC, Router | **105min SERVER LAB**: Router interfaces | NetAcad M8 | | Lab osalemine |
| 8 | 06.11.2025 | 4h | IPv4 adresseerimine | **IPv4 structure** (32-bit), **IP classes** (A/B/C), **Public vs Private IP**, **Subnet mask**, **Network/Broadcast/Host addresses**, **IP planning**, **Interface IP configuration** | 30min: IPv4 struktuur, klassid, masks | **90min PAPER**: IP lugemine + **120min SERVER LAB**: IP planning | NetAcad M11 | | Lab osalemine |
| 9 | 13.11.2025 | 2h | Subnetting basics | **Subnetting concept**, **Formula 2^n-2**, **CIDR notation**, **Network ID**, **Broadcast address**, **Usable host range**, **Basic /24 subnetting** | 15min: Subnetting, 2^n | **105min PAPER**: /24 subnetting, network ID, broadcast | Subnetting harjutused | **CHECK #1**: Server setup, basic config | 10% |
| 10 | 20.11.2025 | 2h | Subnetting practice 1 | **Subnetting drill**, **/25, /26, /27, /28 masks**, **Host calculations**, **Fast subnetting techniques** | 10min: Kordamine | **110min PAPER**: Subnetting drillid | Subnetting harjutused | | Lab osalemine |
| 11 | 27.11.2025 | 2h | Subnetting practice 2 | **Complex subnetting**, **Class A/B subnetting**, **Subnet mask in binary**, **Subnetting troubleshooting** | 15min: Troubleshooting IP | **105min PAPER**: Keerulisemad ülesanded | VLSM reading | | Lab osalemine |
| 12 | 04.12.2025 | 2h | VLSM | **Variable Length Subnet Mask**, **Efficient IP allocation**, **VLSM design**, **Address summarization basics** | 15min: Variable masks | **45min PAPER**: VLSM + **60min SERVER LAB**: IP lisamine | VLSM harjutused | **SUBNETTING TEST** (30min) | 15% |
| 13 | 11.12.2025 | 2h | Router + Routing | **Routing table**, **Connected routes**, **Static routes**, **ip route command**, **Default route** (0.0.0.0/0), **show ip route**, **Packet forwarding** | 15min: Routing table, static routes | **105min SERVER LAB**: Static routes, show ip route | NetAcad M9-10 | | Lab osalemine |
| 14 | 18.12.2025 | 2h | ARP ja Routing jätk | **ARP protocol**, **ARP table**, **show arp**, **MAC-IP mapping**, **Routing troubleshooting**, **Traceroute** | 15min: ARP protokoll | **105min SERVER LAB**: ARP table, troubleshooting | ARP reading | | Lab osalemine |
| 15 | 08.01.2026 | 2h | DHCP | **DHCP concept**, **DORA process** (Discover/Offer/Request/Ack), **DHCP pool configuration**, **ip helper-address**, **DHCP troubleshooting** | 15min: DHCP DORA | **105min SERVER LAB**: DHCP pool config | NetAcad M15 | **CHECK #2**: IP config, routing | 10% |
| 16 | 15.01.2026 | 2h | DNS | **DNS hierarchy**, **DNS resolution**, **DNS server configuration**, **nslookup**, **DNS troubleshooting** | 15min: DNS hierarchy | **105min SERVER LAB**: DNS config, nslookup | NetAcad M15 | | Lab osalemine |
| 17 | 22.01.2026 | 2h | Transport Layer | **TCP vs UDP**, **Port numbers**, **Common ports** (80, 443, 22, 23), **3-way handshake**, **Port testing**, **show ip sockets** | 15min: TCP vs UDP, ports | **105min SERVER LAB**: Port testing | NetAcad M14 | **QUIZ #3**: DHCP, DNS, Routing (15min) | 5% |
| 18 | 29.01.2026 | 2h | Troubleshooting + Final | **Troubleshooting methodology**, **OSI troubleshooting**, **show commands mastery**, **Documentation**, **Network presentation**, **Final project** | 15min: Troubleshooting metoodika | **105min SERVER LAB**: Break and fix, esitlus | Final doc | **FINAL PRESENTATION** | 30% |

**Kokku: 38h**

---

## HINDAMINE (Kokku 100%)

| Komponent | Osakaal | Millal | Kirjeldus |
|-----------|---------|--------|-----------|
| **Quizid** | 15% | Nädalad 3, 6, 17 | Quiz #1 (5%), Quiz #2 (5%), Quiz #3 (5%) |
| **Subnetting Test** | 15% | Nädal 12 | Paper test, 30min |
| **Server Lab Check #1** | 10% | Nädal 9 | Füüsiline setup, basic config |
| **Server Lab Check #2** | 10% | Nädal 15 | IP config, routing |
| **Final Presentation** | 30% | Nädal 18 | Terviklik võrk, dokumentatsioon, esitlus |
| **Lab Osalemine** | 20% | Pidev | Aktiivsus, grupitöö, dokumenteerimine |

**Läbimiseks:** 50%  
**Heaks hindeks:** 70%+

**Homework:** NetAcad moodulid, paper exercises - ei ole kohustuslik, aga lisapunktid (Google Classroom)

---

## 📚 MATERJALID

**Kohustuslik:**
- Cisco NetAcad konto
- Packet Tracer
- Vihik dokumenteerimiseks

**Soovituslik:**
- Subnetting calculator (alguses)
- NetAcad "Introduction to Networks" booklet

---

## 🎯 ÕPIVÄLJUNDID

Kursuse lõpus õpilane:

✅ **Teab:**
- OSI mudeli 7 kihti ja nende praktilist tähtsust
- MAC vs IP aadresside erinevust
- Subnetting matemaatikat ja VLSM põhimõtteid
- DHCP ja DNS toimimist
- Routing põhialused (static routes)

✅ **Oskab:**
- Seadistada Cisco switch-i ja routerit CLI kaudu
- Planeerida IP aadresside skeemi (subnetting, VLSM)
- Konfigureerida DHCP ja DNS
- Luua static route'e
- Troubleshootida võrguprobleeme süstemaatiliselt
- Töötada päris seadmetega serveris (real equipment)

✅ **On valmis:**
- CCNA 200-301 eksami ettevalmistuseks
- Praktiliseks võrguadministraatoriks
- Edasi õppima (routing protocols, VLANs, security)

*Uuendatud: 2025-11-02*  
*Põhineb Cisco NetAcad CCNA ITN struktuuri ja tegelikul kursuse käigul*
