# 5. Git ja versiooniplaan

## 5.1 Semantiline versioonimine (SemVer)

Versiooninumber on kujul **MAJOR.MINOR.PATCH**, näiteks `1.4.2`.

| Osa | Suureneb, kui | Mis juhtub teiste osadega |
|---|---|---|
| **MAJOR** | tehakse muudatus, mis ei ühildu varasemaga (avalik API muutub nii, et vanad kliendid enam ei tööta) | MINOR ja PATCH lähevad nulliks: `1.4.2 → 2.0.0` |
| **MINOR** | lisatakse uus funktsioon, mis ei riku olemasolevat (tagasiühilduv) | PATCH läheb nulliks: `1.4.2 → 1.5.0` |
| **PATCH** | parandatakse viga, uut funktsiooni ei lisata | `1.4.2 → 1.4.3` |

Lisareeglid:

- **0.x.y** tähendab, et rakendus on alles arenduses ja kõik võib muutuda. Versioon **1.0.0** on esimene, mida klient päriselt kasutab, ja sellest alates kehtivad ühilduvuse reeglid.
- **Eelväljalase** märgitakse sidekriipsuga: `1.0.0-rc.1` on esimene väljalaskekandidaat, mida klient testib enne lõplikku `1.0.0` versiooni.
- Avaldatud versiooni sisu ei muudeta. Iga muudatus tähendab uut versiooninumbrit.

**Mis on laohalduse avalik API.** Väljastpoolt kasutatakse REST API-t (`/api/v1/...`): seda kasutab veebiliides ja hiljem võivad kasutada mobiilirakendus või raamatupidamistarkvara. Kui API muutub nii, et vana klient enam ei tööta, on see MAJOR-muudatus. Andmebaasi muudatused tehakse migratsioonidega, mis olemasolevaid andmeid ei kaota.

## 5.2 Versiooniplaan

| Versioon | Millal | Sisu |
|---|---|---|
| 0.1.0 | 1. sprindi lõpp | sisselogimine, tootekataloog, otsing (T-01, US-01 kuni US-03) |
| 0.2.0 | 2. sprindi lõpp | kauba vastuvõtt ja väljastamine, laoliikumiste ajalugu (US-04 kuni US-06) |
| 0.3.0 | 3. sprindi lõpp | madala laoseisu hoiatus, kasutajad ja rollid, kõlblikkusaeg (US-07 kuni US-09) |
| 0.4.0 | 4. sprindi lõpp | aruanne, CSV eksport, inventuur (US-10 kuni US-12) |
| 1.0.0-rc.1 | 5. sprindi keskel | väljalaskekandidaat kliendi vastuvõtutestiks |
| **1.0.0** | 5. sprindi lõpp | esimene tootmisversioon, pood hakkab kasutama |
| 1.0.x | vastavalt vajadusele | vigade parandused |
| 1.1.0 | pärast kasutuselevõttu | tarnijate register (US-13) |
| 2.0.0 | tulevikus | mitme lao tugi: API muutub (vt näide allpool) |

## 5.3 Näited: millal muutub MAJOR, MINOR või PATCH

| Muudatus laohalduses | Versioon enne | Versioon pärast | Miks |
|---|---|---|---|
| Kauba väljastamisel lubati negatiivne kogus, see parandatakse | 1.0.0 | **1.0.1** | vea parandus, uut funktsiooni ei ole |
| CSV-failis olid täpitähed katki | 1.0.1 | **1.0.2** | vea parandus |
| Lisatakse tarnijate register | 1.0.2 | **1.1.0** | uus funktsioon, olemasolev API töötab edasi |
| Tootele lisatakse uus valikuline väli „vöötkood“ | 1.1.0 | **1.2.0** | uus funktsioon, vanad päringud töötavad endiselt |
| Lisatakse mitme lao tugi: iga laoliikumise päringus on nüüd kohustuslik väli `warehouseId` | 1.2.0 | **2.0.0** | vana klient (nt mobiilirakendus), mis välja ei saada, saab vea, ühilduvus on katki |
| Väli `quantity` nimetatakse API-s ümber `stockQuantity`-ks | 1.2.0 | **2.0.0** | API-d kasutavad kliendid lähevad katki |

