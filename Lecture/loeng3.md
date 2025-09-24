# Loeng 3: Juhtmevabad ja mobiilsed võrgud

*Tere hommikust! Täna sukeldume juhtmevabade võrkude maailma - tehnoloogiasse, mida kasutate iga päev ilma sellele mõtlemata.*

## Mida me täna õpime

Mõelge sellele - ärgates kontrollisite telefoni (mobiilsidevõrk), ühendusite kodus WiFi-ga, võib-olla sünkroonisid nutikella (Bluetooth) ja makssite kohvi eest telefoniga (NFC). Neli erinevat juhtmevaba tehnoloogiat enne kella 9 hommikul! Mõistame, kuidas need tegelikult töötavad.

## Osa 1: WiFi - Internet ilma juhtmeteta

### Mis WiFi tegelikult on?

WiFi on lihtsalt raadiolained, mis kannavad internetiandmeid. Kujutage ette karjumist üle toa versus megafoni kasutamist - see on põhimõtteliselt vahe teie sülearvuti WiFi ja ruuteri antenni vahel.

**Nime lugu:**
- WiFi ei tähenda tegelikult "Wireless Fidelity"
- Ametlik nimi on IEEE 802.11 (aga keegi ei ütle seda peol)
- WiFi Alliance andis meile sõbralikud nimed: WiFi 4, 5, 6 ja nüüd 7

### WiFi põlvkonnad - nagu telefoni uuendused

| Põlvkond | Millal | Reaalne kiirus | Mida see teile tähendas |
|----------|--------|----------------|-------------------------|
| **WiFi 4** | 2009 | ~50 Mbps | Netflix muutus võimalikuks |
| **WiFi 5** | 2014 | ~200 Mbps | Mitu seadet, pole probleemi |
| **WiFi 6** | 2019 | ~600 Mbps | Kogu pere striimib |
| **WiFi 7** | 2024 | ~2000 Mbps | VR, 8K video, kõik kohe |

*Mõelge sellest kui teedest: WiFi 4 oli külatee, WiFi 6 on kiirtee, WiFi 7 on Autobahn.*

### Kaks raadio "kiirteed", mida teie ruuter kasutab

**2.4 GHz sagedusala:**
- Läbib seinad paremini
- Aeglasemad kiirused
- Rohkem rahvast (kõik kasutavad)
- Nagu AM-raadio - levib kaugele, aga kvaliteet pole imeline

**5 GHz sagedusala:**
- Kiiremad kiirused
- Ei meeldi seinad
- Vähem rahvast
- Nagu FM-raadio - parem kvaliteet, aga lühem ulatus

**Uus tulija: 6 GHz sagedusala (WiFi 6E/7)**
- Superkiire
- Väga lühike ulatus
- Peaaegu tühi (praegu)
- Nagu oma privaatne kiirtee

### Miks kaob WiFi vannitoas?

Raadiolained vihkavad vett ja metalli!
- Betooniseinad = väike probleem
- Vannitoaplaadid ja torud = suur probleem
- Töötav mikrolaineahi = tohutu probleem (kasutab sama 2.4 GHz)

## Osa 2: WiFi turvalisus - lukustage oma juhtmevaba uks

### WiFi paroolide evolutsioon

Mõelge WiFi turvalisusest kui ustele pandud lukkudest:

1. **Turvalisus puudub** = Lukk puudub (igaüks astub sisse)
2. **WEP (1999)** = Odav tabalukk (murtav 2 minutiga)
3. **WPA (2003)** = Tavaline lukk (murdmiseks kulub vaeva)
4. **WPA2 (2004)** = Hea turvariiv (praegune miinimum)
5. **WPA3 (2018)** = Tark lukk alarmiga (parim valik)

### Mis juhtub tegelikult, kui sisestate WiFi parooli?

```
Teie: "Siin on mu parool"
Ruuter: "Laske mul luua teile unikaalne krüpteerimisvõti"
Teie: "Suurepärane, nüüd on kõik meie jutud segamini aetud"
Naaber: "??#$@%^&*" (ei saa millestki aru)
```

