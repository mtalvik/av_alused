# CLASS A SUBNETTING

## Mis on Class A võrk?

Class A algab **1.0.0.0 – 126.255.255.255**  
(127.x.x.x on reserveeritud loopbackile)

**Vaikimisi mask: /8 = 255.0.0.0**

```
10.0.0.0 /8
└─┬──┘ └───────┬───────┘
Võrk     Hostid
(fikseeritud) (256×256×256 = 16 777 216 hosti)
```

---

## Kuidas jagada?

### Näide 1: Jaga 10.0.0.0 /8 → 4 alamvõrku

**Samm 1:** Mitu bitti?  
- 2² = 4 võrku → vaja 2 bitti

**Samm 2:** Uus mask  
```
Algne:  /8 = 255.0.0.0
                      ↓
Laename 2 bitti teisest oktetist:

Biti väärtused:  128  64  32  16   8   4   2   1
                  ─────────────────────────────────
                  1   1   0   0   0   0   0   0  = 192

Uus: /10 = 255.192.0.0
```

**Samm 3:** Võlunumber  
- Huvitav oktet: **teine**  
- Võlunumber: 256 − 192 = **64**

**Samm 4:** Alamvõrgud

| Võrk | Võrgu aadress | Broadcast | Esimene host | Viimane host | Hoste kokku |
|------|---------------|-----------|--------------|--------------|-------------|
| 1    | 10.0.0.0      | 10.63.255.255 | 10.0.0.1 | 10.63.255.254 | 4 194 302 |
| 2    | 10.64.0.0     | 10.127.255.255 | 10.64.0.1 | 10.127.255.254 | 4 194 302 |
| 3    | 10.128.0.0    | 10.191.255.255 | 10.128.0.1 | 10.191.255.254 | 4 194 302 |
| 4    | 10.192.0.0    | 10.255.255.255 | 10.192.0.1 | 10.255.255.254 | 4 194 302 |

```
Teine oktet:
0                                                               255
├───────────────┼───────────────┼───────────────┼───────────────┤
     Võrk 1          Võrk 2          Võrk 3          Võrk 4
    (0-63)         (64-127)       (128-191)       (192-255)

    Hüppab 64 võrra (võlunumber!)
```

**Hoste arv:**  
- Iga alamvõrgus on 64 × 256 × 256 = 4 194 304 aadressi  
- Miinus 2 (võrgu aadress ja broadcast) = **4 194 302 hosti**

---

### Näide 2: Jaga 12.0.0.0 /8 → 8 alamvõrku

**Samm 1:** Mitu bitti?  
- 2³ = 8 võrku → 3 bitti

**Samm 2:** Uus mask  
```
Laename 3 bitti teisest oktetist:

Biti väärtused:  128  64  32  16   8   4   2   1
                  ─────────────────────────────────
Laename 3 bitti: 1   1   1   0   0   0   0   0  = 224

Uus: /11 = 255.224.0.0
```

**Samm 3:** Võlunumber  
- 256 − 224 = **32**

**Samm 4:** Alamvõrgud (esimesed 4)

| Võrk | Võrgu aadress | Broadcast | Esimene host | Viimane host | Hoste |
|------|---------------|-----------|--------------|--------------|-------|
| 1    | 12.0.0.0      | 12.31.255.255 | 12.0.0.1 | 12.31.255.254 | 2 097 150 |
| 2    | 12.32.0.0     | 12.63.255.255 | 12.32.0.1 | 12.63.255.254 | 2 097 150 |
| 3    | 12.64.0.0     | 12.95.255.255 | 12.64.0.1 | 12.95.255.254 | 2 097 150 |
| 4    | 12.96.0.0     | 12.127.255.255 | 12.96.0.1 | 12.127.255.254 | 2 097 150 |

**Hoste arv:**  
- 32 × 256 × 256 = 2 097 152 aadressi  
- Miinus 2 = **2 097 150 hosti**

---

## Peamised erinevused Class B‑st

| Aspekt | Class B (/16) | Class A (/8) |
|--------|---------------|--------------|
| Huvitav oktet | Kolmas | Teine |
| Vaikimisi hoste | 65 534 | 16 777 214 |
| Broadcast arvutamine | Keskmine (2 oktetti) | Suur (3 oktetti) |

---

## Broadcast aadressi leidmine

**Reegel:** Järgmine võrk miinus 1

Näide: Kui võrk on 10.64.0.0 ja võlunumber 64:  
- Järgmine võrk: 10.128.0.0  
- Broadcast: 10.127.255.255

---

## Harjutus

Jaga võrk **11.0.0.0 /8** kuueteistkümneks alamvõrguks.

1. Mitu bitti? ___  
2. Uus mask? /___  
3. Teine oktet? ___  
4. Võlunumber? ___  
5. Esimene võrk: 11.___.0.0  
6. Teine võrk: 11.___.0.0  
7. Kui palju hoste ühes võrgus? ___  

---

👉 Nii saad sama loogika, mis Class B puhul, aga nüüd **teine oktet on loendur**, ja hostide arv on tohutult suurem.  

Kas tahad, et ma teeks ka **Class C versiooni** samas formaadis, et sul oleks terve komplekt A–B–C subnettingu “paberid”?
