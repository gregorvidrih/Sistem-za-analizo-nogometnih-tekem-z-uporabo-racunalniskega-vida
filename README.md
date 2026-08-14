# Sistem-za-analizo-nogometnih-tekem-z-uporabo-ra-unalni-kega-vida
Diplomska naloga objavljena leta 2026

Repozitorij vsebuje ročno anotirane referenčne podatke (angl. *ground truth*),
uporabljene za kvantitativno vrednotenje sistema za analizo nogometnih tekem
z računalniškim vidom.

## Vsebina repozitorija

| Datoteka | Opis |
|---|---|
| `gt.txt` | Ročno anotirane sledi objektov (igralci, vratarji, sodniki, žoga) v formatu MOT 1.1, izvožene iz orodja [CVAT.ai](https://www.cvat.ai/). |
| `labels.txt` | Seznam razredov (`Player`, `Referee`, `Ball`, `Goalkeeper`), uporabljenih pri anotaciji v CVAT, v vrstnem redu, ki ustreza `class_id` stolpcu v `gt.txt`. |
| `<bay-psg-shot>.mp4` | Izvorni video posnetek nogometne tekme, nad katerim je bila izvedena anotacija in nad katerim teče celoten sistem za detekcijo, sledenje in analizo. |

## Postopek pridobitve referenčnih podatkov

1. Izvorni video posnetek je bil naložen v orodje CVAT.ai.
2. Za vsak okvir videa so bili ročno označeni vsi vidni objekti (igralci,
   vratarji, sodniki, žoga) z ustreznimi mejnimi okvirji (angl. *bounding
   box*) in identifikatorji sledi.
3. Anotacije so bile izvožene v formatu **MOT 1.1**, kar ustvari datoteki
   `gt.txt` (podatki o sledeh) in `labels.txt` (definicija razredov).
4. Te referenčne podatke uporablja evalvacijski del sistema za primerjavo
   napovedanih sledi (RF-DETR + SAM2 za igralce, RF-DETR + Kalmanov filter
   za žogo) z ročno anotiranimi resničnimi vrednostmi, pri čemer se
   izračunajo standardne metrike za sledenje več objektom (MOTA, IDF1,
   IDSW) ter dodatne metrike za sledenje majhnim objektom (Tracking Rate,
   CLE, Precision@k, Success AUC).

## Format `gt.txt` (MOT 1.1)

Vsaka vrstica predstavlja eno detekcijo objekta v enem okvirju:

```
frame, track_id, x, y, w, h, not_ignored, class_id, visibility
```

- `frame` — zaporedna številka okvirja
- `track_id` — enotni identifikator sledi znotraj videa
- `x, y, w, h` — mejni okvir objekta (zgornji levi kot + širina/višina)
- `not_ignored` — 1, če vrstica šteje pri vrednotenju, 0 če je ignorirana
- `class_id` — indeks razreda (1-indeksiran, glej `labels.txt`)
- `visibility` — stopnja vidnosti objekta v tem okvirju

## Format `labels.txt`

Seznam imen razredov, po eno na vrstico, v vrstnem redu, ki ustreza
`class_id` v `gt.txt` (1-indeksirano):

```
Player
Referee
Ball
Goalkeeper
```

## Uporaba

Referenčni podatki se uporabljajo skupaj z evalvacijskimi skriptami iz
glavnega sistema (glej ločen repozitorij/notebook za sledenje in
projekcijo na 2D model igrišča). Video posnetek v tem repozitoriju je
identičen tistemu, ki je bil uporabljen za generiranje `gt.txt`, zato je
ključno, da se pri vrednotenju uporablja prav ta izvorni posnetek —
drugačna ločljivost ali frame rate bi povzročila neusklajenost med
napovedmi in referenčnimi podatki.