**WPA3 lahe trikk:** Isegi kui keegi homme teie parooli ära arvab, ei saa ta dekrüpteerida, mida täna tegite. See on nagu lukkude vahetamine pärast igat kasutust!

## Osa 3: Mobiilsidevõrgud - kõnedest kassivideoteni

### Põlvkondade mäng

Mobiilvõrgud uuenevad umbes iga 10 aasta tagant, nagu konsoolide põlvkonnad:

**1G (1980ndad):** Ainult hääl - nagu juhtmeta telefon tohutu ulatusega
**2G (1990ndad):** Digitaalne hääl + SMS - "Ussimäng" Nokial!
**3G (2000ndad):** Mobiilne internet saabub - Facebook klapptelefonil
**4G (2010ndad):** Kiire internet - Instagram, Uber, kõik
**5G (2020ndad):** Ülikiire, madal viivitus - isejuhtivad autod, kaugkirurgia

### Miks 5G vajab nii palju maste?

**Füüsika reegel: Kõrgem sagedus = lühem vahemaa**

Mõelge sellest nii:
- 4G on nagu taskulamp - valgustab kaugele
- 5G on nagu laserpointer - väga täpne, aga lühem ulatus
- 5G mmWave on nagu laualamp - superere, aga valgustab ainult teie lauda

```
2G mast: ████████████████ Katab terve linna
4G mast: ████████ Katab linnaosa
5G mast: ████ Katab mõned kvartalid
5G mmWave: █ Katab ühe tänava
```

### Mis teeb 5G eriliseks?

Kolm supervõimet:
1. **Kiirus:** Laadige film alla sekunditega
2. **Madal latentsus:** Viivitus puudub (oluline mängimiseks/VR-ile)
3. **Mahutavus:** Tuhandeid seadmeid masti kohta

*Päris näide: Kontserdil 4G ebaõnnestub, sest 10 000 telefoni koormavad selle üle. 5G saab kõigiga hakkama.*

## Osa 4: Bluetooth - sõbralik naabri protokoll

### Milleks Bluetooth on?

WiFi ühendab teid internetiga. Bluetooth ühendab teie seadmed omavahel.
- Kõrvaklapid telefoniga
- Telefon autoga
- Aktiivsusmonitor telefoniga
- Hiir sülearvutiga

### Bluetoothi versioonid - vaikselt paremaks muutumine

Erinevalt WiFi suurtest teadaannetest paraneb Bluetooth vaikselt:
- **Bluetooth 4 (2010):** Low Energy saabub - aktiivsusmonitorid kestavad nädalaid
- **Bluetooth 5 (2016):** 4x ulatus - töötab üle maja
- **Uusim:** Audio jagamine - üks telefon, mitu kõrvaklappi

### Classic vs Low Energy

**Bluetooth Classic:** Teie kõrvaklapid striimivad muusikat (energianäljane)
**Bluetooth Low Energy (LE):** Teie aktiivsusmonitor (töötab nädalaid)

See on nagu vahe auto ja jalgratta vahel - mõlemad transpordivad, aga üks kulutab palju vähem energiat.

## Osa 5: NFC - maagiline puudutus

### Mis on NFC?

Near Field Communication - töötab ainult puudutades või peaaegu puudutades (4cm).
See on meelega lühikese ulatusega turvalisuse huvides!

### Kus te seda iga päev kasutate

- Puudutades maksta (telefon/kaart)
- Hotellitoakaardid
- Bussi-/metrookaardid
- Puuduta WiFi ühendamiseks
- Digitaalsed visiitkaardid

### Miks nii lähedal?

**Turvalisus läheduse kaudu!**
Keegi ei saa varastada teie makseinfot üle toa - nad peaksid teid niikuinii taskust varastama.

## Osa 6: Eriotstarbelised võrgud - LoRaWAN

### Mis on LoRaWAN?

Kujutage ette, et peate kontrollima veemõõdikut metsas või jälgima lehma farmis. Teil on vaja:
- Superpikamaalist (15+ km)
- Aastaid aku kestvust
- Kiirust pole vaja (ainult pisikesed andmepaketid)

