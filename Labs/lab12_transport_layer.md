# Lab 12 — Transpordikiht praktikas

*TCP handshake, pordid ja võrguühenduste uurimine*

---

## Eesmärk

Selles laboris:
- Näed TCP 3-way handshake'i Packet Traceri simulation mode'is
- Uurid oma arvuti aktiivseid ühendusi
- Testid porte käsurealt

---

## Osa 1: TCP Handshake Packet Traceris (20 min)

### 1.1 Topoloogia loomine

Loo Packet Traceris lihtne võrk:

```
[PC0] -------- [Switch] -------- [Server0]
```

**Seaded:**

| Seade | IP aadress | Subnet Mask |
|-------|------------|-------------|
| PC0 | 192.168.1.10 | 255.255.255.0 |
| Server0 | 192.168.1.100 | 255.255.255.0 |

### 1.2 Serveri seadistamine

1. Kliki **Server0** → **Services** → **HTTP**
2. Veendu, et HTTP on **On**

### 1.3 Simulation Mode

1. Kliki all paremas nurgas **Simulation** (mitte Realtime!)
2. Kliki **Edit Filters** → jäta ainult **TCP** ✓
3. See filtreerib välja muu liikluse

### 1.4 Tee HTTP päring

1. Kliki **PC0** → **Desktop** → **Web Browser**
2. Kirjuta URL: `http://192.168.1.100`
3. **ÄRA VAJUTA ENTER VEEL!**

### 1.5 Jälgi handshake'i

1. Vajuta **Capture/Forward** nuppu aeglaselt (samm-sammult)
2. Jälgi pakette:

**📸 Screenshot 1:** Tee pilt, kui näed **SYN** paketti (esimene samm)

**📸 Screenshot 2:** Tee pilt, kui näed **SYN-ACK** paketti (teine samm)

**📸 Screenshot 3:** Tee pilt, kui näed **ACK** paketti (kolmas samm)

### 1.6 Küsimused

**Küsimus 1:** Mis järjekorras läksid paketid? Kirjuta siia:

```
1. _______________
2. _______________
3. _______________
```

**Küsimus 2:** Kliki ühel TCP paketil ja vaata "Outbound PDU Details". Mis **source port** ja **destination port** on? Kirjuta siia:

```
Source Port: _______________
Destination Port: _______________
```

**Küsimus 3:** Miks destination port on just see number?

```
_______________________________________________
```

---

## Osa 2: Oma arvuti ühendused (20 min)

Nüüd uurime, mis toimub SINU arvutis!

### 2.1 Ava käsurida

**Windows:** Start → kirjuta `cmd` → Enter

**Mac:** Terminal

### 2.2 Vaata aktiivseid ühendusi

Kirjuta:

```
netstat -an
```

**📸 Screenshot 4:** Tee pilt tulemustest

### 2.3 Analüüsi tulemusi

**Küsimus 4:** Leia üks rida, kus on **ESTABLISHED** staatuses ühendus. Kopeeri see rida siia:

```
_______________________________________________
```

**Küsimus 5:** Leia üks rida, kus on **LISTENING** staatuses port. Kopeeri see rida siia:

```
_______________________________________________
```

**Küsimus 6:** Mis vahe on ESTABLISHED ja LISTENING staatusel?

```
_______________________________________________
_______________________________________________
```

### 2.4 Filtreeri tulemusi

Proovi neid käske:

**Windows:**
```
netstat -an | findstr :80
netstat -an | findstr :443
netstat -an | findstr ESTABLISHED
```

**Mac/Linux:**
```
netstat -an | grep :80
netstat -an | grep :443
netstat -an | grep ESTABLISHED
```

**Küsimus 7:** Kas leidsid ühendusi porti 443? Mis see tähendab?

```
_______________________________________________
```

---

## Osa 3: Pordi testimine (15 min)

### 3.1 Test-NetConnection (Windows) või nc (Mac/Linux)

**Windows PowerShell:**
```powershell
Test-NetConnection google.com -Port 80
Test-NetConnection google.com -Port 443
Test-NetConnection google.com -Port 22
```

**Mac/Linux:**
```bash
nc -zv google.com 80
nc -zv google.com 443
nc -zv google.com 22
```

**📸 Screenshot 5:** Tee pilt ühest õnnestunud ja ühest ebaõnnestunud testist

**Küsimus 8:** Milline port oli AVATUD ja milline SULETUD?

```
Avatud port: _______________
Suletud port: _______________
```

**Küsimus 9:** Miks Google'i port 22 (SSH) ei ole avatud?

```
_______________________________________________
```

### 3.2 Testi kohalikke teenuseid

Proovi testida oma koolivõrgu teenuseid (kui lubatud):

```powershell
Test-NetConnection [serveri_ip] -Port 80
Test-NetConnection [serveri_ip] -Port 22
```

**Küsimus 10:** Milliseid porte leidsid avatuna?

```
_______________________________________________
```

---

## Osa 4: Wireshark demo (valikuline, kui aega on)

Kui õpetaja näitab Wiresharki:

**📸 Screenshot 6:** Tee pilt päris TCP handshake'ist Wiresharkis

**Küsimus 11:** Mis oli erinevus Packet Traceri ja Wiresharki vahel?

```
_______________________________________________
_______________________________________________
```

---

## Kokkuvõte

### Mida sa täna õppisid?

Märgi ära:

- [ ] Nägin TCP 3-way handshake'i Packet Traceris
- [ ] Tean, mis on SYN, SYN-ACK ja ACK
- [ ] Oskan kasutada `netstat` käsku
- [ ] Tean vahet LISTENING ja ESTABLISHED staatustel
- [ ] Oskan testida, kas port on avatud

### Tähtsamad pordid (kirjuta meelde!)

| Port | Teenus |
|------|--------|
| 22 | |
| 23 | |
| 53 | |
| 80 | |
| 443 | |

---

## Esitamine

**Esita Google Docs fail, mis sisaldab:**

1. ✅ 5-6 screenshot'i (märgitud 📸)
2. ✅ Vastused küsimustele 1-10 (või 1-11)
3. ✅ Täidetud kokkuvõtte tabel

**Tähtaeg:** _______________

**Google Classroom:** _______________

---

*Hästi tehtud! Nüüd tead, kuidas TCP "telgitagustes" töötab! 🚀*
