# Lab Ethernet Kaabli Valmistamine

## Vajalikud Materjalid

![Vajalikud tööriistad ja materjalid](/Labs/images/kit.png)

- Cat5e (1m kaabli kohta)
- RJ-45 otsikud (läbipaistvad on paremad algajatele)
- Kaablitangid (crimping tool)
- Kaablitester
- Kaablikoorijad või nuga

---

## Samm-Sammult Juhend

### 1. Kaabli Ettevalmistamine ja Koorimine

![Kaabli koorimine](/Labs/images/korimine.png)
![Kaabli korimise tangid](/Labs/images/kor_j.jpg)

**Koorimine:**
- Võta kaabel ja mõõda otsast ~1.5 cm
- Kasuta kaablikoorijat
- Lõika ringjooneliselt ainult väliskile
- Ära kahjusta sisemisi värvilisi kiude
- Tõmba väliskest ära ja lõika maha

![Õige vs vale koorimine](/Labs/images/correct-vs-incorrect-stripping.png)
![vale koorimine](/Labs/images/not_correct.png)

**Miks see oluline on:**
- Liiga sügav lõige kahjustab kiude → kaabel ei tööta
- Liiga vähe kooritud → kiud ei jõua otsikusse
- Õige pikkus = ~3-4 cm lahti harutatud kiude

---

### 2. Keerdpaaride Lahtiharutamine

![Keerdpaaride lahtiharutamine](/Labs/images/untwist-wire-pairs.png)

**Kuidas teha:**
- Näed 4 keerdpaari (8 kiudu kokku)
- Haruata igaüks ettevaatlikult lahti
- Siruta kiud täiesti sirgeks (VÄGA oluline!)
- Kasuta sõrmi või lauaserva kiudude sirgendamiseks
- Kõik kiud peavad olema ühel tasapinnal

![Kiudude sirgendamine](/Labs/images/straighten-wires.png)

**Näpunäited:**
- Ära haruuta kiude liiga kaugele tagasi kaablisse
- Hoia kiud paralleelselt
- Kui kiud on käharad, pole võimalik otsikusse sisestada

---

### 3. Kiudude Järjendamine Standardi Järgi

![T-568B värvikood](/Labs/images/t568b-wiring-standard.png)

**T-568B Standard (kõige levinum):**

```
PIN 1: Valge/Oranž (valgete triipudega oranž)
PIN 2: Oranž (täisoranž)
PIN 3: Valge/Roheline (valgete triipudega roheline)
PIN 4: Sinine (täissinine)
PIN 5: Valge/Sinine (valgete triipudega sinine)
PIN 6: Roheline (täisroheline)
PIN 7: Valge/Pruun (valgete triipudega pruun)
PIN 8: Pruun (täispruun)
```

![T-568A/B värvikood](/Labs/images/wiring-standard.png)

**Meeldetrikk T-568B jaoks:**
- Oranž paar alguses (1-2)
- Siis roheline kiud (3)
- Sinine paar keskel (4-5)
- Siis roheline paar valmis (6)
- Pruun paar lõpus (7-8)

**TÄHTIS:** Mõlemad otsad peavad kasutama SAMA standardit!

---

### 4. Kiudude Lõikamine Õigeks Pikkuseks

![Kiudude lõikamine](/Labs/images/trim-wires-length.png)

**Kuidas teha:**
- Hoia kõik 8 kiudu õiges järjekorras, tihedalt koos
- Kiud peavad olema täiesti sirged ja paralleelsed
- Lõika kaablitangide lõiketeraga või teravate kääridega
- Lõika täpselt risti nurga all
- Jäta pikkuseks 12-13mm kooritud otsast

**Kontroll:**
- Vaata otsast: kõik 8 kiudu ühel joonel?
- Kui ei, siis lõika uuesti
- Liiga pikad kiud = ei krimpi korralikult
- Liiga lühikesed = ei jõua otsiku lõppu

---

### 5. RJ-45 Otsiku Paigaldamine

![RJ-45 otsiku orientatsioon](/Labs/images/rj45-connector-orientation.png)

**Otsiku orientatsioon:**
- Hoia otsikut nii, et metallkontaktid on ÜLEVAL
- Plastist lukk (clip) on ALLPOOL
- Avatud ots on sulle POOLE

![Kiudude sisestamine otsikusse](/Labs/images/insert-wires-connector.png)

