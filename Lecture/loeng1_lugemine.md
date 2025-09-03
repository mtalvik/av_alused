# Loeng 1: Arvutivõrkude alused

**Lugemismaterjal esimeseks tunniks**  
**Lugemisaeg:** umbes 20 minutit

---

Arvutivõrk on kahe või enama seadme ühendus, mis võimaldab neil omavahel andmeid jagada ja ressursse kasutada. Tänapäeval on võrgud kõikjal meie ümber ning moodustavad kaasaegse digitaalse maailma selgroo.

## Võrkude roll igapäevaelus

Internet on muutnud viisi, kuidas me suhtleme, õpime, töötame ja meelelahutust tarbime. Võrgud võimaldavad meil saata sõnumeid üle maailma sekundite jooksul, vaadata videoid reaalajas ja mängida online-mänge sõpradega teistest riikidest.

Kodus kasutate tõenäoliselt WiFi-võrku, mis ühendab teie nutitelefoni, arvuti, nutitelevisiooni ja võib-olla isegi külmikut või valgustussüsteemi. Need kõik seadmed moodustavad teie koduse võrgu, mis omakorda ühendub internetiga läbi teenusepakkuja.

Koolis või töökohas toimib sarnane võrk, kuid tavaliselt keerukama struktuuriga. Seal võivad olla eraldi võrgud erinevate osakondade jaoks, külaliste ligipääsu jaoks ja administraatorite haldussüsteemide jaoks.

## Võrgu komponendid

Iga võrk koosneb kolmest põhilisest elemendist: lõppseadmetest, vahepealstest seadmetest ja võrgumeediumist.

Lõppseadmed on need, mida inimesed otse kasutavad. Teie nutitelefon, sülearvuti, tahvelarvuti, mängukonsooli ja printer on kõik lõppseadmed. Ka serverid, kus asuvad veebisaidid, e-kirjad ja pilve-teenused, on lõppseadmed, kuigi need asuvad tavaliselt kaugel andmekeskustes.

Vahepealsed seadmed tagavad andmete liikumise võrgus. WiFi-ruuter teie kodus on vahepealne seade, mis ühendab kõik teie kodused seadmed omavahel ja internetiga. Suurtes võrkudes on palju erinevaid vahepealseid seadmeid: lüliteid, ruutereid, tulemüüre ja teisi spetsiaalseid seadmeid.

Võrgumeediumi all mõistetakse füüsilist teed, mida mööda andmed liiguvad. See võib olla vaskkaabel, valguskiudkaabel või raadiolained WiFi ja mobiilside puhul. Iga meediumi tüübil on oma eelised ja puudused kiiruse, kauguse ja maksumuse osas.

## Võrgu tüübid

Võrke klassifitseeritakse tavaliselt nende suuruse ja ulatuse järgi.

Isiklik ala võrk ehk PAN (Personal Area Network) katab väga väikest ala, tavaliselt ühte inimest ümbritsevat ruumi. Bluetoothi kaudu ühendatud kõrvaklapid, käekell või hiir moodustavad PAN-i.

Kohalik ala võrk ehk LAN (Local Area Network) hõlmab ühte hoonet või väikest piirkonda. Teie kodune WiFi-võrk või kooli arvutiklass on LAN-i näited. LAN-i eripäraks on see, et tavaliselt kuulub see ühele organisatsioonile ja kasutab ühtset tehnoloogiat.

Linnaala võrk ehk MAN (Metropolitan Area Network) katab suurema piirkonna, näiteks kogu linna või linnaosa. Paljud linnad pakuvad avalikku WiFi-teenust, mis on MAN-i näide.

Laiala võrk ehk WAN (Wide Area Network) ühendab kaugel asuvaid võrke. Internet on maailma suurim WAN, mis ühendab miljoneid väiksemaid võrke üle kogu maailma.

## Internet kui globaalne võrk

Internet ei ole üks suur võrk, vaid võrkude võrk. See koosneb tuhandetest erinevatest võrkudest, mis kuuluvad erinevatele organisatsioonidele: internetiteenuse pakkujatele, ülikoolidele, valitsusasutustele ja ettevõtetele. Need kõik võrgud on omavahel ühendatud ja kasutavad ühiseid protokolle, et tagada andmete vaba liikumine.

Internetiteenuse pakkujad (ISP-d) moodustavad interneti selgroo. Suurimad ISP-d opereerivad globaalseid võrke, mis ühendavad mandrid. Väiksemad ISP-d ostavad ühenduse suurematelt ja pakuvad teenust lõppkasutajatele.

## Võrguarhitektuuri põhimõtted

