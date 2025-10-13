# Lab 7: Switch ja MAC Table - PACKET TRACER

**Eesmärk:** Simuleerida switch MAC õppimist (sama mis serveris, aga virtuaalselt)  
**Kestus:** 40 min  
**Grupitöö:** 2 inimest (sama paar)

---

## KONTEKST

**Praegu:**
- Grupp A on serveriruum-is (füüsiline switch)
- Grupp B teeb PT-s (virtuaalne switch)

**Peale pausi vahetate!**

**See lab:** Sama loogika mis serveris, aga simulatsioonis.

---

## GOOGLE DOCS

**Ava Google Classroom → Lab 7 → Kopeeri template**

Täida **PT OSA** (serveris täidate server osa hiljem).

---

## OSA 1: TOPOLOOGIA (5 min)

### 1.1 Ava PT ja loo seadmed

**File → New**

**Lisa:**
- 2x PC (End Devices)
- 1x Switch 2960 (Network Devices → Switches)

**Paiguta:**
```
  PC1
   |
 [SW1]
   |
  PC2
```

### 1.2 Ühenda

**Copper Straight-Through kaabel:**

| Ühendus | PC port | Switch port |
|---------|---------|-------------|
| PC1 ↔ SW1 | FastEthernet0 | Fa0/1 |
| PC2 ↔ SW1 | FastEthernet0 | Fa0/2 |

**Oota:** Punased kolmnurgad → rohelised.

**Google Docs:** Screenshot topoloogiast.

---

## OSA 2: IP SEADISTAMINE (5 min)

**Kasuta oma klassi port numbrit!**

**Näide - kui sinu port 21:**

| PC | IP | Mask |
|----|-----|------|
| PC1 | 192.168.21.10 | 255.255.255.0 |
| PC2 | 192.168.21.20 | 255.255.255.0 |

**Kui sinu port 23:**

| PC | IP | Mask |
|----|-----|------|
| PC1 | 192.168.23.10 | 255.255.255.0 |
| PC2 | 192.168.23.20 | 255.255.255.0 |

**Kuidas:**
1. Kliki PC
2. Desktop → IP Configuration
3. Static
4. Sisesta IP ja mask

**Google Docs:**
```
PC1 IP: 192.168.__.10
PC2 IP: 192.168.__.20
```

---

## OSA 3: MAC AADRESSID (3 min)

**Mõlemad PC-d:**

Desktop → Command Prompt
```
ipconfig /all
```

**Kopeeri Physical Address.**

**Google Docs:**
```
PC1 MAC: ________________
PC2 MAC: ________________
```

---

## OSA 4: MAC TABLE (TÜHI) (2 min)

**Switch CLI:**

1. Kliki switch
2. CLI tab
3. Enter
```
Switch> enable
Switch# show mac address-table
```

**Tühi tabel!** Switch ei tea veel midagi.

**Google Docs:** Tühi ✓

---

## OSA 5: PING TEST (3 min)

**PC1 → PC2:**

Desktop → Command Prompt
```
ping 192.168.[SINU_PORT].20
```

**Näide kui port 21:**
```
ping 192.168.21.20
```

**Tulemus:** Reply ✓

---

## OSA 6: MAC TABLE (TÄIS) (5 min)

**Switch CLI:**
```
Switch# show mac address-table
```

**Nüüd näed:**
```
Vlan    Mac Address       Type      Ports
----    -----------       --------  -----
   1    0001.xxxx.xxxx    DYNAMIC   Fa0/1
   1    0002.xxxx.xxxx    DYNAMIC   Fa0/2
```

**Switch õppis!**

**Google Docs:** Screenshot või copy-paste.

---

## OSA 7: SIMULATION MODE (10 min)

**See on PT unikaalne - näed frame-e liikumas!**

### 7.1 Valmista ette

1. **PC-del kustuta ARP:**
```
arp -d
```

2. **PT parem alumine nurk:** Simulation nupp

3. **Edit Filters:** Näita ainult ARP ja ICMP

### 7.2 Saada ping (aeglaselt)

**PC1:**
```
ping 192.168.[PORT].20 -n 1
```

**Vajuta Play (▶) PT-s AEGLASELT!**

### 7.3 Vaata frame-e

**Frame 1: ARP Request**
- Source: PC1 MAC
- Dest: FF:FF:FF:FF:FF:FF (broadcast!)
- Switch flooding → Fa0/2

**Frame 2: ARP Reply**
- Source: PC2 MAC
- Dest: PC1 MAC (unicast!)
- Switch forwarding → Fa0/1 (teab juba!)

**Frame 3-4: ICMP**
- Ping request/reply
- Switch forwarding mõlemad õigesti

**Google Docs:** Kirjelda, mida nägid (3-4 lauset).

---

## OSA 8: KÜSIMUSED (5 min)

**Google Docs:**

1. **Miks switch saatis ARP request kõigile portidele?**

2. **Miks switch saatis ARP reply ainult Fa0/1?**

3. **Millal switch õppis PC1 MAC-i?**

4. **Millal switch õppis PC2 MAC-i?**

5. **Kas see oli sama mis loengu teoreetilises osas?**

---

## LISAÜLESANNE (kui aega)

### Aging Time

**Switch:**
```
show mac address-table aging-time
```

Näed: 300 sek

**Google Docs:** Kirjuta.

### Broadcast Test

**PC1:**
```
ping 192.168.[PORT].255
```

**Switch MAC table:** Kas FF:FF:FF:FF:FF:FF seal? EI!

---

## SALVESTA

**File → Save As:**
```
lab7-[gruppinimi].pkt
```

**ÄRA ESITA VEEL!**

Peale serveriruum-i täidad server osa → siis esitad KOGU dokumendi.

---

## JÄRGMINE SAMM

**Peale pausi:**
- TE lähete SERVERIRUUM-i
- Näete PÄRIS switch-i
- Teete füüsilised ühendused
- Sama MAC table, aga PÄRISELT!

**Erinevus:**
- PT = kiire, lihtne, simulatsioon
- Server = aeglane, keerulisem, PÄRIS

**Loogika SAMA!**

---

## DETAILID

**Kui vaja rohkem selgitust:**
- ARP protsess → Vaata server lab OSA 5.5
- MAC table loogika → Vaata Loeng Week 6
- Troubleshooting → Vaata server lab

---

**Valmis! Nüüd oota pausi, siis serveriruum!**