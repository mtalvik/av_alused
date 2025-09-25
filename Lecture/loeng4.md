# Loeng 4: Ruuteri seadistamine - Packet Tracer praktika

*Tere! Täna õpime, kuidas kaks erinevat võrku omavahel rääkima panna. Kasutame Packet Tracerit!*

## Meenutame eelmist nädalat (5 min)

**Switch:** Ühendab arvuteid **samas** võrgus
**Ruuter:** Ühendab **erinevaid** võrke

Täna teeme ruuteri tööle!

## Osa 1: Mis on ruuter? (10 min)

### Ruuter on nagu postkontori sorteerija

**Switch** = Korteri postkastid (kõik samas majas)
**Ruuter** = Postkontor (saadab kirju eri majadesse)

```
Maja A (192.168.1.x) ---[RUUTER]--- Maja B (192.168.2.x)
```

Ilma ruuterita need kaks "maja" ei saa omavahel suhelda!

## Osa 2: Avame Packet Tracer (5 min)

### Ehitame lihtsa võrgu koos

**SAMM 1:** Lisa komponendid
1. Võta 1 ruuter (Router 2911)
2. Võta 2 switchi (2960)
3. Võta 4 arvutit (PC)

**SAMM 2:** Ühenda kaablitega
```
[PC0]---[Switch0]---[Router]---[Switch1]---[PC2]
[PC1]---↑                           ↑---[PC3]
```

⚠️ **NB! Ruuter-Switch = sirge kaabel (mitte rist!)**

## Osa 3: Anname arvutitele IP aadressid (10 min)

### Vasak pool - Võrk A (192.168.1.0)

**PC0:**
- IP: 192.168.1.10
- Mask: 255.255.255.0
- Gateway: 192.168.1.1

**PC1:**
- IP: 192.168.1.11
- Mask: 255.255.255.0
- Gateway: 192.168.1.1

### Parem pool - Võrk B (192.168.2.0)

**PC2:**
- IP: 192.168.2.10
- Mask: 255.255.255.0
- Gateway: 192.168.2.1

**PC3:**
- IP: 192.168.2.11
- Mask: 255.255.255.0
- Gateway: 192.168.2.1

**Küsimus klassile:** Miks gateway on .1 mõlemal pool?
*Vastus: See on ruuteri aadress selles võrgus!*

## Osa 4: Seadistame ruuteri - DEMO (15 min)

### Klikime ruuteril → CLI

```cisco
--- Vajuta ENTER et alustada ---

Router>
Router> enable
Router#
```

**Seletame:** 
- `>` = Tavakasutaja (nagu külaline)
- `#` = Admin (nagu omanik)

### Anname ruuterile nime

```cisco
Router# configure terminal
Router(config)# hostname Kool-Ruuter
Kool-Ruuter(config)#
```

### Seadistame vasaku pordi (GigabitEthernet 0/0)

```cisco
Kool-Ruuter(config)# interface gigabitEthernet 0/0
Kool-Ruuter(config-if)# ip address 192.168.1.1 255.255.255.0
Kool-Ruuter(config-if)# no shutdown
```

**Mis just juhtus:**
1. Valisime pordi g0/0
2. Andsime IP 192.168.1.1 (gateway võrk A jaoks)
3. Lülitasime pordi sisse

*Ootame kuni port muutub roheliseks!*

### Seadistame parema pordi (GigabitEthernet 0/1)

```cisco
Kool-Ruuter(config-if)# exit
Kool-Ruuter(config)# interface gigabitEthernet 0/1
Kool-Ruuter(config-if)# ip address 192.168.2.1 255.255.255.0
Kool-Ruuter(config-if)# no shutdown
```

### Vaatame kas töötab

```cisco
Kool-Ruuter(config-if)# end
Kool-Ruuter# show ip interface brief

Interface         IP-Address      Status    Protocol
GigabitEthernet0/0    192.168.1.1     up        up    ✓
GigabitEthernet0/1    192.168.2.1     up        up    ✓
```

Mõlemad pordid "up up" = töötab!

## Osa 5: Testime võrku (10 min)

### Test 1: Sama võrk

**PC0 → PC1** (mõlemad võrgus A)
1. Kliki PC0
2. Desktop → Command Prompt
3. `ping 192.168.1.11`

**Tulemus:** Reply! ✅

### Test 2: Üle ruuteri

**PC0 → PC2** (eri võrkudes)
1. PC0 Command Prompt
2. `ping 192.168.2.10`

**Tulemus:** Reply! ✅

*Ruuter teeb oma tööd - ühendab võrgud!*

### Test 3: Mis juhtub kui ruuter on maas?

1. Lülita ruuter välja (kliki ja Power off)
2. Proovi uuesti PC0 → PC2
3. **Tulemus:** Request timed out ❌

*Ilma ruuterita eri võrgud ei saa suhelda!*

## Osa 6: Nüüd teie kord! (20 min)

### Harjutus: Ehitage oma võrk

**Ülesanne:**
```
Kontor (10.0.1.0) ---[Ruuter]--- Ladu (10.0.2.0)
    2 arvutit                       2 arvutit
```

