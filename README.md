# Laohaldus: tarkvaraprojekti kavand

Väikese ettevõtte laohaldusrakenduse arenduse kavand. Rakendust ei ole programmeeritud: repositoorium sisaldab kavandamise dokumente ja skeeme, mis näitavad, kuidas, miks ja milliste vahenditega rakendust arendataks.

Autor: Urmas Rehkalt, grupp TAK25

## Probleem ja lahendus

Näidisklient on väike jaekaubandusettevõte Nurgapood OÜ, kes müüb toidukaupa ja väikeelektroonikat. Laoseisu peetakse praegu Exceli tabelis, mida uuendatakse käsitsi. Selle tõttu saab kaup ootamatult otsa, aegunud toit avastatakse alles riiulilt ja keegi ei tea täpselt, kes ja millal laoseisu muutis.

Laohaldusrakendus on veebirakendus, kus:

- laotöötaja võtab kauba vastu ja väljastab selle, laoseis muutub kohe;
- iga laoliikumine salvestatakse koos aja ja tegijaga;
- juhataja saab teavituse, kui toote kogus langeb alla miinimumi või kõlblikkusaeg hakkab lõppema;
- juhataja näeb laoseisu aruannet ja lao koguväärtust.

Kasutajad: laotöötaja, juhataja ja administraator. Rakendust kasutatakse kontoriarvutis ja laos tahvelarvutis.

## Kus mis asub

| Teema | Dokument |
|---|---|
| Arendusprotsess, mudelite võrdlus, nõuete muutmine | [docs/01-arendusprotsess.md](docs/01-arendusprotsess.md) |
| Arendusmetoodika (Scrum), kasutajalood, backlog, 1. sprindi plaan | [docs/02-metoodika-ja-backlog.md](docs/02-metoodika-ja-backlog.md) |
| CASE-vahendid, nende võrdlus ja kasutamise näited | [docs/03-case-vahendid.md](docs/03-case-vahendid.md) |
| Tarkvaraarhitektuur, andmevoog, alternatiivi võrdlus | [docs/04-arhitektuur.md](docs/04-arhitektuur.md) |
| Versiooniplaan (SemVer) ja väljalasete tegemine | [docs/05-versiooniplaan.md](docs/05-versiooniplaan.md) |

Tööde haldamine: GitHub Projects tahvel (link lisatakse siia pärast tahvli loomist), kasutajalood on repositooriumi [issue'des](../../issues).

## Skeemid ja nende muudetavad lähtefailid

Kõik skeemid on tekstifailid, mida saab muuta. Pildid ei ole repositooriumis eraldi failidena, sest GitHub joonistab PlantUML-skeemid lähtefailist otse dokumentidesse.

| Skeem | Lähtefail | Vahend |
|---|---|---|
| Arhitektuuriskeem | [diagrams/arhitektuur.drawio](diagrams/arhitektuur.drawio) | diagrams.net |
| Arhitektuuriskeem (tekstina) | [diagrams/arhitektuur.puml](diagrams/arhitektuur.puml) | PlantUML |
| Andmevoog: kauba väljastamine | [diagrams/andmevoog.puml](diagrams/andmevoog.puml) | PlantUML |
| Andmemudel (ER) | [diagrams/andmemudel.puml](diagrams/andmemudel.puml) | PlantUML |
| Kasutusjuhud | [diagrams/kasutusjuhud.puml](diagrams/kasutusjuhud.puml) | PlantUML |

Kuidas skeemi muuta:

- **diagrams.net:** ava <https://app.diagrams.net>, vali *Open Existing Diagram* ja ava fail `arhitektuur.drawio` (või *File > Open from > GitHub*). Pärast muutmist salvesta fail ja tee commit.
- **PlantUML:** muuda `.puml` faili teksti. Eelvaate näeb veebiredaktoris <https://www.plantuml.com/plantuml> (kleebi tekst sinna) või VS Code'i PlantUML-laiendusega.

## Kavandatud tehnoloogiad

| Osa | Valik |
|---|---|
| Rakenduse tüüp | veebirakendus (töötab brauseris arvutis ja tahvlis) |
| Arhitektuur | kihiline monoliit, eraldi veebiliides ja REST API |
| Veebiliides | React + TypeScript |
| Serveripool | Java 21, Spring Boot, Spring Security, Spring Data JPA |
| Andmebaas | PostgreSQL |
| Versioonihaldus ja CI | Git, GitHub, GitHub Actions |
| Tööde haldus | GitHub Projects |
| Modelleerimine | diagrams.net, PlantUML |

## Repositooriumi struktuur

```
README.md          projekti kirjeldus ja juhised (see fail)
docs/              kavandamise dokumendid
diagrams/          skeemide muudetavad lähtefailid (.drawio, .puml)
```

## Tehisintellekti kasutamine

Dokumentide ja skeemide mustandite koostamisel kasutati tehisintellekti (Claude). Valikud, põhjendused ja skeemid on autori poolt üle vaadatud ning autor oskab neid selgitada ja muuta.