**Kui praegune versioon on 2.4.1:** veaparandus annab `2.4.2`, uus tagasiühilduv funktsioon annab `2.5.0` ja avalikku API-t lõhkuv muudatus annab `3.0.0`.

Et MAJOR-muudatus ei jätaks kliente kohe hätta, hoitakse vana API (`/api/v1`) mõnda aega alles ja uus tuleb aadressile `/api/v2`.

## 5.4 Kuidas versiooninumbreid väljalaskmisel kasutatakse

1. **Arendus harudes.** Iga kasutajalugu tehakse eraldi harus (`feature/5-kauba-valjastamine`, vea parandus `bugfix/31-csv-taptahed`). `main` haru on alati töökorras ja seda ei muudeta otse.
2. **Ühendamine.** Haru ühendatakse `main`-i pull request'iga pärast koodi ülevaatust ja testide läbimist.
3. **Versiooni määramine.** Väljalaske eel vaadatakse, mis on pärast eelmist versiooni muutunud: kas ainult parandused (PATCH), uued funktsioonid (MINOR) või ühilduvust rikkuvad muudatused (MAJOR).
4. **Versiooni tõstmine.** Versiooninumber muudetakse projekti failis (`pom.xml`, `package.json`) ja muudatuste loend kirjutatakse faili `CHANGELOG.md`.
5. **Silt (tag).** Commit märgitakse Giti sildiga: `git tag -a v1.1.0 -m "Tarnijate register"` ja `git push origin v1.1.0`.
6. **Väljalase.** GitHub Actions ehitab sildi peale rakenduse, märgib Dockeri tõmmise sama versiooniga (`laohaldus:1.1.0`) ja GitHubis tehakse *Release* koos muudatuste loendiga.
7. **Paigaldus.** Esmalt paigaldatakse versioon testkeskkonda, siis tootmisesse. Kui tootmises on viga, saab kiiresti tagasi minna eelmisele versioonile (`laohaldus:1.0.2`).
8. **Versioon on rakenduses näha.** Versiooninumber on veebiliidese jaluses ja aadressil `/api/version`. Kui kasutaja teatab veast, on kohe teada, millises versioonis viga on.

Näide muudatuste loendist (`CHANGELOG.md`):

```markdown
## [1.1.0] - 2027-03-15
### Lisatud
- Tarnijate register ja toote sidumine tarnijaga (#13)

## [1.0.1] - 2027-02-20
### Parandatud
- Kauba väljastamisel ei saa enam sisestada negatiivset kogust (#28)
```

## 5.5 Miks kasutatakse Gitis harusid

- **Pooleli töö ei sega teisi.** `main` jääb alati töökorda, isegi kui mõni funktsioon on pooleli.
- **Mitu inimest saab töötada korraga.** Iga arendaja töötab oma harus ja ühendab töö, kui see on valmis.
- **Koodi ülevaatus.** Pull request näitab kõiki haru muudatusi ja teine arendaja saab need üle vaadata enne ühendamist.
- **Kiire parandus.** Kui tootmises on viga, saab teha parandusharu, mis ei sõltu pooleli olevatest uutest funktsioonidest.
- **Katsetamine.** Uue lahenduse saab proovida eraldi harus ja kui see ei sobi, haru lihtsalt kustutatakse.

## 5.6 Commit'i sõnumid

Commit'i sõnum ütleb lühidalt, mida muudatus teeb, ja viitab issue'le: `#5: lisa kauba väljastamise API`. Selles repositooriumis on dokumentide commit'id kujul `docs: ...`. Iga commit on üks terviklik muudatus, nii et ajaloost on näha, kuidas kavand valmis.
