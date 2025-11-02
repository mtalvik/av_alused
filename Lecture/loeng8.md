# Nädal 8: OSI Mudel - 2. OSA (Võrgukiht)

## ÕPPEVÄLJUNDID

**Tead:**
- Mis on võrgukiht (Layer 3) ja miks seda vajame
- Erinevust MAC ja IP aadresside vahel
- Mis on router ja kuidas see switchist erineb
- Mis on default gateway

**Oskad:**
- Ühenduda routerisse konsooli kaudu
- Kasutada käsku `show ip interface brief`
- Vaadata routeri routing table-t
- Selgitada, kuidas paketid võrkude vahel liiguvad

## SISSEJUHATUS

See materjal tutvustab sulle võrgukihti ehk Layer 3. Võrgukiht on OSI mudeli kolmas kiht, mis vastutab IP aadresside ja võrkude vahelise suhtluse eest. Järgmisel nädalal õpime IPv4 aadresse detailsemalt, aga täna vaatame üldist pilti.

## 1. MIS ON VÕRGUKIHT?

### Layer 3 koht OSI mudelis

Võrgukiht asub OSI mudeli keskel, Layer 2 (Data Link) ja Layer 4 (Transport) vahel. Eelmisel nädalal õppisime Layer 2 kohta, kus kasutatakse MAC aadresse ja switch'e. Layer 3 töötab IP aadressidega ja routeritega.

```
Layer 7 - Application    ← Rakendused nagu browser
Layer 6 - Presentation   
Layer 5 - Session        
Layer 4 - Transport      ← TCP ja UDP
Layer 3 - Network        ← IP aadressid ja Router (TÄNA!)
Layer 2 - Data Link      ← MAC aadressid ja Switch (eelmine nädal)
Layer 1 - Physical       ← Kaablid ja signaalid
```

![OSI vs TCP/IP mudelid](/Labs/images/osi_vs_tcpip_models.png)

### Mida Layer 3 teeb?

Võrgukihi põhiülesanne on ühendada erinevaid võrke omavahel. Switch (Layer 2) oskab ainult ühendada seadmeid samas võrgus, aga ei tea midagi teistest võrkudest. Router (Layer 3) aga oskab ühendada erinevaid võrke ja saata pakette nende vahel.

Layer 3 annab igale seadmele IP aadressi. IP aadress on nagu postiaadress - see näitab, kus seade asub ja kuidas talle andmeid saata. Ilma IP aadressita ei saa arvuti internetis suhelda.

Kolmas oluline asi, mida Layer 3 teeb, on otsustamine. Kui pakett peab liikuma ühest võrgust teise, siis router otsustab, millisest teest see pakett sinna jõuab. Selleks kasutab router erilist tabelit, mida nimetatakse routing table'iks.

![Võrgukihi funktsioonid](/Labs/images/network_layer_functions.png)

### Analoogia postisüsteemiga

Kujuta ette, et sul on suur kortermaja. Maja sees kannab postiljon kirju ühest korterist teise - ta teab ainult korteri numbreid ja ei pea teadma, kus see maja asub. See on nagu Layer 2 ja Switch.

Aga kui kiri peab minema teise linna, siis on vaja postkontori, kes teab kõikide linnade aadresse ja oskab kirja õigesse linna saata. See postkontor on nagu Layer 3 ja Router.

## 2. MIKS VAJAME LAYER 3?

### Probleem: Layer 2 ei piisa

Kujuta ette, et sinu firmas on kaks osakondi: IT osakond ja raamatupidamine. Mõlemad osakonnad on ühendatud oma switch'iga ja töötavad oma võrgus.

```
        IT OSAKOND                      RAAMATUPIDAMINE

[PC1]  [PC2]  [PC3]              [PC4]  [PC5]  [PC6]
   \     |     /                     \     |     /
   [Switch A]                        [Switch B]
```

Kui PC1 tahab saata faili PC4-le, siis tekib probleem. Switch A ei tea midagi Switch B võrgust ja vastupidi. MAC aadressid töötavad ainult samas võrgus, mitte võrkude vahel. Seega kaks võrku ei saa omavahel suhelda.

