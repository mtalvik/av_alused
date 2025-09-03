# Loeng 1: Võrgude alused

---

## 1: Tere tulemast!

### Arvutivõrkude alused
- **16 nädalat**
- **Praktilised oskused**
- **Cisco seadmed**
- **CCNA ettevalmistus**

"Kellel on kodus ruuter?"

---

## 2: Mis on võrk?

### Lihtne definitsioon
**Võrk = kaks või rohkem seadet, mis saavad omavahel suhelda**

### Igapäevaelus:
- Kodus: WiFi ühendab telefoni, arvuti, TV
- Koolis: arvutiklassi arvutid
- Internetis: WhatsApp sõpradega

"Nimetage 3 seadet, mis on teie kodus võrgus!"

---

## 3: Miks võrke vaja?

```mermaid
mindmap
  root((VÕRGUD))
    Suhtlemine
      WhatsApp
      Discord
      Videokõned
    Jagamine
      Fotod
      Netflix
      Google Drive
    Ressursid
      Printer
      Internet
      Mängud
    Info
      Google
      YouTube
      Wikipedia
```

Mis juhtuks, kui internet kadugu 1 nädalaks?

---

## 4: Võrgu osad

### 3 peamist komponenti:

```mermaid
graph TD
    A[SEADMED<br/>Telefon, Arvuti, Server] --> B[ÜHENDUSED<br/>WiFi, Kaablid, 4G]
    B --> C[VAHENDAJAD<br/>Router, Switch, Telefonimaster]
    C --> A
```

**Analoogia:** Postisüsteem
- Seadmed = inimesed
- Ühendused = teed
- Vahendajad = postkontor

---

## 5: Star Topoloogia

```mermaid
graph TD
    PC1[PC1] --- SW[Switch]
    PC2[PC2] --- SW
    PC3[PC3] --- SW
    PC4[PC4] --- SW
    SW --- Server[Server]
```

### Omadused:
- **Eelised:** Lihtne hallata, üks PC rike ei mõjuta teisi
- **Puudused:** Switch rike = kogu võrk seiskub
- **Näited:** Kodune WiFi, kooli võrk

"Mis juhtub kui WiFi ruuter kodus läheb katki?"

---

## 6: Bus Topoloogia

```mermaid
graph LR
    PC1[PC1] --- PC2[PC2]
    PC2 --- PC3[PC3] 
    PC3 --- PC4[PC4]
    PC4 --- Server[Server]
```

### Omadused:
- **Eelised:** Vähe kaablit, odav
- **Puudused:** Peakaabli rike = kõik seiskub
- **Kasutamine:** Vanemad võrgud

**Analoogia:** Bussiliin - kõik peatused ühel teel

---

## 7: Ring Topoloogia

```mermaid
graph LR
    PC1[PC1] --- PC2[PC2]
    PC2 --- PC3[PC3]
    PC3 --- PC4[PC4] 
    PC4 --- PC1
```

### Omadused:
- **Eelised:** Igal seadmel võrdne juurdepääs
- **Puudused:** Ühe seadme rike mõjutab kogu võrku
- **Andmed:** Liiguvad ühes suunas

---

## 8: Võrgu seadmed - Lõppseadmed

```mermaid
graph TD
    A[Lõppseadmed<br/>End Devices] --> B[Telefon]
    A --> C[Arvuti]
    A --> D[PlayStation]
    A --> E[Printer]
    A --> F[Server]
    A --> G[Smart TV]
```

### Funktsioonid:
- Andmete genereerimine
- Andmete tarbimine
- Võivad olla klient VÕI server

 "Võtke telefon välja - see on lõppseade!"

---

## 9: Võrgu seadmed - Vahendajad

```mermaid
graph TD
    A[Vahendajad<br/>Intermediary Devices] --> B[Switch<br/>24-pesane pikendus]
    A --> C[Router<br/>Postiljon - teab kuhu saata]
    A --> D[WiFi Access Point<br/>Raadiotorn]
    A --> E[Firewall<br/>Turvamees]
```