**Sisestamine:**
- Hoia kiude täpselt õiges järjekorras
- Kiud peavad olema tihedalt koos
- Lükka otsik otse kiudude peale
- Lükka kindlalt ja sirgjooneliselt
- Suruma kuni kiud jõuavad otsiku lõpuni (näed läbipaistva otsiku kaudu)
- Väliskest peab jõudma ~5mm otsikusse

![Kontroll läbi läbipaistva otsiku](/Labs/images/check-through-clear-connector.png)

**Kontrolli enne krimpimist:**
1. Kõik 8 kiudu on näha otsiku esiosas?
2. Õige värvijärjekord?
3. Väliskest on otsiku sees?
4. Kiud on täiesti otsiku lõpus?

---

### 6. Krimpimine

![Kaablitangide kasutamine](/Labs/images/crimping-tool-usage.png)

**Kaablitangide kasutamine:**
- Aseta otsikuga kaabel tangide krimpimispessa
- Otsik peab olema täielikult pesas (lukk allapoole)
- Pigista TUGEVALT mõlema käega
- Kuuled/tunned "KLIKK" või vastupanu kaob
- Äralaskmine tähendab krimpimine valmis

**Mis juhtub krimpimise ajal:**
- Metallkontaktid läbivad plastikust ja puudutavad vasest kiude
- Tagumise osa klamber surutakse väliskile vastu
- Ühendus on nüüd mehhaaniliselt ja elektriliselt tugevdatud

![Õigesti krimpitud otsik](/Labs/images/properly-crimped-connector.png)

---

### 7. Teine Ots

- Korda samme 1-6 teise otsa jaoks
- Kasuta SAMA standardit (T-568B või T-568A)
- Ole sama täpne kui esimese otsaga

---

### 8. Kaabli Testimine

![Kaablitester](/Labs/images/cable-tester-device.png)
**Kaablitesteri kasutamine:**
1. Ühenda üks kaabliots **TX** (Master/Transmit) pessa
2. Ühenda teine ots **RX** (Remote/Receive) pessa
3. Lülita tester sisse
4. Vaata LED-tulesid

![Testi tulemused](/Labs/images/results.png)

**Õige tulemus:**
- Kõik 8 LED-tuld süttivad JÄRJESTIKKU: 1→2→3→4→5→6→7→8
- Süttivad rohelist värvi
- Ilmuvad õiges järjekorras mõlemal pool

**Vead ja nende tähendused:**

| LED käitumine | Probleem | Lahendus |
|---------------|----------|-----------|
| LED ei sütti üldse | Üldine rike | Kontrolli kõik ühendused |
| LED 1 ei sütti | Valge/Oranž kiud ei kontakti | Tee uuesti |
| LED-id vales järjekorras | Kiud vales järjekorras | Kontrolli värvijärjekorda |
| Mõned LEDid puuduvad | Kiud ei ole täielikult otsikus | Krimpi uuesti |
| Vahelduvad LEDid | Katkine kiud | Uus kaabel |

---

### Visuaalne Kontroll

**Kontrolli iga otsik:**
- [ ] Kõik 8 kiudu nähtavad läbi läbipaistva plastiku?
- [ ] Õige värvijärjekord?
- [ ] Väliskest sisestatud otsikusse?
- [ ] Ei ole paljastatud vasejuhet enne otsikut?
- [ ] Otsik on korralikult krimpitud (ei tule maha)?

### Elektriline Kontroll
- [ ] Kaablitester näitab kõik 8 LED-tuld?
- [ ] Õiges järjekorras?
- [ ] Kaabel töötab seadmete vahel?

---

## Tavalised Vead ja Lahendused

**Kiire Diagnostika:**
- ❌ LED-id 1-8 ei sütti → üldine rike, alusta otsast
- ❌ LED-id vales järjekorras → värvid vales kohas
- ❌ Mõned LED-id puuduvad → need kiud ei kontakti
- ❌ Töötab 100Mbps, mitte 1Gbps → PIN 4,5,7,8 ei tööta

---

## Näpunäited Kvaliteedi Parandamiseks

**3 Kuldreeglit:**
1. ✅ **Sirged kiud** = edukas sisestamine
2. ✅ **Kontrolli 2x** = krimpi 1x
3. ✅ **Testi alati** = usalda, aga kontrolli