Hästi disainitud võrk peab vastama mitmele olulisele nõudele.

Vigade taluvus tähendab, et võrk jätkab tööd isegi siis, kui mõni selle osa ebaõnnestub. Seda saavutatakse dubleerimise abil - olulised ühendused ja seadmed on kahekordistatud, et ühe rikke korral oleks olemas varuvariant.

Skaleeritavus tähendab võrgu võimet kasvada. Hästi disainitud võrk suudab käsitleda rohkem kasutajaid, seadmeid ja liiklust ilma märkimisväärse jõudluse languseta.

Teenuse kvaliteet ehk QoS (Quality of Service) tagab, et erinevat tüüpi liiklus saab sobiva prioriteedi. Videokõnedel on vaja madalat viivitust, samas kui failide allalaadimine talub suuremat viivitust, kuid vajab stabiilset ribalaius.

Turvalisus on võrgu kõige kriitilisem aspekt. Võrk peab kaitsma nii andmeid kui ka ressursse volitamata juurdepääsu eest. See hõlmab autentimist (kasutaja tuvastamist), autorisatsiooni (õiguste määramist) ja krüpteerimist (andmete kodeerimist).

## Võrguturvalisuse alused

Võrguturvalisus algab füüsilisest turvalisusest. Serverid ja võrguseadmed peavad asuma lukustatud ruumides, kuhu on juurdepääs ainult volitatud isikutel. Ka lõppkasutaja seadmed vajavad kaitset - sülearvutid tuleks lukustada, kui neid jäetakse järelevalveta.

Tarkvaraline turvalisus hõlmab operatsioonisüsteemide ja rakenduste regulaarset uuendamist. Vananenud tarkvara on kerge märklaud ründajatele. Tulemüürid ja viirusetõrje tarkvara pakuvad lisasamakat kaitset.

Võrgu tasandil kasutatakse krüpteerimist, et kaitsta andmeid nende liikumise ajal. HTTPS veebilehtede puhul ja VPN-ühendused kaugtöö jaoks on krüpteerimise näited, mida te iga päev kasutate.

## Tehnoloogiate areng

Võrgutehnoloogiad arenevad pidevalt. 5G mobiilside lubab kiiruseid, mis võivad konkureerida fiiberoptilistega. Internet of Things ehk asjade internet ühendab järjest rohkem igapäevaseid objekte võrku - alates nutikellast kuni isesõitvate autodeni.

Pilve-teenused on muutnud viisi, kuidas me andmeid salvestame ja rakendusi kasutame. Selle asemel, et hoida kõike oma seadmes, kasutame järjest rohkem teenuseid, mis töötavad kaugetes andmekeskustes.

Tehisintellekt ja masinõpe muudavad võrkude haldamist. Võrgud muutuvad järjest targemaks, suutes automaatselt tuvastada probleeme, optimeerida jõudlust ja isegi ennustada tulevasi vajadusi.

## IT-karjäär võrgunduses

Võrgunduse valdkond pakub mitmeid huvitavaid karjäärivõimalusi. Võrguadministraatorid haldavad igapäevaselt organisatsioonide võrke, lahendavad probleeme ja aitavad kasutajatel. Võrguinsenerid disainevad ja ehitavad uusi võrke, rakendades uusimaid tehnoloogiaid.

Võrguturvalisuse spetsialistid keskenduvad võrkude kaitsele küberründete eest. See on eriti kiiresti kasvav valdkond, kuna küberohud muutuvad järjest keerukamaks.

Professionaalsed sertifikaadid nagu Cisco CCNA aitavad tõestada teie oskusi tööandjatele. Need sertifikaadid on tunnustatud kogu maailmas ja avavad uksed hästi tasustatud töökohtadele.

## Kokkuvõte

Arvutivõrgud on kaasaegse elu lahutamatu osa. Need võimaldavad meil olla ühendatud, jagada informatsiooni ja kasutada ressursse viisil, mida varem ei olnud võimalik ette kujutada.

Võrkude mõistmine annab teile võimaluse mitte ainult paremini kasutada olemasolevaid teenuseid, vaid ka kujundada tuleviku digitaalset maailma. Sõltumata sellest, kas teie huvi on tehnilises lahendamises, turvalisuses või uute teenuste loomes, pakub võrgunduse valdkond lõputuid võimalusi.

Järgmisel tunnil saate praktilise kogemuse, kasutades Cisco Packet Tracer tarkvara oma esimese võrgu loomiseks. See annab teile käega katsutava ülevaate sellest, kuidas teoreetilised kontseptsioonid praktikas töötavad.