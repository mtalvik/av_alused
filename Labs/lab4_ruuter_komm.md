# Lab 4: Loo ruuteri ja kommutaatoriga võrk

## Eesmärk
Seadista võrk, kus kaks arvutit suhtlevad omavahel läbi kommutaatori ja ruuteri.

## Vahendid
- 1 ruuter Cisco 4221 (R1)
- 1 kommutaator Cisco 2960 (S1)
- 2 arvutit (PC-A ja PC-B)
- Etherneti kaablid

## Võrgu topoloogia

```
[PC-A] -------- [S1 Kommutaator] -------- [PC-B]
                       |
                       |
                  [R1 Ruuter]
```

## IP-aadressid

| Seade | Liides | IP-aadress | Alamvõrgu mask | Vaikelüüs |
|-------|--------|------------|----------------|-----------|
| R1 | G0/0/0 | 192.168.0.1 | 255.255.255.0 | - |
| S1 | VLAN 1 | 192.168.0.2 | 255.255.255.0 | 192.168.0.1 |
| PC-A | NIC | 192.168.0.3 | 255.255.255.0 | 192.168.0.1 |
| PC-B | NIC | 192.168.0.4 | 255.255.255.0 | 192.168.0.1 |

## Sammud

### 1. Ühenda seadmed
- PC-A → S1 port 1
- PC-B → S1 port 2  
- R1 → S1 port 5

### 2. Seadista PC-A
- IP: 192.168.0.3
- Mask: 255.255.255.0
- Gateway: 192.168.0.1

### 3. Seadista PC-B
- IP: 192.168.0.4
- Mask: 255.255.255.0
- Gateway: 192.168.0.1

### 4. Seadista ruuter

```
enable
configure terminal
hostname [TeieNimi]
interface g0/0/0
ip address 192.168.0.1 255.255.255.0
no shutdown
exit
exit
copy running-config startup-config
```

### 5. Seadista kommutaator

```
enable
configure terminal
hostname [TeieNimi]-S
interface vlan 1
ip address 192.168.0.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.0.1
exit
copy running-config startup-config
```

### 6. Testi ühendust

PC-A:
```
ping 192.168.0.4
```

PC-B:
```
ping 192.168.0.3
```

## Esitamine Google Classroom'is

### Esita 3 asja:

1. **PKT fail**: `Lab4_[TeieNimi].pkt`

2. **2 ekraanipilti**:
   - Ekraanipilt 1: Ping test
   - Ekraanipilt 2: Ruuteri konfiguratsioon (show running-config)

3. **Vastused küsimustele**:
   - Küsimus 1: Miks on vaja vaikelüüsi?
   - Küsimus 2: Mis vahe on ruuteril ja kommutaatoril?
   - Küsimus 3: Miks kasutame käsku no shutdown?

### Kontroll-loend
- [ ] Ruuteri hostname on teie nimi
- [ ] Kommutaatori hostname on teie nimi koos -S
- [ ] Ping testid töötavad
- [ ] Failid on Google Classroom'i üles laetud