LoRaWAN on selleks ideaalne - see on juhtmevabade võrkude "maratonijooksja":
- Aeglane, aga jõuab igavesti
- Üks lüüs katab terve linna
- Aku kestab 10+ aastat

### Päris näited Eestis

- Parkimisandurid teatavad äpile, kas koht on vaba
- Prügikastid annavad teada, kui täis
- Farmi andurid jälgivad mulda
- Veelekete tuvastamine torudes

## Osa 7: Satelliitinternet - Starlink ja teised

### Elon Muski Starlink - internet kosmosest

**Probleem:** 40% maailmast pole internetti. Miks? Kaableid on liiga kallis vedada mägedesse, saaretele, kõrbetesse.

**Lahendus:** 6000+ satelliiti madalal orbiidil (550km kõrgusel)
- Traditsiooniline satelliit: 35,786km kõrgusel = 600ms viivitus = mängimiseks kõlbmatu
- Starlink: 550km = 20-40ms viivitus = peaaegu sama hea kui kaabel

### Kuidas Starlink töötab?

```
Teie maja → Satelliitantenn (pizza karbi suurune)
    ↑↓ (Ku/Ka sagedus: 12-18/26-40 GHz)
Satelliit 1 → Satelliit 2 → Satelliit 3 (laserühendused)
    ↑↓
Maajaama → Internet
```

**Tehnilised faktid:**
- Kiirus: 50-500 Mbps (sõltub kasutajatest piirkonnas)
- Latentsus: 20-40ms (hea mängimiseks!)
- Hind Eestis: ~€65/kuu + €450 seadmed
- Antenni võimsus: 100W (soojendab end talvel lume sulatamiseks)

### Starlinki konkurendid

| Teenus | Satelliite | Kõrgus | Kiirus | Viivitus | Staatus |
|--------|------------|---------|---------|----------|---------|
| **Starlink** | 6000+ | 550km | 500 Mbps | 20ms | Töötab |
| **OneWeb** | 600+ | 1200km | 200 Mbps | 70ms | Töötab |
| **Amazon Kuiper** | 0/3236 | 590-630km | 400 Mbps | 30ms | 2025 |
| **Hiina Guowang** | 0/13000 | 500-1145km | ? | ? | 2030 |

### Millal Starlink on mõttekas?

**SOBIB:**
- Maakoht ilma fiiberühenduseta
- Merel või ekspeditsioonil
- Varuühendus kriitilisele äri
- Digitaalsed nomadid
- Ukraine sõjavägi (tasuta SpaceX-ilt)

**EI SOBI:**
- Linnas kus on fiiberkaabel (kallim ja aeglasem)
- Tihedalt asustatud kortermajas (jagatud ribalaiust)
- Pilvedega kaetud piirkonnad (vihm/lumi häirib signaali)

## Osa 8: Juhtmevaba internet kodus - WISP ja FWA

### Fixed Wireless Access (FWA) - 5G kui koduinternet

Mõelge: Miks vedada kaablit kui 5G on niigi kiire?

**Kuidas töötab:**
1. Operaator paigaldab 5G ruuteri teie koju
2. See ühendub 5G mastiga (mitte WiFi!)
3. Ruuter jagab internetti WiFi kaudu

**Eestis saadaval:**
- Telia 5G koduinternet: kuni 1 Gbps
- Elisa 5G FWA: kuni 500 Mbps
- Tele2: tulekul

**Plussid:**
- Paigaldus 1 päevaga (vs fiiberkaabel nädalad)
- Saate kaasa võtta kui kolite
- Pole vaja auke puurida

**Miinused:**
- Sõltub mastist (takistused = aeglasem)
- Jagatud ribalaius (õhtul aeglasem)
- Ilm mõjutab (26 GHz 5G ei tööta tugeva vihmaga)

### Wireless ISP (WISP) - Küla internet

Eestis on 50+ väikest WISP operaatorit kes teenindavad maapiirkondi:

**Tehnoloogia:**
- Punkt-punkt ühendused: 1-50km
- Ubiquiti airFiber: kuni 10 Gbps
- Mikrotik wireless: kuni 1 Gbps
- Sagedused: 5 GHz (vaba) või 60 GHz (vaba) või litsentsitud