### Lahendus: Router

Router on seade, mis ühendab erinevaid võrke. Router paigutatakse kahe võrgu vahele ja ta oskab suunata pakette ühest võrgust teise.

```
        IT OSAKOND                      RAAMATUPIDAMINE

[PC1]  [PC2]  [PC3]              [PC4]  [PC5]  [PC6]
   \     |     /                     \     |     /
   [Switch A] -------- [ROUTER] -------- [Switch B]
```

Nüüd saab PC1 saata paketi PC4-le. Pakett läheb PC1-lt Switch A-sse, sealt routerisse, routerist Switch B-sse ja lõpuks PC4-le. Router teab, kuidas mõlemaid võrke omavahel ühendada.

## 3. MAC vs IP AADRESSID

### Kaks erinevat aadressi

Võrgus on igal seadmel kaks aadressi: MAC aadress ja IP aadress. Need töötavad koos, aga nende eesmärgid on erinevad.

MAC aadress on nagu korteri number majas. See on püsiv ja kehtib ainult ühes võrgus. MAC aadress on 48 bitti pikk ja kirjutatakse heksadetsimaalis, näiteks AA:BB:CC:DD:EE:FF. Iga võrgukaart saab oma MAC aadressi juba tehases ja seda ei saa muuta.

IP aadress on nagu täielik postiaadress. See kehtib kogu maailmas ja seda saab muuta. IPv4 aadress on 32 bitti pikk ja kirjutatakse desimaalis, näiteks 192.168.1.10. Administrator saab IP aadressi seadmele määrata või see tuleb automaatselt DHCP serverist.

### Võrdlus tabelina

MAC ja IP aadressid erinevad mitmel viisil. MAC on Layer 2 aadress ja IP on Layer 3 aadress. MAC on 48 bitti ja IP on 32 bitti. MAC-i ei saa muuta, aga IP-d saab. MAC töötab ainult samas võrgus, aga IP töötab kogu internetis.

![MAC vs IP](/Labs/images/mac_vs_ip.png)

### Kuidas nad koos töötavad?

Mõtle sellele nii: kui arvuti PC1 tahab saata paketti arvutile PC2, mis on teises võrgus, siis juhtub järgmine. Paketile pannakse peale kaks aadressi: IP aadress näitab, kus PC2 asub (lõppsihtpunkt), ja MAC aadress näitab, kuhu pakett praegu läheb (esimene samm).

Esimene samm on saata pakett routerile. Seega MAC aadress on routeri MAC. IP aadress on juba lõplik - PC2 IP. Kui router saab paketi kätte, ta vaatab IP aadressi ja mõistab, et pakett peab minema teise võrku. Router muudab MAC aadressi - nüüd MAC on PC2 oma - ja saadab paketi edasi.

Oluline on meeles pidada: IP aadress püsib sama kogu tee jooksul, aga MAC aadress muutub iga routeri juures.

![ARP Diagramm](/Labs/images/arp_diagram.png)

---

## 4. ROUTER - LAYER 3 SEADE

### Mis on router?

Router on võrguseade, mis töötab Layer 3 tasemel. Routeri peamine ülesanne on ühendada erinevaid võrke ja otsustada, kuhu paketid saata. Switch ühendab seadmeid samas võrgus, aga router ühendab erinevaid võrke omavahel.

Router erineb switch'ist mitmel viisil. Esiteks, router kasutab IP aadresse, mitte MAC aadresse. Teiseks, routeril on iga liidese jaoks oma IP aadress - kui routeril on neli liidest, siis on neli erinevat IP aadressi. Kolmandaks, router blokeerib broadcast pakette, mis tähendab, et ühe võrgu broadcast ei lähe teise võrku.

### Router liidesed

Routeril ei ole lihtsalt "pordid" nagu switchil. Routeril on "liidesed" ehk interfaces. Iga liides on eraldi võrgus ja igal liidesel on oma IP aadress.

Näiteks router võib näha välja selline:

```
              ROUTER
    ┌─────────────────────────┐
    │ FastEthernet 0/0        │ → ühendatud võrku A
    │ FastEthernet 0/1        │ → ühendatud võrku B
    │ Serial 0/0/0            │ → ühendatud WAN-i
    └─────────────────────────┘
```

