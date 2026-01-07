# Tunniplaan: Nädal 16 - Dünaamiline marsruutimine

## Tunni info
- **Teema:** Dünaamiline marsruutimine (RIP, OSPF, EIGRP)
- **Kestus:** 2 x 45 min = 90 min
- **Labori number:** Lab 10

**NB! Õpilased tulevad koolivaheajalt - kordamine vajalik!**

---

## Aja jaotus (70-20-10 mudel)

| Osa | Aeg | % |
|-----|-----|---|
| Teooria/kordamine | ~15 min | ~17% |
| Lab (praktika) | ~75 min | ~83% |

---

## Tunni struktuur

### 1. TUND (45 min)

| Aeg | Tegevus | Kirjeldus |
|-----|---------|-----------|
| 0-5 | **Kordamine** | "Mis on routing table? Mida `ip route` tegi?" |
| 5-10 | **Probleem** | "50 ruuterit, 200 võrku - käsitsi?" |
| 10-15 | **3 protokolli** | RIP/OSPF/EIGRP tabel tahvlile |
| 15-45 | **Lab 10** | Topoloogia + IP + RIP konfig |

### 2. TUND (45 min)

| Aeg | Tegevus | Kirjeldus |
|-----|---------|-----------|
| 0-40 | **Lab 10 jätkamine** | Link failure + OSPF + EIGRP |
| 40-45 | **Kokkuvõte** | Kiire kordamine, kodutöö |

**Lab jätkub kodutööna** kui ei jõua.

---

## Teooria (15 min) - KIIRESTI!

### Kordamine (5 min)
- Mis on routing table?
- Mida `ip route` tegi?

### Probleem (2 min)
- 50 ruuterit × 200 marsruuti = võimatu käsitsi
- Link katki = sina parandad

### 3 protokolli tabel (8 min)

| | RIP | OSPF | EIGRP |
|--|-----|------|-------|
| Metric | hop count | cost | bandwidth+delay |
| AD | 120 | 110 | 90 |
| Kood | R | O | D |

**"Madalam AD võidab!"**

---

## Lab 10 fookus (75 min)

1. **Topoloogia + IP** (~20 min)
2. **RIP konfig + test** (~15 min)
3. **Link failure test** (~15 min) ← KÕIGE OLULISEM!
4. **OSPF** (~15 min)
5. **EIGRP + võrdlus** (~10 min)

---

## Võimalikud probleemid

| Probleem | Lahendus |
|----------|----------|
| Ei mäleta IP konfigureerimist | Viita lab9-le |
| RIP `network` käsk vale | Võrgu ID, mitte interface IP! |
| OSPF ei tööta | Wildcard mask! |
| EIGRP ei tööta | AS number sama kõigil? |

---

## Kodutöö

- Lab 10 lõpetamine
- **Esitada:** .pkt + .pdf

---

## Märkmed tunni järel

_________________________________
