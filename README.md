# Sistem za analizo nogometnih tekem z uporabo računalniškega vida

Diplomska naloga, objavljena leta 2026.

Repozitorij vsebuje celoten sistem za analizo nogometnih tekem z računalniškim
vidom — izvorno kodo za detekcijo, sledenje, določanje pripadnosti ekipam ter
projekcijo na 2D model igrišča — skupaj z ročno anotiranimi referenčnimi
podatki (angl. *ground truth*) in evalvacijskimi rezultati, uporabljenimi za
kvantitativno vrednotenje sistema.

## Vsebina repozitorija

| Datoteka / mapa | Opis |
|---|---|
| `notebooks/sistem_za_analizo.ipynb` | Glavni Jupyter/Colab zvezek s celotnim sistemom: detekcija (RF-DETR), sledenje igralcev (SAM2) in žoge (Kalmanov filter), določanje pripadnosti ekipam (SigLIP + UMAP + KMeans), detekcija ključnih točk igrišča, homografska projekcija in prepoznavanje dogodkov. |
| `notebooks/evalvacija.ipynb` | Evalvacijski zvezek, ki napovedi sistema primerja z ročno anotiranimi referenčnimi podatki (`gt/`) in izračuna standardne MOT metrike (MOTA, IDF1, IDSW, net IDSW) ter metrike za sledenje majhnim objektom (Tracking Rate, CLE, Precision@k, Success AUC). |
| `gt/` | Ročno anotirane sledi objektov (igralci, vratarji, sodniki, žoga) v formatu MOT 1.1, izvožene iz orodja [CVAT.ai](https://www.cvat.ai/), za vsak od štirih video posnetkov. |
| `videos/` | Izvorni video posnetki nogometnih tekem, nad katerimi je bila izvedena anotacija in nad katerimi teče celoten sistem: `bay-psg-shot.mp4`, `bay-psg-base.mp4`, `bay-psg-long.mp4`, `int-como.mp4`. |
| `results/` | Rezultati sledenja in evalvacije (`.csv`, `.pkl`) za vse štiri video posnetke, uporabljeni v diplomski nalogi. |

## Video posnetki

Sistem in evalvacija sta bila izvedena na štirih video posnetkih nogometnih tekem:

- **`bay-psg-shot.mp4`** — kratek izsek s strelom na gol.
- **`psg-bay.mp4`** — osnovni referenčni posnetek.
- **`psg-bay-long.mp4`** — daljši posnetek za vrednotenje stabilnosti sledenja skozi čas.
- **`como-int 16.36.24.mp4`** — posnetek druge tekme, uporabljen za preverjanje posplošljivosti sistema na drugačne dresne barve, kamere in osvetlitev.

Za vsak posnetek so na voljo pripadajoče `gt.txt` in `labels.txt` datoteke v mapi `gt/`.

## Postopek pridobitve referenčnih podatkov

1. Vsak izvorni video posnetek je bil naložen v orodje CVAT.ai.
2. Za vsak okvir videa so bili ročno označeni vsi vidni objekti (igralci,
   vratarji, sodniki, žoga) z ustreznimi mejnimi okvirji (angl. *bounding
   box*) in identifikatorji sledi.
3. Anotacije so bile izvožene v formatu **MOT 1.1**, kar ustvari datoteki
   `gt.txt` (podatki o sledeh) in `labels.txt` (definicija razredov) za
   vsak posnetek.
4. Te referenčne podatke uporablja evalvacijski zvezek (`evalvacija.ipynb`)
   za primerjavo napovedanih sledi (RF-DETR + SAM2 za igralce, RF-DETR +
   Kalmanov filter za žogo) z ročno anotiranimi resničnimi vrednostmi, pri
   čemer se izračunajo standardne metrike za sledenje več objektom (MOTA,
   IDF1, IDSW, net IDSW) ter dodatne metrike za sledenje majhnim objektom
   (Tracking Rate, CLE, Precision@k, Success AUC).

## Format `gt.txt` (MOT 1.1)

Vsaka vrstica predstavlja eno detekcijo objekta v enem okvirju:

frame, track_id, x, y, w, h, not_ignored, class_id, visibility

- `frame` — zaporedna številka okvirja
- `track_id` — enotni identifikator sledi znotraj videa
- `x, y, w, h` — mejni okvir objekta (zgornji levi kot + širina/višina)
- `not_ignored` — 1, če vrstica šteje pri vrednotenju, 0 če je ignorirana
- `class_id` — indeks razreda (1-indeksiran, glej `labels.txt`)
- `visibility` — stopnja vidnosti objekta v tem okvirju

## Format `labels.txt`

Seznam imen razredov, po eno na vrstico, v vrstnem redu, ki ustreza
`class_id` v `gt.txt` (1-indeksirano):

Player
Referee
Ball
Goalkeeper

## Uporaba

1. Odpri `notebooks/sistem_za_analizo.ipynb` v Google Colabu ali lokalnem
   Jupyter okolju in ga poženi nad izbranim posnetkom iz mape `videos/`, da
   dobiš napovedane sledi (izhodni `.pkl` in `.mp4`).
2. Odpri `notebooks/evalvacija.ipynb` in nastavi poti do generiranih
   napovedi ter ustrezne `gt.txt`/`labels.txt` datoteke za isti posnetek iz
   mape `gt/`.
3. Zaženi evalvacijo — rezultati (MOT metrike za igralce, metrike za
   sledenje žogi, net IDSW analiza) se izpišejo in shranijo v `results/`.

Ključno je, da se pri vrednotenju vedno uporablja izvorni video posnetek iz
mape `videos/`, ki ustreza referenčnim podatkom — drugačna ločljivost ali
frame rate bi povzročila neusklajenost med napovedmi in referenčnimi podatki.
