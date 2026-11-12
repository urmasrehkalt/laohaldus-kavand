# 3. CASE-vahendid

CASE-vahend (*Computer-Aided Software Engineering*) on tarkvara, mis toetab tarkvara arendamist: nõuete kirjapanekut, modelleerimist, planeerimist, versioonihaldust, testimist. Vahendeid jagatakse sageli kaheks:

- **ülemised CASE-vahendid** toetavad varaseid etappe (analüüs ja disain), näiteks diagrams.net, PlantUML, Visual Paradigm;
- **alumised CASE-vahendid** toetavad arendust, testimist ja hooldust, näiteks Git, IDE, CI-süsteemid.

## 3.1 Kasutatud vahendid

| Vahend | Liik | Milleks seda projektis kasutasin | Toetatud arendustegevus |
|---|---|---|---|
| **diagrams.net** (draw.io) | modelleerimine, ülemine CASE | [arhitektuuriskeem](../diagrams/arhitektuur.drawio) | disain |
| **PlantUML** | modelleerimine, ülemine CASE | [kasutusjuhud](../diagrams/kasutusjuhud.puml), [andmevoog (jadaskeem)](../diagrams/andmevoog.puml), [andmemudel (ER)](../diagrams/andmemudel.puml), [arhitektuur tekstina](../diagrams/arhitektuur.puml), [kasutajaliidese visandid](04-arhitektuur.md#47-kasutajaliidese-visandid) | nõuete analüüs, disain |
| **GitHub Projects** | projektihaldus | backlog, prioriteedid, sprindid, tahvel | planeerimine, jälgimine |
| **Git ja GitHub** | versioonihaldus, alumine CASE | kõik dokumendid ja skeemid on versioonihalduses, iga muudatus on commit | konfiguratsioonihaldus, koostöö |

Arenduse ajal lisanduksid alumised CASE-vahendid: IntelliJ IDEA ja VS Code (koodi kirjutamine), GitHub Actions (automaatne ehitamine ja testimine), Docker (ühesugune keskkond igal arendajal ja serveris).

### Milliseid probleeme CASE-vahendid lahendavad

- **Arusaamine.** Skeemi järgi saavad klient, arendaja ja testija sama pildi süsteemist. Kasutusjuhtude skeemi saab näidata kliendile, kes koodi ei loe.
- **Vead leitakse varem.** Andmemudelit joonistades selgub, et kõlblikkusaeg on tootel, mitte partiil, ja seda saab arutada enne, kui kood on kirjutatud.
- **Dokumentatsioon püsib ajakohane.** PlantUML-skeem on tekst samas repositooriumis, seda muudetakse koos muu tööga ja muudatus on Giti ajaloos näha.
- **Töö on nähtav.** Projektitahvlilt näeb igaüks, mis on tehtud, mis on töös ja mis järgmisena tuleb.
- **Muudatused on jälgitavad.** Gitist näeb, kes, millal ja miks midagi muutis, ja vajaduse korral saab vana versiooni tagasi.

## 3.2 Modelleerimisvahendite võrdlus

| Tunnus | diagrams.net | PlantUML | Visual Paradigm |
|---|---|---|---|
| Kuidas skeem tehakse | hiirega lohistades | kirjeldatakse tekstina, pilt joonistatakse automaatselt | hiirega, UML-reeglite kontrolliga |
| Hind | tasuta | tasuta, avatud lähtekoodiga | tasuline, piiratud tasuta versioon |
| Paigaldus | brauseris või töölauarakendus | veebiredaktor, VS Code'i laiendus või Java programm | töölauarakendus |
| Õppimine | väga lihtne | tuleb õppida süntaksit | keerukam, palju funktsioone |
| Paigutus | käsitsi, täielik kontroll | automaatne, vähe kontrolli | käsitsi |
| Muudatused Gitis | XML-fail, muudatust on raske lugeda | tavaline tekst, `git diff` näitab täpselt, mis muutus | binaarne projektifail |
| UML-reeglite kontroll | puudub, võib joonistada ükskõik mida | osaline (süntaks) | jah, mudel on kooskõlas üle kõigi skeemide |
| Koodi genereerimine | ei | ei | jah (nt Java klassid, andmebaasi skeem) |
| Sobib | arhitektuuri- ja infrastruktuuriskeemid, kiire visand, esitlus | UML-skeemid, mis elavad koodi kõrval ja muutuvad tihti | suured projektid, kus mudel on peamine dokument |

**Mida võrdlusest järeldan.** Tegin arhitektuuriskeemi mõlemas vahendis, et neid päriselt võrrelda. Diagrams.net-is sai skeemi paigutuse täpselt selliseks, nagu tahtsin, ja see sobib kliendile näitamiseks. PlantUML-is on sama skeem umbes 40 rida teksti: uue komponendi lisamiseks piisab kahest reast ja Gitist on näha, mis täpselt muutus, kuid paigutust valib vahend ise. Seepärast kasutan PlantUML-i skeemide jaoks, mis muutuvad koos koodiga (jadaskeem, andmemudel), ja diagrams.net-i ülevaatlike skeemide jaoks. Visual Paradigmi eelised (mudeli kooskõla ja koodi genereerimine) tulevad välja suurtes projektides; selle projekti jaoks on see liiga kallis ja keeruline.

## 3.3 Projektihaldusvahendite võrdlus

| Tunnus | GitHub Projects (valitud) | Trello | Jira |
|---|---|---|---|
| Hind väikesele meeskonnale | tasuta | tasuta | tasuta kuni 10 kasutajat |
| Seos koodiga | töö on sama issue, mille sulgeb pull request | lisamoodul | integratsioon GitHubiga |
| Scrumi tugi | väljad, iteratsioonid, tahvel, graafikud | ainult tahvel | täielik: sprindid, burndown, aruanded |
| Õppimine | lihtne | väga lihtne | keerukam |
| Sobib | arendusmeeskond, kes töötab niikuinii GitHubis | lihtne ülesannete nimekiri, mitte ainult arendus | suured meeskonnad ja ettevõtted |

GitHub Projects on valitud, sest kood, dokumendid ja tööd on samas kohas: pull request'is kirjutatud `Closes #5` sulgeb kasutajaloo ja liigutab selle tahvlil veergu *Done*.

## 3.4 Kasutamise näited

### Kasutusjuhtude skeem (PlantUML)

![Kasutusjuhud](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/urmasrehkalt/laohaldus-kavand/main/diagrams/kasutusjuhud.puml)

### Näide 1: uue komponendi lisamine PlantUML-is

Kui klient soovib, et laoseisu saaks lugeda ka raamatupidamistarkvara, lisatakse failis [arhitektuur.puml](../diagrams/arhitektuur.puml) kaks rida:

```plantuml
component "Raamatupidamise\nliides" as acc #F5F5F5
acc --> api : REST, JSON
```

Pärast commit'i joonistab GitHub dokumendis skeemi uuesti ja `git diff` näitab täpselt need kaks lisatud rida.

### Näide 2: komponendi lisamine diagrams.net-is

1. Ava <https://app.diagrams.net>, vali *Open Existing Diagram* ja fail `diagrams/arhitektuur.drawio`.
2. Lohista vasakult paneelilt ristkülik õigesse kohta, topeltklõpsa ja kirjuta nimi.
3. Vii hiir ühe komponendi kohale, lohista sinine nool teise komponendini, topeltklõpsa noolel ja kirjuta silt (nt „REST, JSON“).
4. Konteineri (näiteks „Rakendusserver“) sisse lohistatud komponent liigub edaspidi koos konteineriga.
5. *File > Save* ja seejärel `git commit`.

### Näide 3: töö lisamine GitHub Projectsi

1. Repositooriumis *Issues > New issue*: pealkiri „US-14 Vöötkoodi skannimine“, sisu kasutajaloo kujul koos vastuvõtukriteeriumidega.
2. Paremalt *Projects*: lisa issue tahvlile, määra *Prioriteet*, *Punktid* ja *Sprint*.
3. Tahvlil lohista kaart õigesse veergu.
