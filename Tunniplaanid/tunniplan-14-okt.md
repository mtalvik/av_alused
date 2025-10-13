# TUNNIPLAN - 14. oktoober 2025

**Teema:** OSI Layer 1-2 + Lab 7 (Server + PT)  
**Õpilasi:** 20 (10 paari) → Grupp A (5 paari), Grupp B (5 paari)

---

## ⏰ AJAKAVA

### GRUPP 1 (08:30-11:00)

| Aeg | Tegevus | Asukoht |
|-----|---------|---------|
| 08:30-08:40 | Quiz | Klass 310 |
| 08:40-09:20 | Loeng (Layer 1-2) | Klass 310 |
| 09:20-10:00 | **Rotatsioon 1** | |
| | Grupp A (5 paari) → Server | Serveriruum |
| | Grupp B (5 paari) → PT lab | Klass 310 |
| 10:00-10:15 | **PAUS** | |
| 10:15-11:00 | **Rotatsioon 2** | |
| | Grupp A (5 paari) → Dokumenteerimine | Klass 310 |
| | Grupp B (5 paari) → Server | Serveriruum |

### GRUPP 2 (09:15-11:45)

| Aeg | Tegevus | Asukoht |
|-----|---------|---------|
| 09:15-09:25 | Quiz | Klass 310 |
| 09:25-10:00 | Loeng (Layer 1-2) | Klass 310 |
| 10:00-10:15 | **PAUS** | |
| 10:15-11:00 | **Rotatsioon 1** | |
| | Grupp A (5 paari) → Server | Serveriruum |
| | Grupp B (5 paari) → PT lab | Klass 310 |
| 11:00-11:45 | **Rotatsioon 2** | |
| | Grupp A (5 paari) → Dokumenteerimine | Klass 310 |
| | Grupp B (5 paari) → Server | Serveriruum |

---

## 📚 MATERJALID (PRINT/VALMISTA)

### Õpetajale
- [ ] Quiz küsimused (10 tk)
- [ ] Loeng materjalid (Nädal 6 dokument)
- [ ] Füüsiline kaabel + RJ-45 (demo)
- [ ] Grupp A rotatsioon juhend
- [ ] Grupp B rotatsioon juhend

### Õpilastele
- [ ] Lab 7 - Server manual (kõigile)
- [ ] Lab 7 - PT manual (kõigile)
- [ ] Google Docs template link (Classroom)

### Serveriruum
- [ ] 10 switch-i valmis (toide, kaablid)
- [ ] 10+ patch-kaablit (sinised)
- [ ] 10 konsoolikaablit (USB)

---

## 📝 TUNNI KULG - DETAILSELT

### 1. QUIZ (10 min)

**Formaat:**
- Paaritöö (sama paarid kui lab-is)
- 10 küsimust tahvlil
- Vastused vihikusse
- Kiire ülevaatus koos

**Teemad:**
- OSI mudel (mis layer on mis?)
- Lab 6 meenutus (T-568B, crimping)
- MAC vs IP küsimus
- Hub vs Switch

---

### 2. LOENG (35-40 min)

**Struktuur:**

| Aeg | Teema | Sisu |
|-----|-------|------|
| 0-5 min | Kontekst | Lab 6 → Layer 1-2 seos |
| 5-17 min | Layer 1 | Signaalid, kaablid, Hub |
| 17-37 min | Layer 2 | MAC, Frame, Switch, MAC table |
| 37-40 min | Kokkuvõte | Lab 7 teaser (serveriruum!) |

**Olulised punktid:**
- Näita füüsilist kaablit (Lab 6 output)
- Demo: `ipconfig /all` (MAC aadress)
- Joonista MAC table õppimist tahvlile
- Hoiatus: serveriruum = päris võrk!

**Materjalid:**
- Nädal 6 loeng dokument (base)
- Tahvel joonistamiseks
- 1 PC projektor küljes

---

### 3. ROTATSIOON 1 (40-45 min)

#### GRUPP A → SERVERIRUUM

**Ülesanded:**
1. Leia oma klassi pordid (PC1, PC2)
2. Leia patch-paneel serveris
3. Vali switch
4. Ühenda PC2 (Ethernet Fa0/2)
5. Ühenda PC1 (Console USB)
6. Terminal ühendus (PuTTY)
7. Reset (kui aega)
8. Hostname + paroolid (kui aega)

