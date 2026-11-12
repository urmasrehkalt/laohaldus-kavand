# 4. Tarkvaraarhitektuur

## 4.1 Rakenduse tüüp: veebirakendus

Laohaldus on **veebirakendus**, mida kasutatakse brauseris.

- Kontoris kasutab juhataja arvutit, laos kasutab laotöötaja tahvelarvutit. Veebirakendus töötab mõlemal ilma paigalduseta.
- Uus versioon paigaldatakse ainult serverisse ja kõik kasutajad saavad selle korraga.
- Andmed on ühes kohas serveris, mitte iga töötaja arvutis.

| Tüüp | Miks mitte |
|---|---|
| Töölauarakendus | tuleb paigaldada ja uuendada igas arvutis, tahvlis ei tööta, andmete jagamiseks on ikkagi vaja serverit |
| Mobiilirakendus | tuleb teha kaks rakendust (Android ja iOS) või kasutada raamistikku, avaldamine poodides võtab aega; kontoris on mugavam suur ekraan |

Kuna serveripool on tehtud REST API-na, saab hiljem lisada ka mobiilirakenduse, mis kasutab sama API-t.

## 4.2 Arhitektuur: kihiline monoliit

Serveripool on **kihiline monoliit**: kogu äriloogika on ühes rakenduses, mis paigaldatakse ühe tervikuna, kuid selle sees on kood jagatud kihtidesse. Iga kiht suhtleb ainult enda all oleva kihiga. Veebiliides on eraldi React-rakendus, mis suhtleb serveriga REST API kaudu.

### Arhitektuuriskeem

![Arhitektuuriskeem](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/urmasrehkalt/laohaldus-kavand/main/diagrams/arhitektuur.puml)

Muudetavad lähtefailid: [arhitektuur.drawio](../diagrams/arhitektuur.drawio) (diagrams.net) ja [arhitektuur.puml](../diagrams/arhitektuur.puml) (PlantUML). Mõlemas on sama skeem.

### Komponendid