### Funktsioonid:
- Andmete edastamine
- Õige sihtkoha leidmine
- Turvalisuse tagamine

Vaata meie klassis switchi

---

## 10: Ühendused

```mermaid
graph TD
    A[Network Media<br/>Ühendused] --> B[Ethernet kaabel<br/>Vaskjuhtmed]
    A --> C[WiFi<br/>Raadiolained]
    A --> D[Fiber Optic<br/>Valgus klaaskius]
    A --> E[4G/5G<br/>Telefonimaster]
```

### Omadused:
- **Ethernet:** Stabiilne, kiire
- **WiFi:** Mugav, mobiilne  
- **Fiber:** Ülikiire, kallis
- **Mobiilne:** Kõikjal kättesaadav

---

## 11: Võrgu tüübid suuruse järgi

```mermaid
graph TD
    PAN[PAN<br/>Personal Area Network<br/>1-10m] --> LAN[LAN<br/>Local Area Network<br/>Hoone/Campus]
    LAN --> MAN[MAN<br/>Metropolitan Area Network<br/>Linn]
    MAN --> WAN[WAN<br/>Wide Area Network<br/>Riik/Maailm]
```

**Näited:**
- **PAN:** Telefon + kõrvaklapid
- **LAN:** Kodune WiFi
- **MAN:** Linna avalik WiFi
- **WAN:** Internet

"Millist võrku kasutate kõige rohkem?"

---

## 12: Internet kui WAN

```mermaid
graph TD
    ISP1[Tier 1 ISP<br/>AT&T, Verizon] --- ISP2[Tier 2 ISP<br/>Elion, Tele2]
    ISP2 --- ISP3[Tier 3 ISP<br/>Kohalik teenuse pakkuja]
    ISP3 --- HOME[Kodune võrk]
    ISP1 --- GOOGLE[Google]
    ISP1 --- FACEBOOK[Facebook]
```

### Internet = võrkude võrk
- Tuhandeid erinevaid võrke
- Ühised protokollid
- Globaalne ühenduvus

---

## 13: Miks Cisco?

### Cisco faktid:
- **50%** maailma ruuteritest
- **70%** Internet'i liiklusest läbib Cisco seadmeid
- **CCNA** sertifikaat tunnustatud kogu maailmas
- **200+ riigis** kasutusel

### Meie koolis:
- Päris Cisco seadmed serveriruu
- Sama käsud mis ettevõtetes
- Reaalse töökogemuse simulatsioon

---

## 14: Packet Tracer

```mermaid
graph TD
    A[Packet Tracer] --> B[Simulatsioonitarkvara]
    A --> C[Turvaline katsetamine]  
    A --> D[Sama käsud mis päris seadmetes]
    A --> E[Kodus harjutamine]
    A --> F[Visuaalne õppimine]
```

### Järgmise tunni eelvaade:
- Installeerime/avame PT
- Loome esimese võrgu
- Testimg ping käsuga
- 2 PC + 1 Switch

**NB:** Vajate Google kontot!

---

## 15: Kontrollküsimused

### 1. Mis on võrgu kolm peamist osa?


### 2. Millise topoloogia näete kõige rohkem?


### 3. Mis vahe on lõppseadmel ja vahendajal?


### 4. Nimetage 4 erinevat võrgu tüüpi!


---

## 16: Järgmiseks tunniks

### Ootused:
- **Google konto** Packet Tracer jaoks
- **Avatud meel** uute asjade õppimiseks

### Kuhu liigume:
1. **Täna:** Packet Tracer esimene võrk
2. **Järgmine nädal:** Päris Cisco seadmed
3. **Kuu pärast:** Oma võrk serveriruumis

### Küsimused?

**"IT-s töötamiseks tuleb alustada algusest - täna teeme selle esimese sammu!"**

---


### Analoogiad:

**Võrk = Inimeste grupp**
- Lõppseadmed = Inimesed kes räägivad
- Vahendajad = Tõlkijad
- Ühendused = Keel mida kasutavad