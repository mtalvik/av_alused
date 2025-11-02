# IPv4 Adresseerimine - ÕPETAJA JUHEND

## ÜLEVAADE

See sessioon on praktiline tund, kus õpilased harjutavad paberil IPv4 aadresseerimise põhitõdesid. Eelmises tunnis said nad teoreetilise aluse, nüüd pannakse see praktikasse.

**Sessiooni jaotus:**
- **20-25 min**: Õpetamine ja näidete lahendamine koos klassiga
- **50-60 min**: Õpilased lahendavad harjutusi iseseisvalt/paarides
- **10-15 min**: Vastuste ülevaatamine ja küsimused

**Vajalikud materjalid:**
- Õpilaste harjutuste lehed (printida välja)
- Tahvel või ekraan näidete jaoks
- Pliiatsid (kalkulaatorid ei ole vajalikud, aga võib lubada)

---

## OSA 1: ÕPETAMINE (20-25 min)

### 1.1 Binary ↔ Decimal Konversioon

**Alusta meeldetuletusega:**

Kirjuta tahvlile binary positsiooni tabel:

```
128   64   32   16   8   4   2   1
```

Selgita: "Iga positsioon on 2 aste. Kui bit on 1, siis liidad selle numbri. Kui bit on 0, siis ei liida."

#### NÄIDE 1: Binary → Decimal

**Ülesanne:** `11000000` → ?

**Lahenda koos klassiga:**

```
Positsioon: 128   64   32   16   8   4   2   1
Binary:      1    1    0    0   0   0   0   0
            ✓    ✓    ✗    ✗   ✗   ✗   ✗   ✗

Arvutus: 128 + 64 = 192

Vastus: 192
```

**Näpunäide õpilastele:** "Alusta vasakult. Kui näed 1, lisa number. Kui näed 0, jäta vahele."

#### NÄIDE 2: Decimal → Binary

**Ülesanne:** `168` → ?

**Lahenda koos klassiga:**

Meetod: Lahuta suurimad võimalikud 2 astmed

```
168 - kas >= 128? JAH → bit 1, jääb 40
40  - kas >= 64?  EI  → bit 0, jääb 40
40  - kas >= 32?  JAH → bit 1, jääb 8
8   - kas >= 16?  EI  → bit 0, jääb 8
8   - kas >= 8?   JAH → bit 1, jääb 0
0   - kas >= 4?   EI  → bit 0
0   - kas >= 2?   EI  → bit 0
0   - kas >= 1?   EI  → bit 0

Vastus: 10101000
```

**Kontroll:** 128 + 32 + 8 = 168 ✓

**Näpunäide õpilastele:** "Lihtsam on alustada suurimast. Kui number on suurem või võrdne, siis 1 ja lahuta. Kui väiksem, siis 0 ja jäta number samaks."

#### NÄIDE 3: Terve IP aadress

**Ülesanne:** `192.168.1.10` → binary?

**Lahenda oktetthaaval:**

```
192 → 11000000
168 → 10101000
1   → 00000001
10  → 00001010

Vastus: 11000000.10101000.00000001.00001010
```

**Näpunäide:** "Tee üks oktett korraga. Kontrolli iga oktett enne järgmise juurde minekut."

---

### 1.2 IP Klassi Äratundmine

**Alusta meeldetuletusega:**

Kirjuta tahvlile klassid:

```
Class A: 1   - 126   (esimene oktett)
Class B: 128 - 191
Class C: 192 - 223
Class D: 224 - 239 (multicast)
Class E: 240 - 255 (experimental)

127 = loopback (localhost)
```

**Näpunäide:** "Vaata ainult ESIMEST oktetti!"

#### NÄIDE 4: Klassi tuvastamine

```
10.50.30.1      → esimene oktett 10  → Class A
172.16.5.100    → esimene oktett 172 → Class B
192.168.1.50    → esimene oktett 192 → Class C
224.0.0.5       → esimene oktett 224 → Class D
127.0.0.1       → esimene oktett 127 → Loopback
```

**Harjutus klassiga:** Näita mõned IP-d ja lase õpilastel klassi ära arvata (käed üles).

---

### 1.3 Network, Broadcast ja Host Aadressid

**Alusta meeldetuletusega:**

```
Network address   = kõik host bittid on 0
Broadcast address = kõik host bittid on 1
Host addresses    = nende vahel
```

#### NÄIDE 5: Class C võrk

**Võrk:** `192.168.1.0/24`

**Selgita sammhaaval:**

```
Subnet mask: /24 = esimesed 24 bitti on network
             = esimesed 3 oktetti on network
             = viimane oktett on host

Network address:
- Network osa: 192.168.1
- Host osa: kõik nullid = .0
- Network: 192.168.1.0

Broadcast address:
- Network osa: 192.168.1
- Host osa: kõik ühed = .255
- Broadcast: 192.168.1.255

Host aadressid:
- Vahemik: 192.168.1.1 kuni 192.168.1.254
- Hostide arv: 2^8 - 2 = 254
```

**Joonista tahvlile:**

```
192.168.1.0      ← Network (ei saa kasutada)
192.168.1.1      ← Esimene host (tavaliselt router)
192.168.1.2      ← Host
...
192.168.1.254    ← Viimane host
192.168.1.255    ← Broadcast (ei saa kasutada)
```

#### NÄIDE 6: Class B võrk

