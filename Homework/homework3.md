# Kodutöö: Protokollid ja mudelid

**Tähtaeg:** Järgmine nädal enne loengut  
**Esitamine:** Moodle

---

## Osa 1: NetAcad Module 3 (20 min)

- Logi sisse NetAcad.com
- Vaata üle Module 3: Protocols and Models
- Tee mooduli quiz
- **Screenshot tulemusest** (lisa PDF-i)

---

## Osa 2: OSI ja TCP/IP mudel (15 min)

### Täida tabel:

| OSI kiht | OSI nimi (eesti/inglise) | TCP/IP kiht | Näide |
|----------|--------------------------|-------------|--------|
| 7 | | | HTTP |
| 6 | | | |
| 5 | | (kõik kolm = ?) | |
| 4 | | | TCP |
| 3 | | | |
| 2 | | | Switch |
| 1 | | | |

### Vasta:
1. Mitu kihti on OSI mudelis? ____
2. Mitu kihti on TCP/IP mudelis? ____
3. Kumba kasutab Internet? ____

---

## Osa 3: Packet Tracer ülesanne (25 min)

### Ehita võrk:

```
[PC1]----[Switch]----[PC2]
           |
      [DNS Server]
```

### Seadistus:

**PC1:**
- IP: 192.168.1.10
- Mask: 255.255.255.0
- DNS: 192.168.1.100

**PC2:**
- IP: 192.168.1.20
- Mask: 255.255.255.0
- DNS: 192.168.1.100

**DNS Server:**
- IP: 192.168.1.100
- Mask: 255.255.255.0
- Services → DNS → ON
- Lisa kirje: www.kool.ee = 192.168.1.100

### Testid:
1. PC1 ping PC2 - töötab? ___
2. PC1 ping www.kool.ee - töötab? ___
3. **Screenshot** Simulation Mode'ist kus näha DNS päring

**Salvesta fail:** Kodutoo3_[SinuNimi].pkt

---

## Osa 4: Protokollid (15 min)

### Ühenda protokoll õige pordiga:

| Protokoll | Port | 
|-----------|------|
| HTTP | ___ |
| HTTPS | ___ |
| DNS | ___ |
| DHCP server | ___ |
| FTP | ___ |

**Vihje:** 21, 53, 67, 80, 443

### Vasta:

1. **Mis vahe on TCP ja UDP vahel?** (2-3 lauset)
   _________________________________

2. **Miks DNS kasutab UDP-d?** (1-2 lauset)
   _________________________________

3. **Miks HTTP kasutab TCP-d?** (1-2 lauset)
   _________________________________

---

## Osa 5: Kapseldamine (10 min)

### Järjesta õigesse järjekorda (1-5):

Kui saadad emaili, mis järjekorras info "pakkitakse"?

___ Lisatakse MAC aadressid (Ethernet päis)
___ Sinu email "Tere!" (DATA)
___ Lisatakse IP aadressid
___ Muudetakse bittideks (010101...)
___ Lisatakse TCP port 25 (SMTP)

### PDU nimed:

Ühenda õige kiht õige PDU nimega:

- Layer 4 → _________ (Frame/Segment/Packet)
- Layer 3 → _________ (Frame/Segment/Packet)  
- Layer 2 → _________ (Frame/Segment/Packet)

---

## Osa 6: Mõtteülesanne (5 min)

**Stsenaarium:** DNS server on maas. 

Kas saad:
- [ ] Avada www.google.com?
- [ ] Pingida www.google.com?
- [ ] Pingida 8.8.8.8?
- [ ] Saata emaili?

Selgita lühidalt MIKS: _______________

---

## BOONUS (+2 punkti)

### DHCP lisa

Sama Packet Tracer võrk + lisa:
1. DHCP Server (IP: 192.168.1.50)
2. Seadista DHCP pool (Start: 192.168.1.200)
3. Lisa uus PC3 → DHCP
4. Screenshot kui PC3 saab automaatselt IP

---

## Hindamine

| Osa | Punktid |
|-----|---------|
| NetAcad Module 3 | 20 |
| OSI/TCP-IP tabel | 15 |
| Packet Tracer | 25 |
| Protokollid | 20 |
| Kapseldamine | 15 |
| Mõtteülesanne | 5 |
| **KOKKU** | **100** |

Läbimiseks vaja: **51 punkti**

---

## Esitamine

**Üks PDF fail:**
- Kõik vastused
- Screenshotid
- Failinimi: Perenimi_KT3.pdf

**Packet Tracer fail:**
- Kodutoo3_[Nimi].pkt

---

## Abi:

- Loengu slaidid
- NetAcad Module 3
- YouTube: "DNS Packet Tracer"
- Küsi klassivennal!

**NB! Iseseisev töö!**
