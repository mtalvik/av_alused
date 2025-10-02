# Lab 4: Loo ruuteri ja kommutaatoriga võrk

## Eesmärk
Seadista lihtne võrk, kus kaks arvutit suhtlevad omavahel läbi ruuteri ja kommutaatori. Õpid, kuidas seadistada ruuteri ja kommutaatori IP-aadresse ning kontrollida ühendust.

## Vahendid

### Riistvara
- **1 ruuter Cisco 4221** (näiteks R1)
  - *Välimus*: Tavaliselt metallkarbis, millel on esipaneelil mitmed pordid, sealhulgas Gigabit Etherneti pordid (G0/0/0 ja G0/0/1), USB-pordid ning konsoolipordid seadistamiseks

- **1 kommutaator Cisco Catalyst 2960** (näiteks S1)
  - *Välimus*: Metallist karp, mille esipaneelil on 24 või 48 Etherneti porti, ning mõned mudelid võivad sisaldada ka Gigabit Etherneti pordid või konsoolipordi seadistamiseks

- **2 arvutit** (PC-A ja PC-B)
- **Etherneti kaablid**

## Võrgu topoloogia

```
[PC-A] -------- [S1 Kommutaator] -------- [PC-B]
                       |
                       |
                  [R1 Ruuter]
```

### Ühendused:
- PC-A → S1 port 1
- PC-B → S1 port 2  
- R1 → S1 port 5

## IP-aadresside jaotus

| Seade | Liides | IP-aadress | Alamvõrgu mask | Vaikelüüs |
|-------|--------|------------|----------------|-----------|
| R1 | G0/0/0 | 192.168.0.1 | 255.255.255.0 | - |
| R1 | G0/0/1 | 192.168.1.1 | 255.255.255.0 | - |
| S1 | VLAN 1 | 192.168.1.2 | 255.255.255.0 | 192.168.1.1 |
| PC-A | NIC | 192.168.0.3 | 255.255.255.0 | 192.168.0.1 |
| PC-B | NIC | 192.168.1.3 | 255.255.255.0 | 192.168.1.1 |

## Sammud

### 1. Seadmete ühendamine
1. Ühenda **PC-A** kommutaatori S1 porti 1
2. Ühenda **PC-B** kommutaatori S1 porti 2
3. Ühenda **Ruuter R1** kommutaatori S1 porti 5
4. Lülita kõik seadmed sisse

### 2. Seadista IP-aadressid arvutites
1. Määra PC-A ja PC-B IP-aadressid, alamvõrgu maskid ja vaikelüüsid vastavalt tabelile
2. Kontrolli, et IP-aadressid on õigesti määratud:
   ```cmd
   ipconfig
   ```

### 3. Seadista ruuter

Avage ruuteri konsooliaken ja sisesta järgmised käsud:

```cisco
enable
config terminal
hostname R1

! Seadista esimene liides
interface g0/0/0
ip address 192.168.0.1 255.255.255.0
no shutdown
exit

! Seadista teine liides  
interface g0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit

! Luba IPv6 ruutimine
ipv6 unicast-routing

! Salvesta konfiguratsioon
exit
copy running-config startup-config
```

### 4. Seadista kommutaator

Avage kommutaatori konsooliaken ja sisesta järgmised käsud:

```cisco
enable
config terminal
hostname S1

! Seadista haldusliides
interface vlan 1
ip address 192.168.1.2 255.255.255.0
no shutdown
exit

! Määra vaikelüüs
ip default-gateway 192.168.1.1

! Salvesta konfiguratsioon
exit
copy running-config startup-config
```

### 5. Kontrolli ühendust

#### PC-A käsurealt:
```cmd
# Testi ühendust PC-B-ga
ping 192.168.1.3

# Testi ühendust ruuteriga
ping 192.168.0.1
```

#### PC-B käsurealt:
```cmd
# Testi ühendust PC-A-ga
ping 192.168.0.3

# Testi ühendust ruuteriga
ping 192.168.1.1
```

## Oodatav tulemus
Kui kõik on õigesti seadistatud, peaksid nägema edukaid ping vastuseid kõigi seadmete vahel. Vastused peaksid näitama:
- Reply from [IP-aadress]: bytes=32 time<1ms TTL=64
- 0% packet loss