| Komponent | Ülesanne |
|---|---|
| Veebiliides (React SPA) | näitab andmeid, kontrollib vormi (nt kogus > 0), saadab päringud API-le |
| Turvakiht (Spring Security) | kontrollib, kas kasutaja on sisse loginud ja kas tema rollil on õigus seda teha |
| Esitluskiht: REST API kontrollerid | võtavad vastu HTTP päringu, teevad JSON-ist DTO (andmeedastusobjekti), tagastavad vastuse ja HTTP koodi |
| Teenuskiht (äriloogika) | ärireeglid: laoseis ei lähe alla nulli, tootekood on unikaalne, laoliikumine salvestatakse koos laoseisu muutusega ühes tehingus |
| Teavitused | saadab madala laoseisu ja kõlblikkusaja hoiatused e-postiga |
| Ajastatud tööd | käivitab igal ööl kõlblikkusaja kontrolli |
| Andmepääsukiht (Repository) | loeb ja salvestab olemeid, Spring Data JPA teeb neist SQL-päringud |
| PostgreSQL | hoiab tooteid, laoliikumisi ja kasutajaid ([andmemudel](#45-andmemudel)) |
| E-posti teenus (SMTP) | väline teenus, mis kirjad kohale toimetab |

Kihtide jaotus on sama, mis Java laohalduse harjutusülesandes: `ConsoleMenu` (sisend ja väljund) vastab siin kontrollerile ja veebiliidesele, `Inventory` (äriloogika) teenuskihile ja `Product` olemile. Menüü ei arvuta midagi ise ja äriloogika ei prindi midagi; samamoodi ei ole kontrolleris ärireegleid ega teenuskihis HTTP-d.

## 4.3 Kuidas andmed komponentide vahel liiguvad

Näide: laotöötaja väljastab laost 5 pakki piima (kasutajalugu US-05).

![Andmevoog](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/urmasrehkalt/laohaldus-kavand/main/diagrams/andmevoog.puml)

Lähtefail: [andmevoog.puml](../diagrams/andmevoog.puml)

1. **Veebiliides** kontrollib vormi (kogus peab olema positiivne) ja saadab päringu `POST /api/v1/stock/issue` JSON-andmetega `{"productId": 12, "quantity": 5}`. Ühendus on HTTPS-iga krüpteeritud.
2. **Turvakiht** kontrollib sessiooniküpsise järgi, kes kasutaja on ja kas tema rollil on õigus kaupa väljastada. Kui ei ole, tagastatakse kohe `401` või `403`.
3. **Kontroller** teeb JSON-ist DTO ja kutsub välja teenuse meetodi `issue(...)`.
4. **Teenuskiht** alustab andmebaasi tehingut, laeb toote lukustatult (`SELECT ... FOR UPDATE`, et kaks samaaegset väljastust ei saaks mõlemad sama kaupa võtta) ja kontrollib ärireeglit: laos peab olema piisavalt.
5. **Andmepääsukiht** salvestab uue koguse ja laoliikumise kirje. Mõlemad kirjutatakse ühes tehingus: kas mõlemad õnnestuvad või kumbki mitte.
6. Kui kogus langes alla miinimumi, annab teenuskiht **teavitusteenusele** käsu saata hoiatus. See käib asünkroonselt, et kasutaja ei peaks e-kirja saatmist ootama.
7. Vastus liigub tagasi: teenus tagastab tulemuse, kontroller teeb sellest JSON-i (`200 OK`) ja veebiliides näitab „Väljastatud, laos alles 7 tk“. Kui laos oli vähem, tagastab kontroller `409 Conflict` ja veebiliides näitab veateadet.

Andmete kuju muutub teel nii: **vormiväljad → JSON (HTTP) → DTO → olem (entity) → SQL-rida** ja tagasi samas järjekorras.

## 4.4 Eelised ja puudused

| Eelised | Puudused |
|---|---|
| Lihtne arendada ja testida: üks projekt, üks andmebaas | Kogu serveripool skaleerub ainult tervikuna |
| Lihtne paigaldada: üks rakendus ja üks andmebaas ühes serveris | Ühe osa viga (nt mälu lekkimine aruannetes) võib maha võtta kogu rakenduse |
| Tehingud on lihtsad, sest kõik andmed on ühes andmebaasis | Iga muudatuse järel paigaldatakse uuesti kogu rakendus |
| Väike meeskond saab kogu süsteemist aru | Kui rakendus väga suureks kasvab, muutub kood raskemini hallatavaks |
| Kihid hoiavad äriloogika eraldi ja seda saab testida ilma andmebaasi ja veebiliideseta | Kihtidest on vaja kinni pidada, muidu tekib „spagetikood“ |
| Odav ülal pidada: piisab ühest väikesest serverist | |
| REST API lubab hiljem lisada mobiilirakenduse | |

## 4.5 Andmemudel

![Andmemudel](https://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/urmasrehkalt/laohaldus-kavand/main/diagrams/andmemudel.puml)

Lähtefail: [andmemudel.puml](../diagrams/andmemudel.puml)

Laoseis (`product.quantity`) ja laoliikumised (`stock_movement`) muutuvad alati koos ühes tehingus. Laoliikumisi ei kustutata: nii saab iga koguse muutuse hiljem tagasi jälgida (kes, millal, miks). Lihtsustus: kõlblikkusaeg on tootel, mitte partiil. Kui klient hakkab sama toodet eri kõlblikkusaegadega partiidena hoidma, lisatakse tabel `batch`.

## 4.6 Võrdlus alternatiiviga: mikroteenused

Mikroteenuste arhitektuuris oleks iga ärivaldkond eraldi rakendus oma andmebaasiga: tooteteenus, laoteenus, kasutajateenus, aruandlusteenus ja teavitusteenus. Teenused suhtlevad omavahel võrgu kaudu (REST või sõnumijärjekord) ja kasutaja päringud jõuavad nendeni API-lüüsi (API gateway) kaudu.

| Tunnus | Kihiline monoliit (valitud) | Mikroteenused |
|---|---|---|
| Paigaldus | üks rakendus | 5 või rohkem eraldi rakendust, vaja konteinerite haldust (nt Kubernetes) |
| Skaleerimine | kogu rakendus korraga | iga teenus eraldi, ainult see, mis on koormatud |
| Andmed ja tehingud | üks andmebaas, lihtsad tehingud | iga teenuse oma andmebaas, teenuste vahel tehinguid ei ole, andmed võivad olla ajutiselt ebakõlas |
| Tõrkekindlus | üks viga võib peatada kõik | ühe teenuse viga ei pruugi teisi peatada |
| Arendus | lihtne, sobib väikesele meeskonnale | sobib mitmele meeskonnale, kes arendavad teenuseid sõltumatult |
| Testimine ja vigade otsimine | lihtne, kõik ühes kohas | keeruline, viga võib olla teenuste vahelises suhtluses |
| Kulu | üks väike server | mitu serverit, rohkem seadistust ja jälgimist |

**Järeldus.** Laohaldust kasutab kuni 10 inimest ja arendab üks väike meeskond. Mikroteenused lisaksid palju keerukust (võrguühendused, mitu andmebaasi, hajutatud tehingud), aga nende peamisi eeliseid (sõltumatu skaleerimine ja mitu meeskonda) see projekt ei vaja. Seepärast on valitud kihiline monoliit. Kood jagatakse ka monoliidi sees pakettideks ärivaldkondade järgi (tooted, ladu, kasutajad, aruanded), nii et vajaduse korral saab mõne osa (näiteks aruandluse) hiljem eraldi teenuseks tõsta.
