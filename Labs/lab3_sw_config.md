# PT Lab 2.1: Põhilise kommutaatori seadistamine

## Labori eesmärk
Õppida Cisco kommutaatori põhilisi seadistusi ja CLI käske.

## Seadmed
- 1x Cisco 2960 kommutaator
- 1x arvuti (PC-A)

## Topoloogia
```
[PC-A]----[S1]
         Fa0/6
```

## Ülesanded

### 1. Ühenda seadmed
1. Lisa Packet Traceris:
   - 1x Switch 2960
   - 1x PC
2. Ühenda PC-A kommutaatori porti Fa0/6

### 2. Seadista PC-A
- IP aadress: 192.168.1.2
- Alamvõrgu mask: 255.255.255.0
- Vaikimisi lüüs: 192.168.1.1

### 3. Alusta kommutaatori seadistamist
1. Kliki kommutaatoril
2. Vali "CLI" vahekaart
3. Kui küsib "Continue with configuration dialog?", vasta: **no**
4. Vajuta Enter

### 4. Sisene privilegeeritud režiimi
```
Switch>enable                    # Läheb privilegeeritud režiimi (märkus: # prompt)
Switch#
```

### 5. Vaata praegust konfiguratsiooni
```
Switch#show running-config       # Näitab hetke töötavat seadistust
```

### 6. Mine globaalsesse konfigureerimise režiimi
```
Switch#configure terminal        # Läheb seadistamise režiimi
Switch(config)#                  # Märkus: (config) prompt
```

### 7. Anna kommutaatorile nimi
```
Switch(config)#hostname S1       # Annab kommutaatorile nime S1
S1(config)#                      # Märkus: nimi muutus kohe
```

### 8. Kaitse privilegeeritud režiim parooliga
```
S1(config)#enable secret class   # Seadistab krüpteeritud parooli "class"
```

### 9. Keela IP domeeni otsimine
```
S1(config)#no ip domain-lookup   # Takistab valesti sisestatud käske DNS-ist otsimast
```

### 10. Seadista konsooliühenduse parool
```
S1(config)#line console 0       # Läheb konsooliühenduse seadistustesse
S1(config-line)#password cisco   # Määrab parooli "cisco"
S1(config-line)#login           # Nõuab sisselogimisel parooli
S1(config-line)#exit            # Tagasi config režiimi
```

### 11. Seadista virtuaalterminali (VTY) parool
```
S1(config)#line vty 0 15        # Seadistab kõik 16 virtuaalterminali
S1(config-line)#password cisco   # Määrab parooli "cisco"
S1(config-line)#login           # Nõuab sisselogimisel parooli
S1(config-line)#exit
```

### 12. Krüpteeri kõik paroolid
```
S1(config)#service password-encryption   # Krüpteerib kõik lihtteksti paroolid
```

### 13. Lisa sisselogimise bänner
```
S1(config)#banner motd #
************************************************
HOIATUS! Volitamata ligipääs on keelatud!
************************************************
#
```

### 14. Seadista kommutaatori haldusaadress
```
S1(config)#interface vlan 1              # VLAN 1 on vaikimisi haldusliides
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#no shutdown               # Lülitab liidese sisse
S1(config-if)#exit
```

### 15. Salvesta konfiguratsioon
```
S1(config)#exit
S1#copy running-config startup-config   # Salvestab seadistused püsimällu
Destination filename [startup-config]?  # Vajuta Enter
```

### 16. Kuva uuesti konfiguratsioon
```
S1#show running-config          # Kontrolli, et kõik seadistused on paigas
```

### 17. Kuva liideste info
```
S1#show ip interface brief      # Näitab kõiki liideseid lühidalt
```

### 18. Testi ühendust PC-A-st
1. Ava PC-A Command Prompt
2. Testi: `ping 192.168.1.1`

## Kontrollküsimused
1. Mis vahe on `>` ja `#` promptil?
2. Miks peame kasutama "no shutdown" käsku?
3. Mis juhtub kui ei salvesta konfiguratsiooni?

## Väljakutse
Lisa teine PC (PC-B) porti Fa0/18 IP-ga 192.168.1.3 ja testi ühendust.

*Salvesta fail: Nimi_Lab2.1.pkt*
