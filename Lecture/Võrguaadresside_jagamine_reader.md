# VÕRGUAADRESSIDE JAGAMINE

## Probleem

Sinu juht annab võrgu **192.168.2.0 /24** ja ütleb: jaga **4 võrdseks** alamvõrguks.

```
     ┌─────────┬─────────┐
     │  MÜÜK   │   HR    │
     ├─────────┼─────────┤
     │  ADMIN  │   IT    │
     └─────────┴─────────┘
```

## Lahendus sammhaaval

### 1. Kui palju bite vajame võrkude jaoks?

Tahame 4 võrku. Binaarses süsteemis:
- 1 bitt annab: 2¹ = 2 võrku
- 2 bitti annab: 2² = 4 võrku ✓

**Vajame 2 biti.**

### 2. Mis on uus mask?

Algne /24 tähendab:
```
255.255.255.0
           └── siin on 8 biti hostidele (kõik nullid)
```

Võtame hostidelt 2 biti võrkudele:
```
Biti väärtused:  128  64  32  16   8   4   2   1
                  ─────────────────────────────────
Algne:             0   0   0   0   0   0   0   0  = 0
                   ↓   ↓
Võtame 2 biti:     1   1   0   0   0   0   0   0  = 128+64 = 192
```

Uus mask: **255.255.255.192** ehk **/26**

### 3. Võlunumber

Võlunumber = 256 - viimane oktet  
Võlunumber = 256 - 192 = **64**

See tähendab: iga võrk on 64 aadressi suur.

### 4. Millised on võrgud?

Alusta 0-st ja lisa võlunumbrit:

```
192.168.2.0     ┐
                │ 64 aadressi
192.168.2.63    ┘
                
192.168.2.64    ┐
                │ 64 aadressi  
192.168.2.127   ┘

192.168.2.128   ┐
                │ 64 aadressi
192.168.2.191   ┘

192.168.2.192   ┐
                │ 64 aadressi
192.168.2.255   ┘
```

| Osakond | Võrgu aadress | Broadcast | Hostid |
|---------|---------------|-----------|--------|
| MÜÜK    | 192.168.2.0   | .63       | .1-.62 |
| HR      | 192.168.2.64  | .127      | .65-.126 |
| ADMIN   | 192.168.2.128 | .191      | .129-.190 |
| IT      | 192.168.2.192 | .255      | .193-.254 |