## Veaotsing

### Kui ping ei tööta:
1. **Kontrolli füüsilisi ühendusi** - kaablid peavad olema korralikult ühendatud
2. **Veendu, et liidesed on aktiivsed** - kasuta ruuteril/kommutaatoril käsku `show ip interface brief`
3. **Kontrolli IP-seadeid** - kõik seadmed peavad olema õiges alamvõrgus
4. **Kontrolli vaikelüüsi** - PC-del peab olema õige gateway määratud

### Kasulikud käsud veaotsinguks:
```cisco
show running-config
show ip interface brief  
show interfaces status
show vlan brief
```

## Lisaülesanded

1. **Lisa kolmas arvuti** võrku ja seadista sellele IP-aadress 192.168.0.4
2. **Seadista DHCP** ruuteril, et IP-aadressid jagataks automaatselt
3. **Lisa teine VLAN** kommutaatoris ja eralda PC-d erinevatesse VLANidesse

## Märkused
- Ära unusta salvestada konfiguratsioone käsuga `copy running-config startup-config`
- IPv6 ruutimise lubamine on valikuline, kuid soovitatav tuleviku jaoks
- Võrgu turvalisuse parandamiseks lisa paroolid ja krüpteeri need

---

## Labi esitamise nõuded

### Mida peate esitama:

#### 1. PKT fail (Packet Tracer)
- Salvestage oma Packet Tracer fail nimega: `Lab4_[TeieNimi].pkt`
- Fail peab sisaldama täielikult seadistatud võrku
- **OLULINE:** Ruuteri hostname peab olema teie nimi (nt `hostname JohnSmith`)
- **OLULINE:** Kommutaatori hostname peab olema teie nimi koos S-tähega (nt `hostname JohnSmith-S`)

#### 2. Ekraanipildid (2 tükki)

**Ekraanipilt 1: Edukad ping testid**
- Näidake PC-A käsurealt ping PC-B poole
- Tulemused peavad näitama edukaid vastuseid (0% packet loss)

**Ekraanipilt 2: Seadmete konfiguratsioon**
- Ruuteri käsk: `show running-config | section hostname|interface`
- Peab olema näha teie nimi hostname'is

#### 3. Vastused küsimustele

Vastake kirjalikult järgmistele küsimustele (1-2 lauset iga küsimuse kohta):

**Küsimus 1:** Miks on vaja seadistada vaikelüüs (default gateway) arvutites?

**Küsimus 2:** Mis on erinevus kommutaatori ja ruuteri vahel võrgu toimimises?

**Küsimus 3:** Miks kasutasime käsku `no shutdown` liideste seadistamisel?

### Seadistamise meeldetuletus hostname'ide jaoks:

**Ruuteri jaoks:**
```cisco
enable
configure terminal
hostname [TeieNimi]
```

**Kommutaatori jaoks:**
```cisco
enable
configure terminal
hostname [TeieNimi]-S
```

### Esitamise kontroll-loend:

- [ ] PKT fail on salvestatud õige nimega
- [ ] Ruuteri hostname on teie nimi
- [ ] Kommutaatori hostname on teie nimi koos -S lõpuga
- [ ] Kõik IP-aadressid on õigesti seadistatud
- [ ] Ping testid töötavad
- [ ] 2 ekraanipilti on tehtud
- [ ] 3 küsimusele on vastatud

### Esitamine Google Classroom'is:

**Esitamise tähtaeg:** [Õpetaja määrab]

**Esitamiseks:**
1. Avage Google Classroom'is Lab 4 ülesanne
2. Lisage manused järgmises järjekorras:
   - `Lab4_[TeieNimi].pkt` - Packet Tracer fail
   - `Screenshot1_ping.png` - Ekraanipilt ping testist
   - `Screenshot2_config.png` - Ekraanipilt seadmete konfiguratsioonist
   - `Vastused_Lab4.txt` või `.docx` - Vastused 3 küsimusele

3. Klõpsake "Turn in" / "Esita"

**NB!** Ärge esitage tööd "Mark as done" nupuga - failid peavad olema manustatud!
