# IPv4 Adresseerimine - HARJUTUSED - VARIANT A

**Nimi:** ________________________  **Kuupäev:** __________

**Juhised:**
- Lahenda harjutused paberil
- Näita kogu arvutuskäiku, mitte ainult vastust
- Võid töötada üksi või paarides
- Kontrolli oma vastuseid enne edasi minekut

---

## OSA 1: BINARY → DECIMAL (10 punkti)

Teisenda järgmised binary numbrid decimal kujule. Näita arvutuskäik.

**Näide:**
```
Binary: 11000000

Lahendus:
Positsioon: 128  64  32  16  8  4  2  1
Binary:      1   1   0   0  0  0  0  0

Arvutus: 128 + 64 = 192
Vastus: 192
```

### Harjutused:

**1.** `10000000` = ________

**2.** `11111111` = ________

**3.** `00001111` = ________

**4.** `10101010` = ________

**5.** `11000011` = ________

**6.** `01010101` = ________

**7.** `11110000` = ________

**8.** `00011111` = ________

**9.** `10011001` = ________

**10.** `11101110` = ________

---

## OSA 2: DECIMAL → BINARY (10 punkti)

Teisenda järgmised decimal numbrid binary kujule. Näita arvutuskäik.

**Näide:**
```
Decimal: 192

Lahendus:
192 >= 128? JAH → bit 1, jääb 64
64  >= 64?  JAH → bit 1, jääb 0
0   >= 32?  EI  → bit 0
0   >= 16?  EI  → bit 0
0   >= 8?   EI  → bit 0
0   >= 4?   EI  → bit 0
0   >= 2?   EI  → bit 0
0   >= 1?   EI  → bit 0

Vastus: 11000000
```

### Harjutused:

**11.** `128` = ________

**12.** `255` = ________

**13.** `192` = ________

**14.** `168` = ________

**15.** `64` = ________

**16.** `32` = ________

**17.** `10` = ________

**18.** `254` = ________

**19.** `127` = ________

**20.** `200` = ________

---

## OSA 3: TERVE IP AADRESS BINARY-KS (5 punkti)

Teisenda järgmised IP aadressid täielikult binary kujule (kõik 4 oktetti).

**Näide:**
```
IP: 192.168.1.1

192 → 11000000
168 → 10101000
1   → 00000001
1   → 00000001

Vastus: 11000000.10101000.00000001.00000001
```

### Harjutused:

**21.** `10.0.0.1`

Vastus: ________________________________________

**22.** `172.16.5.100`

Vastus: ________________________________________

**23.** `192.168.1.254`

Vastus: ________________________________________

**24.** `255.255.255.0`

Vastus: ________________________________________

**25.** `10.50.30.25`

Vastus: ________________________________________

---

## OSA 4: IP KLASSI ÄRATUNDMINE (10 punkti)

Määra iga IP aadressi klass (A, B, C, D, E või Loopback).

**Näide:**
```
IP: 192.168.1.1
Esimene oktett: 192
Klass: C (sest 192 on vahemikus 192-223)
```

### Harjutused:

**26.** `10.50.30.1` → Klass: ________

**27.** `172.16.5.100` → Klass: ________

**28.** `192.168.1.50` → Klass: ________

**29.** `224.0.0.5` → Klass: ________

**30.** `127.0.0.1` → Klass: ________

**31.** `126.255.255.255` → Klass: ________

**32.** `128.0.0.1` → Klass: ________

**33.** `223.255.255.255` → Klass: ________

**34.** `240.0.0.1` → Klass: ________

**35.** `191.168.1.1` → Klass: ________

---

## OSA 5: PUBLIC vs PRIVATE IP (5 punkti)

Määra, kas IP aadress on PUBLIC või PRIVATE.

**Meeldetuletus:**
- Private vahemikud: `10.0.0.0 - 10.255.255.255`
- Private vahemikud: `172.16.0.0 - 172.31.255.255`
- Private vahemikud: `192.168.0.0 - 192.168.255.255`

### Harjutused:

**36.** `10.50.30.1` → ____________

**37.** `8.8.8.8` → ____________

**38.** `172.16.5.100` → ____________

**39.** `172.32.5.100` → ____________

**40.** `192.168.1.50` → ____________

---

## OSA 6: HOSTIDE ARVU ARVUTAMINE (10 punkti)

Arvuta, mitu hosti mahub antud võrku. Kasuta formulat: **2^n - 2**

**Näide:**
```
Võrk: 192.168.1.0/24

Subnet mask: /24 → network bitte 24
Host bitte: 32 - 24 = 8
Hostide arv: 2^8 - 2 = 256 - 2 = 254

Vastus: 254 hosti
```

### Harjutused:

**41.** `192.168.1.0/24` → ________ hosti

**42.** `172.16.0.0/16` → ________ hosti

**43.** `10.0.0.0/8` → ________ hosti

**44.** `192.168.5.0/30` → ________ hosti

