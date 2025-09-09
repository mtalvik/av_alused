# Labor 2: Packet Tracer CLI

## Alustame lihtsalt - ehitame võrgu klikkides!

**Packet Tracer** on nagu LEGO võrkudele - saad ehitada ja testida ilma midagi katki tegemata.

## Labor 1.1: Esimene võrk - 2 arvutit

**SAMM 1: Lisa seadmed**

Ava Packet Tracer ja lisa 2 arvutit - kliki all vasakul "End Devices" (arvuti ikoon), vali "PC" ja kliki tööalal 2 korda erinevates kohtades. Siis lisa switch - kliki "Network Devices" (teine ikoon), vali "Switches" → "2960" ja kliki tööala keskele.

```
Tööala peaks nägema välja:
    [PC0]           [PC1]
         \         /
          [Switch0]
```

**SAMM 2: Ühenda juhtmetega**

Vali "Connections" (nooleke all vasakul), siis vali sirge must joon (Straight-Through Cable). Ühenda PC0 → FastEthernet0 switchiga FastEthernet0/1 ja PC1 → FastEthernet0 switchiga FastEthernet0/2. OOTA! Punased punktid muutuvad roheliseks (30 sek).

**SAMM 3: Anna IP aadressid**

PC0 seadistamine: kliki PC0 → Desktop → IP Configuration, sisesta IP Address `192.168.1.10` ja Subnet Mask `255.255.255.0`

PC1 seadistamine: kliki PC1 → Desktop → IP Configuration, sisesta IP Address `192.168.1.20` ja Subnet Mask `255.255.255.0`

**SAMM 4: Testi ühendust**

PC0 → Desktop → Command Prompt, kirjuta `ping 192.168.1.20`. Kui näed "Reply from..." - TÖÖTAB! 🎉

## Labor 1.2: Proovime CLI-d (ilma hirmuta!)

**Switchi nime muutmine GUI kaudu**

Kliki Switch0 peal, vali "Config" tab. Display Name: kirjuta "Minu-Switch" ja Hostname: kirjuta "SW1".

**Nüüd proovi CLI kaudu**

Kliki Switch0 → CLI tab, vajuta Enter ja kirjuta järgmised käsud:

```cisco
enable
configure terminal
hostname Klass-SW1
exit
```

Näed? Nimi muutus käsureal!

## Labor 1.3: Vaatame kuidas andmed liiguvad

Paremas alanurgas kliki "Simulation" (stopper), vali "Simple PDU" (kirja ikoon), kliki PC0, siis PC1 ja vajuta "Play" ▶️

Näed animatsiooni: pakett läheb PC0 → Switch, switch mõtleb → saadab PC1-le, PC1 saadab vastuse tagasi.

## OSA 2: PÄRIS SEADMED SERVERIRUUMIS

Nüüd lähme päris Cisco seadme juurde!

**Õpi pähe need 5 käsku:**
- `enable` - saan adminiks
- `configure terminal` - tahan muuta
- `hostname NIMI` - annan nime
- `exit` - lähen tagasi
- `copy run start` - SALVESTAN!

**Küsimused mõtlemiseks:**

Miks on vaja kolme erinevat režiimi? Millal kasutada PT-d ja millal päris seadet? Miks on salvestamine nii oluline?