**Dokument:** `lab7-switch-mac-table.md` (OSA 1-4)

**Jälgi:**
- Ära lase puudutada musta/kollaseid kaableid!
- Kontrolli, et COM port leitud (Device Manager)
- Aita resetiga (Mode nupp keeruline)

#### GRUPP B → KLASS (PT)

**Ülesanded:**
1. Ava Packet Tracer
2. Ehita topoloogia (3 PC + 1 Switch)
3. Seadista IP-d
4. Ping test
5. Vaata MAC table (`show mac address-table`)
6. Simulation mode (ARP vaatamine)
7. Dokumenteeri Google Docs

**Dokument:** `lab7-pt-switch-mac.md` (uus!)

**Jälgi:**
- PT versioon peab õige olema
- Simulation mode demo vaja?

---

### 4. PAUS (15 min)

**Kontrolli:**
- Kas Grupp A jõudis serveris resetini?
- Kas Grupp B sai PT-s MAC table vaadatud?

---

### 5. ROTATSIOON 2 (45 min)

#### GRUPP B → SERVERIRUUM

**Ülesanded:**
- Sama mis Grupp A tegi (OSA 1-4)
- Aga nüüd lisaks:
  - PC1 konsool → Ethernet (OSA 5.3)
  - IP aadressid
  - ARP/Ping test
  - MAC table vaatamine

**Dokument:** `lab7-switch-mac-table.md` (OSA 1-5)

**Eesmärk:** Näha MAC table LIVE!

#### GRUPP A → KLASS (Dokumenteerimine)

**Ülesanded:**
1. Täida Google Docs täielikult
2. Screenshot switch output
3. Vasta kontrollküsimustele
4. Kui valmis → võib teha PT lab (bonus)

**Dokument:** Google Docs template

---

## 🎯 LÕPPTULEMUS

**Iga õpilane peab saama:**
- ✅ Serveriruum kogemus (füüsiline töö)
- ✅ MAC table vaatamine (switch CLI)
- ✅ PT simulatsioon (virtuaalne)
- ✅ Google Docs täidetud ja esitatud

**Hindamine:** 13p (vaata Lab 7 manual)

---

## ⚠️ KRIITILISED PUNKTID

### Enne tundi
- [ ] Kontrolli 10 switch-i serveris (toide, reset tehtud?)
- [ ] Testi 1 konsoolikaabel (COM port töötab?)
- [ ] Google Classroom: Lab 7 assignment + template uploaded

### Tunni ajal
- [ ] Jaga grupid ENNE rotatsiooni (5+5 paari)
- [ ] Anna selged juhised: "Grupp A nimed: ..."
- [ ] Timer pausi jaoks (15 min täpselt)

### Serveris
- [ ] Jälgi, et AINULT sinised kaablid kasutuses
- [ ] Mode nupp reset: aita esimest korda
- [ ] COM port probleem: Device Manager demo

### Pärast tundi
- [ ] Kontrolli, et kõik kaablid tagasi
- [ ] Switch-id välja lülitatud (või jäetud töötama?)
- [ ] Google Docs esitamised (Classroom)

---

## 📊 ÕNNESTUMISE KRITEERIUMID

| Kriteerium | Eesmärk |
|------------|---------|
| Aeg | Kõik jõuavad mõlemas rotatsioonis käia |
| Serveris | Iga paar saab füüsilise ühenduse tehtud |
| PT | Iga paar näeb MAC table-it |
| Turvalisus | Mitte ühtegi vale kaablit tõmmatud |
| Dokumenteerimine | 100% Google Docs esitatud |

---

## 🔧 BACKUP PLAANID

**Kui switch-e vähem kui 10:**
- Tee 3 rotatsiooni (3x 20-25 min)
- Mõned ootavad → anna neile teooria küsimused

**Kui serverisse ei saa:**
- Kõik teevad PT lab-i
- Demo switch tahvlil/ekraanil

**Kui PT ei tööta:**
- Kõik teevad teooria + dokumenteerimist
- YouTube videod MAC table kohta