**45.** `172.20.0.0/28` → ________ hosti

**46.** `10.10.0.0/20` → ________ hosti

**47.** `192.168.100.0/26` → ________ hosti

**48.** `172.30.0.0/12` → ________ hosti

**49.** `10.50.50.0/29` → ________ hosti

**50.** `192.168.200.0/25` → ________ hosti

---

## OSA 7: NETWORK JA BROADCAST AADRESSI LEIDMINE (20 punkti)

Leia iga võrgu network address ja broadcast address.

**Näide:**
```
Võrk: 192.168.1.0/24

Subnet mask: /24 = esimesed 3 oktetti network, viimane host

Network address: 192.168.1.0 (kõik host bittid = 0)
Broadcast address: 192.168.1.255 (kõik host bittid = 1)
```

### Harjutused:

**51.** `192.168.5.0/24`
- Network: __________________
- Broadcast: __________________

**52.** `172.16.0.0/16`
- Network: __________________
- Broadcast: __________________

**53.** `10.0.0.0/8`
- Network: __________________
- Broadcast: __________________

**54.** `192.168.10.0/24`
- Network: __________________
- Broadcast: __________________

**55.** `172.20.0.0/16`
- Network: __________________
- Broadcast: __________________

**56.** `10.50.0.0/16`
- Network: __________________
- Broadcast: __________________

**57.** `192.168.100.0/24`
- Network: __________________
- Broadcast: __________________

**58.** `172.30.0.0/16`
- Network: __________________
- Broadcast: __________________

**59.** `10.100.0.0/16`
- Network: __________________
- Broadcast: __________________

**60.** `192.168.200.0/24`
- Network: __________________
- Broadcast: __________________

---

## OSA 8: HOST AADRESSIDE VAHEMIK (10 punkti)

Leia iga võrgu esimene ja viimane kasutatav host aadress.

**Näide:**
```
Võrk: 192.168.1.0/24

Network: 192.168.1.0 (ei saa kasutada)
Esimene host: 192.168.1.1
Viimane host: 192.168.1.254
Broadcast: 192.168.1.255 (ei saa kasutada)
```

### Harjutused:

**61.** `192.168.5.0/24`
- Esimene host: __________________
- Viimane host: __________________

**62.** `172.16.0.0/16`
- Esimene host: __________________
- Viimane host: __________________

**63.** `10.0.0.0/8`
- Esimene host: __________________
- Viimane host: __________________

**64.** `192.168.10.0/24`
- Esimene host: __________________
- Viimane host: __________________

**65.** `172.20.0.0/16`
- Esimene host: __________________
- Viimane host: __________________

---

## OSA 9: KÕIK KOKKU (20 punkti)

Järgmised harjutused kombineerivad kõiki oskusi.

**66.** IP aadress `192.168.50.100/24` kuulub võrku:
- Võrgu klass: ________
- Network address: __________________
- Broadcast address: __________________
- Esimene host: __________________
- Viimane host: __________________
- Hostide arv: ________

**67.** IP aadress `172.16.150.50/16` kuulub võrku:
- Võrgu klass: ________
- Network address: __________________
- Broadcast address: __________________
- Esimene host: __________________
- Viimane host: __________________
- Hostide arv: ________

**68.** IP aadress `10.100.200.50/8` kuulub võrku:
- Võrgu klass: ________
- Network address: __________________
- Broadcast address: __________________
- Esimene host: __________________
- Viimane host: __________________
- Hostide arv: ________

**69.** Kas IP aadressid `192.168.1.10/24` ja `192.168.1.50/24` on samas võrgus?
- JAH / EI (ringi ümber)
- Põhjendus: _________________________________________________

**70.** Kas IP aadressid `192.168.1.10/24` ja `192.168.2.10/24` on samas võrgus?
- JAH / EI (ringi ümber)
- Põhjendus: _________________________________________________

---

## BOONUS: KEERULISEMAD HARJUTUSED (+10 punkti)

**71.** Teisenda IP aadress `255.255.255.0` täielikult binary kujule ja selgita, miks see on tüüpiline Class C subnet mask.

Vastus: ________________________________________________________

**72.** Kui palju host bitte on vaja, et mahutada vähemalt 50 hosti? Milline oleks sobiv subnet mask CIDR kujul?

Vastus: ________________________________________________________

**73.** Miks ei saa me määrata 192.168.1.0 ega 192.168.1.255 ühelegi arvutile võrgus 192.168.1.0/24?

Vastus: ________________________________________________________

**74.** Kui sul on võrk 172.16.0.0/16, siis millisesse klassi see kuulub ja mitu hosti maksimaalset mahub sellesse võrku?

Vastus: ________________________________________________________

**75.** WAN linkide jaoks kasutatakse sageli /30 subnet mask. Mitu hosti mahub /30 võrku ja miks see on sobiv WAN linkideks?

Vastus: ________________________________________________________

---

**KOKKU: 100 punkti + 10 boonus**