FastEthernet liidesed on Ethernet kaablite jaoks (nagu LAN). Serial liidesed on WAN ühenduste jaoks (näiteks ühendus internetti või teise linna). Igal liidesel võib olla erinev IP aadress ja nad võivad olla erinevates võrkudes.

![Ruuteri seadistamise sammud](/Labs/images/router_setup_steps.png)

### Router vs Switch

Vaatame, mis vahe on routeril ja switchil. Switch töötab Layer 2 tasemel ja kasutab MAC aadresse. Router töötab Layer 3 tasemel ja kasutab IP aadresse. Switch ühendab seadmeid samas võrgus. Router ühendab erinevaid võrke.

Switch hoiab MAC address table-t, kus on kirjas, millises pordis on milline MAC aadress. Router hoiab routing table-t, kus on kirjas, millised võrgud on olemas ja kuidas nendesse jõuda. Switch edastab broadcast pakette kõikidele portidele. Router blokeerib broadcast pakette ja ei lase neid teise võrku.

## 5. ROUTER KÄSUD

Nüüd vaatame, milliseid käske saab routeris kasutada. Need on käsud, mida sa täna serveris proovid.

### Privileged EXEC mode

Esimene asi, mida pead tegema, on minna privileged mode'i. See on nagu "administraatori" režiim, kus saad vaadata routeri konfiguratsiooni ja seadeid.

```
Router> enable
Router#
```

Pane tähele, et prompt muutub `>` märgilt `#` märgile. See näitab, et sa oled nüüd privileged mode'is.

### Vaata liideste staatust

Kõige olulisem käsk on `show ip interface brief`. See käsk näitab kõiki routeri liidesi ja nende staatust.

```
Router# show ip interface brief
```

Väljund näeb välja selline:

```
Interface         IP-Address      Status    Protocol
FastEthernet0/0   192.168.1.254   up        up
FastEthernet0/1   unassigned      down      down
Serial0/0/0       unassigned      admin down down
```

Vaatame, mida need veerud tähendavad. Interface veerg näitab liidese nime - näiteks FastEthernet0/0 või Serial0/0/0. IP-Address veerg näitab, mis IP aadress on sellel liidesel. Kui seal on kirjas "unassigned", siis IP-d pole veel antud.

Status veerg näitab füüsilist seisundit. Kui seal on "up", siis kaabel on ühendatud ja füüsiline kiht töötab. Kui seal on "down", siis kaablit pole ühendatud või on mõni teine füüsiline probleem. Kui seal on "administratively down", siis liides on välja lülitatud käsuga.

Protocol veerg näitab protokolli seisundit. Kui seal on "up", siis konfiguratsioon on õige ja liides töötab. Kui seal on "down", siis midagi on valesti konfigureeritud või füüsiline kiht ei tööta.

### Vaata routing table-t

Teine oluline käsk on `show ip route`. See näitab routing table-t ehk kõiki võrke, mida router teab.

```
Router# show ip route
```

Väljund võib näha välja selline:

```
C    192.168.1.0/24 is directly connected, FastEthernet0/0
C    192.168.2.0/24 is directly connected, FastEthernet0/1
```

Täht "C" tähendab "directly connected" ehk otse ühendatud. See tähendab, et need võrgud on ühendatud otse routeri liideste külge. Järgmine osa näitab võrgu aadressi (192.168.1.0/24). Viimane osa näitab, millisest liidest selle võrguni jõuab.

Praegu sa ei pea kõike täpselt mõistma. Oluline on teada, et routing table näitab, milliseid võrke router teab ja kuidas nendesse jõuda.

### Lülita liides sisse

Kui liides on "administratively down", siis see tähendab, et keegi on selle välja lülitanud. Sa saad selle sisse lülitada käsuga `no shutdown`.

```
Router# configure terminal
Router(config)# interface fastethernet 0/0
Router(config-if)# no shutdown
Router(config-if)# exit
Router(config)# exit
Router#
```