```
Operaatori mast
    ↓ (5/60 GHz raadiolink)
Küla mast
    ↓↓↓ (WiFi või LTE)
Majapidamised
```

## Osa 9: WiFi 7 ja tuleviku tehnoloogiad

### WiFi 7 - mitte lihtsalt kiirem

**Multi-Link Operation (MLO)** - revolutsioon!
- Telefon ühendub korraga 2.4 + 5 + 6 GHz
- Kui üks kanal on hõivatud, kasutab teist
- Latentsus: 5ms → 1ms (VR jaoks kriitiline!)

**320 MHz kanalid:**
```
WiFi 6:  ████████ (160 MHz)
WiFi 7:  ████████████████ (320 MHz)
Teooria: 46 Gbps (praktikas ~5 Gbps)
```

### Li-Fi - internet läbi valguse

**Kuidas töötab:** LED lamp vilgub miljon korda sekundis, silm ei näe, aga sensor loeb.

**Plussid:**
- Kiirus: 224 Gbps laboritingimustes
- Turvalisus: valgus ei läbi seinu
- Tervislik: pole raadiolaineid

**Miinused:**
- Vajab otsenähtavust
- Päikesevalgus segab
- Ei tööta taskus

**Kus kasutatakse:** Haiglad (ei sega aparatuuri), lennukid, salajased ruumid

### 6G (2030) - mitte ainult kiirus

**Tehnilised eesmärgid:**
- 1 Tbps kiirus (100x 5G)
- 0.1ms latentsus (10x 5G)
- 10 miljonit seadet/km²
- Terahertz sagedused (0.1-10 THz)

**Uued võimalused:**
- **Digital twin:** Terve linn digitaalses koopias reaalajas
- **Holograafilised kõned:** 3D inimene teie toas
- **Brain-computer interface:** Mõtted otse võrku (Neuralink)
- **Energiaharvesting:** Võrk laeb seadmeid õhu kaudu

### Mesh võrgud - tuleviku WiFi

**Traditsiooniline:** Üks ruuter → nõrk signaal kaugel
**Mesh:** Palju väikeseid ruutereid → tugev signaal kõikjal

```
Traditsiooniline:
    [Ruuter] ----nõrk----> [Teie]

Mesh:
    [Ruuter1] ←→ [Ruuter2] ←→ [Ruuter3]
         ↓            ↓            ↓
      [Tugev]     [Tugev]     [Tugev]
```

**Eestis populaarsed:**
- Asus ZenWiFi (WiFi 6E)
- Google Nest WiFi
- Eero Pro 6E

### Quantum Internet - kaugem tulevik

**Mis see on:** Kvantseisundite edastamine, mitte bitid

**Miks oluline:**
- Murdmatu krüpteerimine (füüsika seadused kaitsevad)
- Kvantarvutite ühendamine
- Teleportatsioon (info, mitte inimesed... veel)

**Praegune seis:**
- Hiina: 2000km kvant-satelliitside
- Holland: 3 linna kvant-testimisvõrk
- Eesti: TalTech teeb kvant-krüpto uuringuid

## Osa 10: Küberoht juhtmevabastes võrkudes

### WiFi ründed mida IT tudeng peab teadma

**Evil Twin** - vale WiFi punkt
```
Õige: "Kohvik_WiFi"
Vale: "Kohvik_WiFi" (häkker)
Teie telefon: ühendub automaatselt valega!
```
Kaitse: Kasuta VPN-i avalikes kohtades

**Deauth Attack** - viskab teid WiFi-st välja
- Häkker saadab "disconnect" käsu teie nimel
- Kaitse: WPA3 (Protected Management Frames)

**KRACK** (2017) - WPA2 nõrkus
- Murdis WPA2 krüpteeringu
- Paigatud, aga vanad seadmed ohus
- Õppetund: Uuenda alati firmware!

**Wardriving** - WiFi kaardistamine
- Sõitmine ringi + WiFi skanner + GPS
- Wigle.net: 1.1 miljardit WiFi võrku kaardistatud
- Teie WiFi on seal tõenäoliselt ka!

