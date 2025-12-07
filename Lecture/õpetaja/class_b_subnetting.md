# CLASS B SUBNETTING

## Mis on Class B võrk?

Class B tähendab, et aadressid algavad vahemikust **128.0.0.0 kuni 191.255.255.255**.  
Vaikimisi mask on **/16 = 255.255.0.0**.

👉 Mõtle, et IP‑aadress on **neli numbrit**, mis käituvad nagu loendurid.  
Näide: `172.16.0.0`

- Esimesed kaks numbrit (172.16) on **fikseeritud** → need ei muutu.  
- Viimased kaks numbrit (0.0) on **loendurid** → need muutuvad 0–255.

```
172.16.0.0 /16
└─┬──┘ └──┬──┘
Võrk   Hostid
(fikseeritud) (256×256 = 65536 hosti)
```

---

## Kuidas jagada?

### Näide 1: Jaga 172.16.0.0 /16 → 4 alamvõrku

**Samm 1: mitu bitti vaja?**  
Tahame 4 võrku → 2² = 4 → vaja 2 bitti.

**Samm 2: uus mask**  
Laename 2 bitti kolmandast numbrist (oktetist).  
See annab uue maski: **/18 = 255.255.192.0**

**Samm 3: võlunumber**  
256 − 192 = **64** → see on samm, kui kolmas number hüppab.

**Samm 4: alamvõrgud**

| Võrk | Võrgu aadress | Broadcast | Esimene host | Viimane host | Hoste kokku |
|------|---------------|-----------|--------------|--------------|-------------|
| 1    | 172.16.0.0    | 172.16.63.255 | 172.16.0.1 | 172.16.63.254 | 16382 |
| 2    | 172.16.64.0   | 172.16.127.255 | 172.16.64.1 | 172.16.127.254 | 16382 |
| 3    | 172.16.128.0  | 172.16.191.255 | 172.16.128.1 | 172.16.191.254 | 16382 |
| 4    | 172.16.192.0  | 172.16.255.255 | 172.16.192.1 | 172.16.255.254 | 16382 |

---

### Selgitus: loendurid

- **Neljas number (W)** loendab **väikseid samme**: 0 → 255  
- Kui W jõuab 255‑ni, siis **kolmas number (Z)** suureneb ühe võrra.  
- See on täpselt nagu matemaatikas arvutamine:  
  - 99 → 100 (üks koht nullitakse, järgmine suureneb)  
  - IP‑s: `172.16.0.255 → 172.16.1.0`

---

## Näide 2: Jaga 10.50.0.0 /16 → 8 alamvõrku

- 2³ = 8 võrku → 3 bitti  
- Uus mask: **/19 = 255.255.224.0**  
- Võlunumber: 256 − 224 = **32**

| Võrk | Võrgu aadress | Broadcast | Esimene host | Viimane host | Hoste |
|------|---------------|-----------|--------------|--------------|-------|
| 1    | 10.50.0.0     | 10.50.31.255 | 10.50.0.1 | 10.50.31.254 | 8190 |
| 2    | 10.50.32.0    | 10.50.63.255 | 10.50.32.1 | 10.50.63.254 | 8190 |
| 3    | 10.50.64.0    | 10.50.95.255 | 10.50.64.1 | 10.50.95.254 | 8190 |
| 4    | 10.50.96.0    | 10.50.127.255 | 10.50.96.1 | 10.50.127.254 | 8190 |

---

## Peamised erinevused Class C‑st

| Aspekt | Class C (/24) | Class B (/16) |
|--------|---------------|---------------|
| Huvitav number | Neljas | Kolmas |
| Vaikimisi hoste | 254 | 65534 |
| Broadcast arvutamine | Lihtne (1 number) | Suurem (2 numbrit) |

---

## Harjutus

Jaga võrk **192.168.0.0 /16** kuueteistkümneks alamvõrguks.

1. Mitu bitti? ___  
2. Uus mask? /___  
3. Kolmas number? ___  
4. Võlunumber? ___  
5. Esimene võrk: 192.168.___.0  
6. Teine võrk: 192.168.___.0  
7. Kui palju hoste ühes võrgus? ___  

---

👉 Nii saab mõelda IP‑aadressidest nagu **suured numbrid, mis loendavad**. Kolmas ja neljas number on lihtsalt **loendurid**, mis töötavad koos — täpselt nagu matemaatikas, kui üks koht täis saab, järgmine suureneb.  

Kas tahad, et ma joonistaks ka **visuaalse number‑joone** (graafiku), kus näeb, kuidas kolmas ja neljas oktet liiguvad?
