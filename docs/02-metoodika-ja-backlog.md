# 2. Arendusmetoodika ja tööde nimekiri

## 2.1 Valitud metoodika: Scrum

Laohalduse arendamiseks valisin Scrumi. Scrum on iteratiivse ja inkrementaalse mudeli ([1. peatükk](01-arendusprotsess.md)) praktiline töökord: töö käib kahenädalaste sprintidena ja iga sprindi lõpus on olemas töötav osa rakendusest.

Miks Scrum sobib sellele projektile:

1. **Klient vajab regulaarset ülevaadet.** Poe omanik ei ole tehniline inimene. Sprindi ülevaatusel (iga kahe nädala tagant) näeb ta töötavat rakendust ja saab kohe öelda, mis on valesti või puudu.
2. **Projektil on selge eesmärk ja tähtaeg.** Tuleb valmis saada esimene versioon (1.0.0), mida pood igapäevaselt kasutab. Scrumi sprindid ja meeskonna kiirus (velocity) aitavad ennustada, mis selleks ajaks valmib.
3. **Nõuded muutuvad.** Uus soov läheb backlogi ja võetakse ette järgmise sprindi planeerimisel. Käimasolev töö ei lähe segi.
4. **Meeskond on väike.** Scrum sobib 3 kuni 9 liikmega meeskonnale.

Pärast versiooni 1.0.0 läheb projekt hooldusesse. Siis tulevad tööd ebaühtlaselt (veateated, väiksed soovid) ja sobivam on **Kanban**, kus töid võetakse ette pidevalt, ilma sprintideta.

### Scrum ja Kanban

| Tunnus | Scrum | Kanban |
|---|---|---|
| Töö rütm | kindla pikkusega sprindid (meil 2 nädalat) | pidev voog, sprinte ei ole |
| Planeerimine | sprindi alguses valitakse sprinti tööd | uus töö võetakse ette, kui eelmine valmib |
| Muudatused | käimasoleva sprindi sisu ei muudeta | prioriteete võib muuta igal ajal |
| Rollid | tootejuht, Scrum Master, arendusmeeskond | kindlaid rolle ei nõuta |
| Piirang | sprindi maht (punktid) | samaaegsete tööde arv veerus (WIP-limiit) |
| Mõõdik | kiirus (punkte sprindis), burndown-graafik | läbimisaeg (lead time), tsükliaeg |
| Sobib | uue toote arendus selge eesmärgiga | hooldus, tugi, ettearvamatu tööde voog |

### Rollid

| Roll | Kes | Ülesanne |
|---|---|---|
| Tootejuht (Product Owner) | Nurgapoe juhataja kui kliendi esindaja | haldab backlogi, otsustab prioriteedid, võtab töö vastu |
| Scrum Master | üks arendajatest | korraldab Scrumi sündmusi, eemaldab takistusi |
| Arendusmeeskond | 3 arendajat (veebiliides, serveripool, testimine) | hindab, arendab ja testib |

### Sündmused ühes sprindis

| Sündmus | Millal | Kestus | Tulemus |
|---|---|---|---|
| Sprindi planeerimine | 1. päeva hommikul | 2 h | sprindi eesmärk ja sprindi backlog |
| Igapäevane koosolek | iga päev | 15 min | mida tegin, mida teen, mis takistab |
| Backlogi täpsustamine | sprindi keskel | 1 h | järgmise sprindi lood on hinnatud ja arusaadavad |
| Sprindi ülevaatus | viimasel päeval | 1 h | klient näeb töötavat osa, tagasiside läheb backlogi |
| Retrospektiiv | viimasel päeval | 1 h | mida meeskond järgmises sprindis teisiti teeb |

### Valmis olemise definitsioon (Definition of Done)

Töö on valmis, kui:

- kõik vastuvõtukriteeriumid on täidetud;
- kood on teise arendaja poolt üle vaadatud (pull request);
- automaattestid on kirjutatud ja läbivad GitHub Actionsis;
- muudatus töötab testkeskkonnas;
- vajadusel on dokumentatsioon uuendatud.

## 2.2 Tööde nimekiri (product backlog)

Prioriteedid on määratud **MoSCoW** meetodiga:

- **Must:** ilma selleta ei saa rakendust kasutusele võtta;
- **Should:** oluline, kuid esimene versioon toimiks ka ilma;
- **Could:** kasulik lisa, kui aega jääb;
- **Won't:** sellesse versiooni ei tule.

Hinnang on **lugemispunktides** (Fibonacci jada 1, 2, 3, 5, 8, 13), mis näitavad töö suhtelist mahtu ja keerukust, mitte tunde. Hinnangud antakse meeskonnaga *planning poker*'i abil. Eeldatav kiirus on 13 punkti sprindis. Esimese sprindi järel asendatakse eeldus tegeliku kiirusega.

| ID | Issue | Kasutajalugu | Prioriteet | Punktid | Sprint |
|---|---|---|---|---|---|
| T-01 | [#14](https://github.com/urmasrehkalt/laohaldus-kavand/issues/14) | Arenduskeskkond ja CI (tehniline töö) | Must | 3 | 1 |
| US-01 | [#1](https://github.com/urmasrehkalt/laohaldus-kavand/issues/1) | Sisselogimine | Must | 3 | 1 |
| US-02 | [#2](https://github.com/urmasrehkalt/laohaldus-kavand/issues/2) | Toote lisamine ja muutmine | Must | 5 | 1 |
| US-03 | [#3](https://github.com/urmasrehkalt/laohaldus-kavand/issues/3) | Toodete nimekiri ja otsing | Must | 2 | 1 |
| US-04 | [#4](https://github.com/urmasrehkalt/laohaldus-kavand/issues/4) | Kauba vastuvõtt | Must | 5 | 2 |
| US-05 | [#5](https://github.com/urmasrehkalt/laohaldus-kavand/issues/5) | Kauba väljastamine | Must | 5 | 2 |
| US-06 | [#6](https://github.com/urmasrehkalt/laohaldus-kavand/issues/6) | Laoliikumiste ajalugu | Should | 3 | 2 |
| US-07 | [#7](https://github.com/urmasrehkalt/laohaldus-kavand/issues/7) | Madala laoseisu hoiatus | Should | 5 | 3 |
| US-08 | [#8](https://github.com/urmasrehkalt/laohaldus-kavand/issues/8) | Kasutajate ja rollide haldus | Should | 5 | 3 |
| US-09 | [#9](https://github.com/urmasrehkalt/laohaldus-kavand/issues/9) | Kõlblikkusaja jälgimine | Should | 3 | 3 |
| US-10 | [#10](https://github.com/urmasrehkalt/laohaldus-kavand/issues/10) | Laoseisu aruanne ja lao koguväärtus | Could | 3 | 4 |
| US-11 | [#11](https://github.com/urmasrehkalt/laohaldus-kavand/issues/11) | Aruande eksport CSV-failina | Could | 2 | 4 |
| US-12 | [#12](https://github.com/urmasrehkalt/laohaldus-kavand/issues/12) | Inventuur | Could | 8 | 4 |
| T-02 | [#15](https://github.com/urmasrehkalt/laohaldus-kavand/issues/15) | Kasutuselevõtt: tootmisserver, varukoopiad, andmete import Excelist | Must | 5 | 5 |
| US-13 | [#13](https://github.com/urmasrehkalt/laohaldus-kavand/issues/13) | Tarnijate register | Won't (v1.1.0) | 5 | hiljem |

Viies sprint on ette nähtud kasutuselevõtuks, kliendi vastuvõtutestiks ja leitud vigade parandamiseks. Kokku kulub versioonini 1.0.0 viis sprinti ehk 10 nädalat.

### Kasutajalood ja vastuvõtukriteeriumid

**T-01 Arenduskeskkond ja CI.** Arendajana tahan valmis projekti põhja ja automaatset kontrolli, et iga muudatus oleks kohe testitud.
- Spring Boot ja React projektid on repositooriumis ja käivituvad ühe käsuga.
- PostgreSQL töötab kohalikult Dockeris, andmebaasi skeem luuakse migratsioonidega (Flyway).
- GitHub Actions ehitab projekti ja käivitab testid iga pull request'i peale.

**US-01 Sisselogimine.** Töötajana tahan sisse logida oma e-posti ja parooliga, et laoandmeid näeksid ja muudaksid ainult ettevõtte töötajad.
- Vale parooli korral ei pääse sisse ja kuvatakse veateade.
- Sisse loginud kasutaja näeb oma nime ja rolli.
- Väljalogimine lõpetab sessiooni. Pärast 30 minutit tegevusetust logitakse kasutaja automaatselt välja.

**US-02 Toote lisamine ja muutmine.** Juhatajana tahan lisada uue toote ja muuta olemasolevat, et tootekataloog oleks ajakohane.
- Tootel on nimi, tootekood, kategooria, ühik, hind ja miinimumkogus.
- Tootekood on unikaalne: olemasoleva koodiga toodet lisada ei saa.
- Hind ja miinimumkogus ei saa olla negatiivsed.
- Toidukaubale saab märkida kõlblikkusaja, elektroonikale garantii kuudes.

**US-03 Toodete nimekiri ja otsing.** Laotöötajana tahan näha toodete nimekirja koos laoseisuga ja otsida toodet nime või koodi osa järgi, et leida kiiresti õige toode.
- Nimekirjas on nimi, tootekood, laoseis ja ühik.
- Otsing leiab osalise vaste („pii“ leiab „Piim“) ega sõltu suur- ja väiketähtedest.

**US-04 Kauba vastuvõtt.** Laotöötajana tahan registreerida saabunud kauba, et laoseis suureneks kohe.
- Kogus peab olema positiivne täisarv.
- Pärast salvestamist on toote laoseis suurenenud.
- Tekib laoliikumise kirje: tüüp „vastuvõtt“, kogus, aeg ja kasutaja.

**US-05 Kauba väljastamine.** Laotöötajana tahan registreerida kauba väljastamise laost, et laoseis väheneks.
- Laoseisust rohkem väljastada ei saa, kuvatakse teade „Laos on ainult X tk“.
- Kaks samaaegset väljastust ei vii laoseisu negatiivseks.
- Tekib laoliikumise kirje: tüüp „väljastus“, kogus, aeg ja kasutaja.

**US-06 Laoliikumiste ajalugu.** Juhatajana tahan näha toote kõiki laoliikumisi, et puudujäägi korral põhjus välja selgitada.
- Ajaloos on kuupäev, tüüp, kogus, kasutaja ja kommentaar, uuemad eespool.
- Ajalugu saab filtreerida kuupäevavahemiku järgi.

**US-07 Madala laoseisu hoiatus.** Juhatajana tahan saada teavituse, kui toote kogus langeb alla miinimumi, et jõuaksin kauba õigel ajal tellida.
- Alla miinimumi tooted on nimekirjas esile tõstetud.
- Kui kogus langeb väljastamisel alla miinimumi, saadetakse juhatajale e-kiri.
- Sama toote kohta ei saadeta päevas üle ühe kirja.

**US-08 Kasutajate ja rollide haldus.** Administraatorina tahan lisada töötajaid ja määrata neile rolli, et igaüks teeks ainult oma tööks vajalikke toiminguid.
- Rollid: laotöötaja (vastuvõtt, väljastus, otsing), juhataja (lisaks tooted ja aruanded), administraator (lisaks kasutajad).
- Lahkunud töötaja konto saab sulgeda, tema tehtud laoliikumised jäävad alles.

**US-09 Kõlblikkusaja jälgimine.** Juhatajana tahan näha tooteid, mille kõlblikkusaeg lõpeb 7 päeva jooksul, et aeguv kaup jõuaks müüki või allahindlusse.
- Eraldi vaade näitab aeguvaid ja aegunud tooteid.
- Igal hommikul saadetakse juhatajale nende kohta kokkuvõte.

**US-10 Laoseisu aruanne ja lao koguväärtus.** Juhatajana tahan näha laoseisu kategooriate kaupa ja lao koguväärtust, et teada, kui palju raha on kaupa seotud.
- Koguväärtus on kõigi toodete hind korda kogus.
- Aruannet saab filtreerida kategooria järgi.

**US-11 Aruande eksport CSV-failina.** Juhatajana tahan laoseisu aruande alla laadida CSV-failina, et saata see raamatupidajale.
- Fail avaneb Excelis õigesti (täpitähed, kümnendkoha eraldaja).

**US-12 Inventuur.** Laotöötajana tahan sisestada inventuuril loendatud kogused, et süsteem näitaks erinevusi ja parandaks laoseisu.
- Inventuuri lõpus näidatakse süsteemi koguse ja loendatud koguse erinevusi.
- Kinnitamisel tekib iga erinevuse kohta parandusliikumine.

**US-13 Tarnijate register.** Juhatajana tahan hoida tarnijate kontakte ja siduda tooted tarnijaga, et teaksin, kellelt tellida. Tuleb versiooni 1.1.0.

## 2.3 Esimese sprindi plaan

**Sprindi eesmärk:** töötaja saab sisse logida ning tootekataloogis tooteid lisada, muuta ja otsida.

**Sprindi backlog (13 punkti):** T-01, US-01, US-02, US-03.

| Lugu | Issue | Ülesanded |
|---|---|---|
| T-01 | [#14](https://github.com/urmasrehkalt/laohaldus-kavand/issues/14) | Spring Boot ja React projekti põhi; PostgreSQL `docker-compose` failis; esimene Flyway migratsioon; GitHub Actionsi töövoog (ehitamine ja testid) |
| US-01 | [#1](https://github.com/urmasrehkalt/laohaldus-kavand/issues/1) | tabel `app_user` ja algne administraator; Spring Security sessioonipõhine sisselogimine; sisselogimise vorm; väljalogimine; testid |
| US-02 | [#2](https://github.com/urmasrehkalt/laohaldus-kavand/issues/2) | tabelid `product` ja `category`; `POST` ja `PUT /api/v1/products`; kontrollid teenuskihis (unikaalne kood, hind ≥ 0); toote vorm; testid |
| US-03 | [#3](https://github.com/urmasrehkalt/laohaldus-kavand/issues/3) | `GET /api/v1/products?q=`; toodete nimekirja vaade; otsingu testid |

**Ajakava (10 tööpäeva):**

| Päev | Tegevus |
|---|---|
| 1 | sprindi planeerimine, T-01 algus |
| 2 kuni 3 | T-01 valmis, US-01 |
| 4 kuni 7 | US-02 (serveripool ja vorm paralleelselt) |
| 6 | backlogi täpsustamine: US-04, US-05 ja US-06 sprindiks 2 |
| 8 kuni 9 | US-03, testimine, vigade parandus |
| 10 | sprindi ülevaatus kliendiga, retrospektiiv, väljalase 0.1.0 |

Igal hommikul on 15-minutiline koosolek tahvli ees.

## 2.4 Tööde planeerimine ja jälgimine

Tööd on GitHubis: iga kasutajalugu on **issue** ja kõik issue'd on **GitHub Projects** tahvlil ([„Laohaldus: backlog“](https://github.com/users/urmasrehkalt/projects/1)).

Tahvli väljad:

- **Status:** *Todo*, *In Progress*, *Done*;
- **Prioriteet:** Must, Should, Could, Won't;
- **Punktid:** hinnang lugemispunktides;
- **Sprint:** millisesse sprinti töö on planeeritud.

Kuidas töö liigub:

1. **Planeerimine.** Sprindi planeerimisel valib meeskond backlogi tipust nii palju lugusid, kui kiirus lubab, ja märgib neile välja *Sprint*. Lood jagatakse ülesanneteks (issue sees märkeruutudena).
2. **Arendus.** Arendaja võtab loo, liigutab selle veergu *In Progress* ja loob haru `feature/<issue-number>-nimi`. Korraga on ühel arendajal töös üks lugu.
3. **Ülevaatus ja valmis.** Pull request viitab issue'le (`Closes #5`). Kui kood on üle vaadatud ja testid läbivad, ühendatakse haru ning issue sulgub ja liigub veergu *Done*.
4. **Jälgimine.** Igapäevasel koosolekul vaadatakse tahvlit: mis on töös, mis seisab. Sprindi edenemist näitab GitHub Projectsi *Insights* graafik (burn-up: valmis punktid kogumahu suhtes), sprindi lõpus loetakse kokku valmis punktid ja see on meeskonna tegelik kiirus.
5. **Muudatused.** Uus soov lisatakse uue issue'na veergu *Todo* ilma sprindita. Tootejuht annab sellele prioriteedi ja järgmisel planeerimisel otsustatakse, millal see tehakse.
