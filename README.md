# Sistem za analizo nogometnih tekem z uporabo računalniškega vida

Diplomska naloga, objavljena leta 2026.

Repozitorij vsebuje celoten sistem za analizo nogometnih tekem z računalniškim
vidom — glavni zvezek s sistemom ter evalvacijski zvezek — skupaj z rezultati
sledenja in evalvacije, pridobljenimi na šestih testnih videoposnetkih, ki so
bili uporabljeni za kvantitativno vrednotenje sistema v diplomski nalogi.

## Vsebina repozitorija

| Datoteka / mapa | Opis |
|---|---|
| `sistem_za_analizo.ipynb` | Glavni Jupyter/Colab zvezek s celotnim sistemom: detekcija (RF-DETR), sledenje igralcev (SAM2) in žoge (Kalmanov filter), določanje pripadnosti ekipam (SigLIP + UMAP + KMeans), detekcija ključnih točk igrišča, homografska projekcija in prepoznavanje dogodkov. |
| `evalvacija.ipynb` | Evalvacijski zvezek, ki napovedi sistema primerja z ročno anotiranimi referenčnimi podatki in izračuna standardne MOT metrike (MOTA, MOTP, IDF1, IDSW, net IDSW) ter metrike za sledenje majhnim objektom (Tracking Rate, CLE, Precision@k, Success AUC). |

Preostanek repozitorija je razdeljen na dve vrsti map: **mape posameznih
testnih posnetkov** (rezultati in evalvacija za en sam video) ter **zbirne
mape** (rezultati, združeni čez vseh šest posnetkov, uporabljeni za izračun
povprečij oziroma vsot v diplomski nalogi).

### Mape testnih posnetkov

| Mapa | Opis |
|---|---|
| `ars-che/` | Rezultati za testni posnetek Arsenal – Chelsea. |
| `bay-psg-base/` | Rezultati za osnovni testni posnetek Bayern – PSG. |
| `bay-psg-shot/` | Rezultati za izsek posnetka Bayern – PSG s strelom na gol. |
| `lev-dor/` | Rezultati za testni posnetek Leverkusen – Dortmund. |
| `liv-mci-pass/` | Rezultati za izsek posnetka Liverpool – Manchester City s podajo. |
| `liv-mci-shot/` | Rezultati za izsek posnetka Liverpool – Manchester City s strelom na gol. |
| `liv-mci/` | Izvorni, še nerazrezan posnetek tekme Liverpool – Manchester City, iz katerega sta bila izrezana izseka `liv-mci-pass` in `liv-mci-shot`. |

V vsaki od zgornjih map (razen `liv-mci/`, ki služi le kot izvorni posnetek
in ne vsebuje lastne evalvacije) se med drugim nahajajo naslednje
evalvacijske datoteke, katerih rezultati so uporabljeni v diplomski nalogi:

| Datoteka | Opis |
|---|---|
| `tracker_comparison_players.csv` | Rezultati sledenja igralcem (MOTA, MOTP, IDF1, IDSW) za posamezni posnetek, za metode DeepSORT, ByteTRACK, SAM2 (vanilla) in SAM2 (predlagani). |
| `net_idsw_comparison.csv` | Standardno in net IDSW (trajno) število zamenjav identitet igralcev za posamezni posnetek. |
| `ball_tracking_comparison_xml.csv` | Rezultati sledenja žogi (Tracking Rate, CLE, Precision@10/20px, Success AUC) za posamezni posnetek. |
| `team_sorting_evaluation.csv` | Točnost, precision, recall in F1-score razvrščanja igralcev v ekipe za posamezni posnetek. |
| `keypoints_stats.csv` | Statistika napake poravnave po posameznih ključnih točkah igrišča za posamezni posnetek. |
| `reprojection_details.csv` | Surove meritve napake poravnave (razdalja, zamik po X in Y) po vsaki ključni točki in okvirju za posamezni posnetek. |
| `shot-events_evaluation_report.csv` | Rezultati prepoznavanja dogodkov (podaja, strel, obramba vratarja, prestrezanje) za posamezni posnetek. |

### Zbirne mape

Za potrebe diplomske naloge so bili rezultati zgornjih šestih posnetkov
združeni v naslednjih mapah — relativne mere (odstotki, povprečja) so
povprečene, absolutna števila dogodkov pa seštete čez vseh šest posnetkov:

| Mapa | Vsebuje kopije | Zbirna datoteka |
|---|---|---|
| `tracker-comparison/` | `tracker_comparison_players.csv` in `net_idsw_comparison.csv` za vseh šest posnetkov | `tracker_comparison_players_avg.csv`, `net_idsw_comparison_sum.csv` |
| `ball_tracking/` | `ball_tracking_comparison_xml.csv` za vseh šest posnetkov | `ball_tracking_comparison_avg.csv` |
| `team-clustering/` | `team_sorting_evaluation.csv` za vseh šest posnetkov | `team_sorting_evaluation_avg.csv` |
| `keypoints/` | `keypoints_stats.csv` in `reprojection_details.csv` za vseh šest posnetkov | `field_alignment_avg.csv` |
| `events-tracker/` | `shot-events_evaluation_report.csv` za vseh šest posnetkov | `event_detection_sum.csv` |
| `timerun/` | poročila o hitrosti izvajanja za vseh šest posnetkov | `performance_speed_sum.csv` |

Zbirne datoteke (`*_avg.csv`, `*_sum.csv`) vsebujejo končne, agregirane
vrednosti, uporabljene v razdelku Rezultati diplomske naloge.

## Video posnetki

Sistem in evalvacija sta bila izvedena na šestih testnih posnetkih
nogometnih tekem:

- **ars-che** — Arsenal – Chelsea
- **bay-psg-base** — osnovni posnetek Bayern – PSG
- **bay-psg-shot** — izsek posnetka Bayern – PSG s strelom na gol
- **lev-dor** — Leverkusen – Dortmund
- **liv-mci-pass** — izsek posnetka Liverpool – Manchester City s podajo
- **liv-mci-shot** — izsek posnetka Liverpool – Manchester City s strelom na gol

## Uporaba

1. Odpri `sistem_za_analizo.ipynb` v Google Colabu ali lokalnem Jupyter
   okolju in ga poženi nad izbranim posnetkom iz ene od map testnih
   posnetkov, da dobiš napovedane sledi in ostale vmesne rezultate sistema.
2. Odpri `evalvacija.ipynb` in nastavi poti do generiranih napovedi ter
   pripadajočih referenčnih anotacij za isti posnetek.
3. Zaženi evalvacijo — rezultati (MOT metrike za igralce, metrike za
   sledenje žogi, net IDSW analiza, razvrščanje ekip, poravnava igrišča in
   prepoznavanje dogodkov) se izpišejo in shranijo v ustrezno mapo
   testnega posnetka.

Za izračun agregiranih vrednosti čez vseh šest testnih posnetkov (mape v
razdelku Zbirne mape) je bilo treba evalvacijski zvezek pognati ločeno na
vsakem od šestih posnetkov, nato pa dobljene datoteke združiti, kot je
opisano zgoraj.