**Sammud:**
1. Lisa ruuter + 2 switchi + 4 PC
2. Ühenda kaablitega
3. **Kontor:** 10.0.1.10, 10.0.1.11 (gateway 10.0.1.1)
4. **Ladu:** 10.0.2.10, 10.0.2.11 (gateway 10.0.2.1)
5. Seadista ruuter:
   - g0/0 = 10.0.1.1
   - g0/1 = 10.0.2.1
6. Testi pingiga!

**Abi käsud:**
```cisco
enable
configure terminal
interface gigabitEthernet 0/0
ip address [IP] [MASK]
no shutdown
exit
```

## Osa 7: Olulised käsud (10 min)

### Vaatame, mida ruuter "teab"

```cisco
Router# show ip route

C    192.168.1.0/24 is directly connected, GigabitEthernet0/0
C    192.168.2.0/24 is directly connected, GigabitEthernet0/1
```

**C = Connected** (otse ühendatud)

### Käsud mida päriselt vaja

**Vaatamise käsud (ei muuda midagi):**
```cisco
show version              ← Mis mudel ja tarkvara
show ip interface brief   ← Kõik pordid korraga
show running-config      ← Kõik seaded
show ip route            ← Kuhu pakette saata
```

**Seadistamise käsud:**
```cisco
enable                   ← Saa adminiks
configure terminal       ← Mine seadistustesse  
hostname NIMI           ← Anna seadmele nimi
interface g0/0          ← Vali port
ip address IP MASK      ← Anna IP aadress
no shutdown             ← Lülita sisse
exit                    ← Mine tagasi
```

**Hädaabi:**
```cisco
?                       ← Näita abi
Ctrl+C                  ← Katkesta käsk
Ctrl+Z                  ← Mine kohe admin režiimi
```

## Osa 8: Parooli lisamine ruuterile (5 min)

### Teeme ruuteri turvaliseks

```cisco
Kool-Ruuter(config)# enable secret minusalajane
Kool-Ruuter(config)# exit
Kool-Ruuter# exit

Kool-Ruuter con0 is now available
Press RETURN to get started.

User Access Verification
Password: [kirjuta minusalajane]
Kool-Ruuter#
```

Nüüd küsib parooli!

## Osa 9: Salvestamine (5 min)

### OLULINE: Salvesta oma töö!

**Packet Trackeris:**
- File → Save
- Pane nimi: "Nimi_Router_Lab.pkt"

**Ruuteri config:**
```cisco
Router# copy running-config startup-config
```

See salvestab ruuteri seaded. Muidu kaob kõik kui ruuter restardib!

## Osa 10: Mis juhtub kui... (10 min)

### Vea otsimine - nagu detektiiv

**"Ping ei tööta!"**

**Samm 1:** Kontrolli IP-d
```cisco
PC> ipconfig
    IP Address: 192.168.1.10
    Subnet Mask: 255.255.255.0
    Gateway: 192.168.1.1    ← Kas see on õige?
```

**Samm 2:** Kontrolli ruuterit
```cisco
Router# show ip interface brief

Interface    IP-Address      Status    Protocol
Gi0/0        192.168.1.1     up        up      ← Peab olema up up!
Gi0/1        192.168.2.1     down      down    ← Probleem!
```

**Samm 3:** Paranda viga
```cisco
Router# configure terminal
Router(config)# interface gi0/1
Router(config-if)# no shutdown    ← Unustasid sisse lülitada!
```

### Tüüpilised vead

| Viga | Põhjus | Lahendus |
|------|--------|----------|
| "Invalid input" | Kirjutasid valesti | Kasuta `?` abi saamiseks |
| "Incomplete command" | Käsk poolik | Vajuta TAB et lõpetada |
| Port "down down" | Pole sisse lülitatud | `no shutdown` |
| Vale gateway | IP vale | Kontrolli ja paranda |

## Kokkuvõte

### Mida õppisime:

✅ **Ruuter** ühendab erinevaid võrke
✅ Igal võrgul oma IP vahemik (192.168.1.x vs 192.168.2.x)
✅ Gateway = ruuteri aadress selles võrgus
✅ Ilma ruuterita eri võrgud ei saa suhelda

### Kodus proovimiseks:

**Challenge:** Lisa kolmas võrk!
```
Võrk A ---[Ruuter]--- Võrk B
            |
          Võrk C
```

Vihje: Ruuteril on rohkem porte (g0/2)!

## Spikker

### Ruuteri seadistamise sammud:

1. **enable** → saa adminiks
2. **configure terminal** → mine seadistustesse
3. **interface g0/0** → vali port
4. **ip address [IP] [MASK]** → anna IP
5. **no shutdown** → lülita sisse
6. **exit** → tagasi
7. **copy run start** → salvesta

### IP aadresside näited:

| Võrk | Network | Gateway | PC-d |
|------|---------|---------|------|
| Kodu | 192.168.1.0 | .1 | .10, .11, .12 |
| Kontor | 10.0.1.0 | .1 | .10, .11, .12 |
| Kool | 172.16.1.0 | .1 | .10, .11, .12 |

---

**Küsimused?** Küsi kohe - kõik õpivad!
