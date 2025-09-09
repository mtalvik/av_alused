# Kodutöö 2: Cisco CLI
**Tähtaeg:** Järgmine tund (Nädal 3)

## PACKET TRACER HARJUTUSED

**Harjutus 1.1: Laienda klassi võrku**

Ehita 5 arvutiga võrk kasutades UUSI CLI käske:

```
Võrgu skeem:
    [PC0]---[Switch0]---[PC1]
              |
         [PC2] [PC3]
              |
            [PC4]
```

**Switch seadistamine CLI-ga:**
```cisco
enable
configure terminal
hostname Klass5-SW
banner motd #Tere tulemast 5PC laborisse!#
interface range fastEthernet 0/1-5
description Arvuti_port
no shutdown
exit
interface vlan 1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
enable secret class123
line console 0
password cisco
login
exit
copy running-config startup-config
```

Kõik PC-d: IP aadressid 192.168.1.10 kuni 192.168.1.50, gateway 192.168.1.1. Testi ping ja salvesta `Labor2_5PC.pkt`.

**Harjutus 1.2: Kahe switchi võrk täiendatud käskudega**

```cisco
! Switch1 seadistus
enable
configure terminal
hostname SW-Vasak
interface gigabitEthernet 0/1
description Yhendus_SW-Parem
no shutdown
exit
interface range fa0/1-2
description PC_port
no shutdown
exit
copy run start

! Switch2 sama loogika
```

Lisa Simulation Mode test ja tee screenshot.

## OSA 2: KODU RUUTERI UURIMINE (PÄRIS SEADE!)

**SINU KODU RUUTER - Päris praktika**

**Samm 1: Leia oma ruuteri info**
- Vaata ruuteri pealt mudeli number (nt TP-Link Archer C6, Telia Thomson)
- Pildista ruuteri kleebis (seal on vaikeparoolid!)
- Otsi Google: "[ruuteri mudel] manual PDF"

**Samm 2: Logi sisse ruuteri veebiliidesesse**

Juhendis peaks olema kirjas või ISP käest.

**Samm 3: Uuri ja dokumenteeri**

Leia ruuteri menüüst ja tee screenshot:
- **WiFi seaded** - mis on su WiFi nimi (SSID)?
- **Connected Devices / DHCP Clients** - kes on ühendatud?

## CLI KÄSKUDE HARJUTAMINE - TÄIENDATUD

**Harjutus 2.1: Laiendatud CLI spikker**

```
=== CLI SPIKKER v2 ===

PÕHIKÄSUD:
enable                          - admin režiim
configure terminal              - seadistuste režiim
exit / end                      - tagasi / otse üles

PAROOLID:
enable secret PAROOL            - krüptitud admin parool
line console 0                  - konsoolipordile
  password PAROOL
  login
line vty 0 4                    - Telnet/SSH jaoks
  password PAROOL
  login

PORTIDE HALDUS:
interface fastEthernet 0/1      - vali port
  description KIRJELDUS         - lisa selgitus
  no shutdown                   - lülita sisse
  shutdown                      - lülita välja
interface range fa0/1-10        - mitu porti korraga
  no shutdown

VLAN PÕHITÕED:
interface vlan 1                - haldus VLAN
  ip address 192.168.1.1 255.255.255.0
  no shutdown

KONTROLLIMINE:
show running-config             - praegune konfig
show startup-config             - salvestatud konfig
show interfaces status          - kõik pordid tabelina
show mac address-table          - MAC tabel
show vlan brief                 - VLAN info

SALVESTAMINE:
copy running-config startup-config  - OLULINE!
write memory                         - sama asi
```