**Võrk:** `172.16.0.0/16`

```
Subnet mask: /16 = esimesed 16 bitti on network
             = esimesed 2 oktetti on network
             = viimased 2 oktetti on host

Network address: 172.16.0.0
Broadcast address: 172.16.255.255

Host aadressid: 172.16.0.1 kuni 172.16.255.254
Hostide arv: 2^16 - 2 = 65534
```

#### NÄIDE 7: Class A võrk

**Võrk:** `10.0.0.0/8`

```
Subnet mask: /8 = esimesed 8 bitti on network
            = esimene oktett on network
            = viimased 3 oktetti on host

Network address: 10.0.0.0
Broadcast address: 10.255.255.255

Host aadressid: 10.0.0.1 kuni 10.255.255.254
Hostide arv: 2^24 - 2 = 16777214
```

---

### 1.4 Hostide Arvu Arvutamine

**Formula:** `2^n - 2`

Kus `n` = host bittide arv

**Selgita, miks -2:**
- -1 network address
- -1 broadcast address

#### NÄIDE 8: Erinevad subnet maskid

```
/24 → host bitte: 32-24 = 8  → hostide arv: 2^8 - 2  = 254
/16 → host bitte: 32-16 = 16 → hostide arv: 2^16 - 2 = 65534
/8  → host bitte: 32-8  = 24 → hostide arv: 2^24 - 2 = 16777214

/30 → host bitte: 32-30 = 2  → hostide arv: 2^2 - 2  = 2 (WAN link!)
/28 → host bitte: 32-28 = 4  → hostide arv: 2^4 - 2  = 14
```

**Näpunäide:** "/30 on kasulik WAN linkide jaoks, kus on vaja ainult 2 IP-d (router ↔ router)"

---

## OSA 2: ÕPILASED HARJUTAVAD (50-60 min)

**Anna välja harjutuste lehed.**

**Juhised õpilastele:**
1. Võite töötada üksi või paarides
2. Näidake kogu arvutuskäiku, mitte ainult vastust
3. Kui jääte hätta, vaadake tagasi näidetele
4. Küsige julgelt abi

**Õpetaja roll:**
- Liigu klassis ringi
- Aita õpilasi, kes on hätta jäänud
- Kontrolli, et nad näitavad arvutuskäiku
- Anna vihjeid, aga ära anna otse vastuseid

**Levinud vead, mida jälgida:**
- Binary konversioonil unustavad bitti positsioone
- Loevad IP klassi mitme okteti järgi (ainult esimene loeb!)
- Broadcast aadressis jätavad mõne okteti 0-ks
- Unustavad hostide arvutuses -2 teha

---

## OSA 3: VASTUSTE ÜLEVAATAMINE (10-15 min)

**Kontrolli koos klassiga olulisemad harjutused:**

1. Küsi, kes said õigesti
2. Lase õpilasel selgitada oma lahenduskäiku
3. Selgita vigu, kui keegi eksis
4. Rõhuta õigeid meetodeid

**Soovitus:** Ära käi kõiki harjutusi läbi - vali 3-4 keerukamat ja kontrolli need põhjalikult.

---

## KOKKUVÕTE JA JÄRGMINE TUND

**Lõpeta sessioon:**

"Täna harjutasime IPv4 aadresside baasoskusi paberil. Need on olulised, sest:
- Järgmisel tunnis (SERVER LAB) hakkame neid IP-sid päris seadmetele seadistama
- Ilma nende oskusteta ei saa te IP-sid planeerida ega võrke üles ehitada
- Need on CCNA eksami osa"

**Kodune töö (valikuline):**
"Kui tunnete, et vajate veel harjutamist, tehke harjutuste lehe ülesanded uuesti või leidke internetist binary konversiooni harjutusi."

**Järgmine tund:**
"Järgmine kord (120min SERVER LAB) me:
- Planeerime IP aadresse võrgutopoloogiale
- Seadistame routeritele ja arvutitele IP aadressid
- Kontrollime, kas võrk töötab"

---

## LISA: KIIRED VASTUSED SAGEDASTELE KÜSIMUSTELE

**K: "Kas ma pean kõik binary numbrid pähe õppima?"**
V: "Ei, aga 128, 64, 32, 16, 8, 4, 2, 1 peaksid meeles olema. Ülejäänu saad arvutada."

**K: "Miks me üldse binary-t õpime kui arvuti teeb selle nagunii ära?"**
V: "Sest subnet mask ja subnetting töötavad binary tasemel. Ilma binary mõistmata ei saa sa võrke planeerida."

**K: "Mis vahe on /24 ja 255.255.255.0?"**
V: "Need on sama asi. /24 on lühike viis öelda 'esimesed 24 bitti on network'. 255.255.255.0 on pikk viis sama asja öelda."

**K: "Miks ei saa ma network aadressi seadmele määrata?"**
V: "Network aadress on nagu maja aadress - see näitab, kus maja asub, aga sa ei saa elada 'maja aadressis'. Sa elad konkreetses korteris (host aadress)."

**K: "Kas ma saan 192.168.1.0 ja 192.168.1.255 pingida?"**
V: "Network aadressi (192.168.1.0) ei saa pingida. Broadcast aadressi (192.168.1.255) teoreetiliselt saad, aga mitte kõik seadmed vastavad sellele."

---

**Edu õpetamisel!** 🎯