### Bluetooth ohud

**BlueBorne** (2017) - kontrollis seadmeid üle
**Bluesnarfing** - varastab andmeid
**AirTag stalking** - jälgimine Apple tracker'itega

### 5G turvamured

**IMSI Catcher** (StingRay)
- Teeskleb olevat mobiilimast
- Püüab kõnede metaandmeid
- Kaitse: 5G SA (Standalone) võrgud

**Hiina seadmed:**
- Huawei/ZTE keelatud USA/UK 5G võrkudes
- Kartus: tagauksed Hiina valitsusele
- Eesti: Ei kasuta 5G tuumvõrkudes

## Praktilised nõuanded tudengitele

### Hea WiFi seadistamine kodus

1. **Ruuteri paigutus:** Kõrgel ja keskel (mitte kapis!)
2. **Kanali valik:** Kasutage WiFi analüsaatori äppi, valige tühi kanal
3. **Turvalisus:** WPA3 võimalusel, WPA2 miinimum
4. **Parool:** Pikk fraas parem kui keeruline: "MuKoerArmastab2SüüaPitsat!" 
5. **Sageduse valik:** 5GHz striimimiseks, 2.4GHz IoT seadmetele

### Tõrkeotsingu alused

**Aeglane WiFi?**
- Kontrollige, mitu seadet on ühendatud
- Testige ruuteri lähedal vs kaugel
- Taaskäivitage ruuter (jah, tõesti)
- Kontrollige, kas naabrid on samal kanalil

**Ei saa ühenduda?**
- Unustage võrk ja ühenduge uuesti
- Kontrollige, kas MAC-filtreerimine on sees
- Kinnitage WPA2/WPA3 ühilduvus

## Testime teie arusaamist

### Kiirküsimused

1. **Miks töötab teie WiFi aias, aga mitte ülakorrusel?**
   (Vihje: Kuidas raadiolained liiguvad?)

2. **Olete kohvikus. Kas peaksite kasutama nende avatud WiFi-t panganduseks?**
   (Mõelge turvaprotokollide peale)

3. **Teie Bluetooth kõrvaklapid katkevad, kui panete telefoni tagataskusse. Miks?**
   (Mõelge, mis on telefoni ja kõrvaklappide vahel)

4. **Miks tühjendab 5G telefoniaku kiiremini kui 4G?**
   (Mõelge nende mitme antenni ja sageduse peale)

[![2.4 GHz vs 5 GHz WiFi: What is the difference?](https://img.youtube.com/vi/J_bf_KE5llQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=J_bf_KE5llQ&t=139s)

### Praktiline harjutus

**Kujundage WiFi võrk 3-korruselisele majale:**
- Kuhu paigutaksite ruuterid?
- Milliseid sagedusi millisele toale?
- Kuidas lahendaksite nutika teleri, mängukonsooli ja 15 nutika pirni?

## Peamised järeldused

1. **WiFi** = Teie peamine internetiühendus, muutub iga põlvkonnaga kiiremaks
2. **Mobiilvõrgud** = Kõikjal katvus, 5G on kiire, aga vajab rohkem maste
3. **Bluetooth** = Seadmest seadmesse, ideaalne tarvikutele
4. **NFC** = Superlühike ulatus turvalisuse huvides
5. **LoRaWAN** = Pikamaaline anduritele, mis vajavad aastaid akutööd

## Järgmiseks nädalaks

Proovige kodus:
1. Kasutage WiFi analüsaatori äppi oma telefonis
2. Loendage, mitme juhtmevaba tehnoloogiaga päeva jooksul kokku puutute
3. Kontrollige, millist WiFi turvalisust teie koduruuter kasutab

Pidage meeles: Need tehnoloogiad ei konkureeri - nad täiendavad üksteist. Teie tulevased IoT projektid kasutavad tõenäoliselt mitut neist koos!

---

**Küsimused?** Ärge kõhelge küsimast - kui teie olete segaduses, on tõenäoliselt pool klassist ka!
