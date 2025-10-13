# GRUPP A - ROTATSIOON JUHEND

**Õpilasi:** 5 paari (10 inimest)

---

## ROTATSIOON 1: SERVERIRUUM (40 min)

### Eesmärk
Teha füüsilised ühendused ja alustada switch konfiguratsiooni.

### Kontrollist

**OSA 1-3 (kohustuslik):**
- [ ] Leia oma klassi pordid
- [ ] Leia patch-paneel serveris
- [ ] Vali switch
- [ ] Ühenda PC2 (Ethernet Fa0/2)
- [ ] Ühenda PC1 (Console USB)
- [ ] Leia COM port (Device Manager)
- [ ] PuTTY ühendus
- [ ] Mode nupp reset
- [ ] Kustuta config

**OSA 4 (kui aega):**
- [ ] Hostname
- [ ] Enable secret
- [ ] Console password
- [ ] Banner
- [ ] Salvesta config

### Checkpoint (20 min)

**Peaksid olema tehtud:**
- Füüsilised ühendused ✓
- PuTTY ühendus ✓
- Reset alustatud

**Kui ei jõua OSA 4:**
- Pole probleem!
- Teete peale pausi

### Õpetaja Abi

**Kus raskused:**
- Mode nupp reset (aita esimest korda)
- COM port ei leidu (Device Manager check)
- PuTTY ei ühendu (seaded)

---

## PAUS (15 min)

**Kontrolli:**
- Kas reset tehtud?
- Kas hostname vähemalt peal?

---

## ROTATSIOON 2: DOKUMENTEERIMINE (45 min)

### Eesmärk
Täida Google Docs ja valmista ette esitamiseks.

### Tegevused

**Kohustuslik:**
1. **Kontrolli Google Docs**
   - Kas serveris osa täidetud?
   - Kas screenshot-id tehtud?
   
2. **Täida server osa lõpuni**
   - Hostname, paroolid
   - MAC table output
   - Kontrollküsimused

3. **Kontrolli PT osa** (kui teinud)

4. **ESITA** Google Classroom-is

**Kui valmis vara (bonus):**
- Tee PT lab (kui ei teinud)
- Aita teist paari
- Loe järgmise nädala materjali

### Checkpoint (30 min)

**Peaksid olema:**
- [ ] Google Docs 90% täidetud
- [ ] Valmis esitamiseks

### Lõpp (45 min)

**ESITA Google Docs!**

---

## KORDUMA KIPPUVAD KÜSIMUSED

**Q:** Ei jõudnud serveris kõike teha?  
**A:** Pole probleem - täida Google Docs selle põhjal, mida jõudsid.

**Q:** Kas peame PT-d tegema?  
**A:** Valikuline, aga soovitatav.

**Q:** Mida teha, kui kõik valmis?  
**A:** Aita teist paari või tee PT lab.

---

**Edu! Küsi julgelt abi!**

---

# GRUPP B - ROTATSIOON JUHEND

**Õpilasi:** 5 paari (10 inimest)

---

## ROTATSIOON 1: PACKET TRACER (40 min)

### Eesmärk
Simuleerida switch MAC õppimist PT-s.

### Kontrollist

**OSA 1-6 (kohustuslik):**
- [ ] Topoloogia ehitatud (2 PC + Switch)
- [ ] IP-d seadistatud (port-based)
- [ ] MAC aadressid leitud
- [ ] MAC table vaadatud (tühi)
- [ ] Ping test tehtud
- [ ] MAC table vaadatud (täis)

**OSA 7 (oluline!):**
- [ ] Simulation mode
- [ ] ARP Request vaadatud
- [ ] ARP Reply vaadatud
- [ ] Frame forwarding mõistetud

**OSA 8:**
- [ ] Küsimustele vastatud

### Checkpoint (20 min)

**Peaksid olema:**
- Topoloogia valmis ✓
- Ping töötab ✓
- MAC table vaadatud ✓

**Kui aeglaselt:**
- Jäta OSA 7 lühemaks
- Vaata ainult paar frame'i

### Checkpoint (35 min)

**Peaksid olema:**
- [ ] Simulation mode kasutatud
- [ ] Google Docs PT osa täidetud
- [ ] PT fail salvestatud

### Õpetaja Abi

**Kus raskused:**
- IP aadressid valesti (port number!)
- Ping ei tööta (kontrolli IP-sid)
- Simulation mode segane (näita esimest frame'i)

---

## PAUS (15 min)

**Kontrolli:**
- Kas PT osa täidetud?
- Kas fail salvestatud?

---

## ROTATSIOON 2: SERVERIRUUM (45 min)

### Eesmärk
Teha füüsilised ühendused ja näha MAC table PÄRISELT.

### Eelised

**Teil on lihtsam kui Grupp A-l oli:**
- Te teate juba MAC table loogikat (PT-st)
- Te olete näinud simulation mode-is
- Grupp A on juba teed näidanud

**Seega jõuate kaugemale!**

### Kontrollist

**OSA 1-4 (kiiresti):**
- [ ] Füüsilised ühendused
- [ ] PuTTY
- [ ] Reset
- [ ] Baaskonfiguratsioon (hostname, paroolid)
- [ ] Salvesta config

**OSA 5 (OLULINE!):**
- [ ] PC1 → Ethernet
- [ ] IP-d seadistatud
- [ ] Ping test
- [ ] **MAC table vaadatud LIVE!**
- [ ] ARP cache vaadatud
- [ ] Screenshot switch output

### Checkpoint (30 min)

**Peaksid olema:**
- [ ] Reset + config tehtud
- [ ] PC1 võrgus
- [ ] Ping töötab

**Kui ei jõua:**
- Vähemalt MAC table vaadatud!

### Lõpp (45 min)

**OLULINE:**
- [ ] MAC table screenshot Google Docs-i
- [ ] Eemalda kaablid
- [ ] Pane switch tagasi

---

## PÄRAST LABI

**Täida Google Docs lõpuni:**
- Server osa (serveris tehtud)
- PT osa (juba tehtud)
- Kontrollküsimused
- **ESITA!**

---

## KORDUMA KIPPUVAD KÜSIMUSED

**Q:** Kas serveris sama mis PT-s?  
**A:** Jah! Aga füüsiline ja päris MAC aadressid.

**Q:** Miks MAC table tühi alguses?  
**A:** Nagu PT-s - switch pole veel frame'e saanud.

**Q:** IP-d samad mis PT-s?  
**A:** Jah! Sama port number based.

---

**Edu! Küsi julgelt abi!**