Esimene käsk viib sind configuration mode'i. Teine käsk vali, millist liidest tahad muuta. Kolmas käsk lülitab liidese sisse. Viimased kaks käsku väljuvad configuration mode'ist tagasi.

## 6. DEFAULT GATEWAY

### Mis on default gateway?

Default gateway on väga oluline mõiste. See on routeri IP aadress, kuhu arvuti saadab kõik paketid, mis ei ole samas võrgus.

Iga arvuti peab teadma, milline on tema default gateway. Muidu ei oska arvuti pakette teistesse võrkudesse saata. Kui default gateway-d ei ole, siis arvuti saab suhelda ainult teiste seadmetega samas võrgus.

![Vaikelüüsi määramine](/Labs/images/default_gateway_setup.png)

### Kuidas see töötab?

Mõtle nii: arvuti PC1 tahab saata paketti. Esimene küsimus on: kas destination IP on samas võrgus või mitte? Kui on samas võrgus, siis arvuti saadab paketi otse sinna (kasutades ARP-d, et leida MAC aadress).

Aga kui destination IP ei ole samas võrgus, siis arvuti saadab paketi default gateway-le. Default gateway on router, kes teab, kuidas teistesse võrkudesse jõuda. Router vaatab routing table-t ja saadab paketi edasi õiges suunas.

### Näide

Oletame, et PC1 konfiguratsioon on selline:

```
IP aadress:      192.168.1.10
Subnet mask:     255.255.255.0
Default gateway: 192.168.1.254
```

Kui PC1 pingib 192.168.1.50, siis ta arvutab: see IP on samas võrgus (192.168.1.0/24), seega saadan otse. Kui PC1 pingib 10.0.0.1, siis ta arvutab: see IP ei ole samas võrgus, seega saadan default gateway-le (192.168.1.254), mis on router.

![Ruuteri töö loogika](/Labs/images/router_logic.png)

---

## 7. KOKKUVÕTE

### Peamised punktid

Layer 3 ehk võrgukiht vastutab IP aadresside ja võrkude vahelise suhtluse eest. Ilma Layer 3-ta ei saaks erinevad võrgud omavahel suhelda ja internet ei töötaks.

Router on Layer 3 seade, mis ühendab erinevaid võrke. Igal routeri liidesel on oma IP aadress ja iga liides on erinevas võrgus. Router kasutab routing table-t, et otsustada, kuhu paketid saata.

MAC ja IP aadressid töötavad koos. MAC aadress on lokaalne ja muutub iga routeri juures. IP aadress on globaalne ja püsib sama kogu tee jooksul. MAC on nagu "praegune samm" ja IP on nagu "lõplik sihtpunkt".

Default gateway on routeri IP aadress, kuhu arvuti saadab paketid, kui need ei ole samas võrgus. Ilma default gateway-ta ei saa arvuti teistesse võrkudesse pakette saata.

### Mida õpime järgmiseks?

Järgmisel nädalal (4h sessioon) õpime IPv4 aadresse palju põhjalikumalt. Vaatame, kuidas IP aadressid on üles ehitatud, mis on subnet mask, kuidas IP aadresse planeerida ja mis on subnetting.

Täna oli üldine tutvustus. Järgmine kord läheb detailidesse.

### Kodune lugemine

Loe see materjal läbi veel kord pärast tundi. Proovi ise seletada, mis vahe on switchil ja routeril. Mõtle, miks on vaja kahte tüüpi aadresse (MAC ja IP).

Järgmisel korral tuleb palju matemaatikat (binary, subnet mask), seega on oluline, et Layer 3 üldine idee oleks selge.


**Soovitatavad videod:**
- [Routing Basics](https://www.youtube.com/watch?v=_P5Mm11_o7k&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=15)

**Järgmiseks nädalaks (IPv4):**
- [IPv4 Addressing](https://www.youtube.com/watch?v=f0iCqZpJZcA&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=10)
- [Subnetting](https://www.youtube.com/watch?v=zcOeSxvE3ME&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=11)
- [VLSM](https://www.youtube.com/watch?v=loWsRUDgW34&list=PLk4NQNr6-L8onI6MaPcfsRZJOvFO3S5D6&index=12)

---

**Edu serveris!** 🚀