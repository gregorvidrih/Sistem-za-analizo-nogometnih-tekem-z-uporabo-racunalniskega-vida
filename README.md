# Football Match Analysis System Using Computer Vision

Bachelor's thesis, published in 2026.

This repository contains the complete computer-vision system for football match analysis: the main notebook with the system and the evaluation notebook, together with tracking and evaluation results from the six test videos used to quantitatively evaluate the system in the bachelor's thesis.

![Product](Sistem-za-analizo-nogometnih-tekem-z-uporabo-racunalniskega-vida/Product.gif)


## Repository Contents

| File / folder | Description |
|---|---|
| `sistem_za_analizo.ipynb` | Main Jupyter/Colab notebook containing the complete system: object detection (RF-DETR), player tracking (SAM2) and ball tracking (Kalman filter), team identification (SigLIP + UMAP + KMeans), pitch keypoint detection, homography-based projection, and event recognition. |
| `evalvacija.ipynb` | Evaluation notebook that compares system predictions with manually annotated ground truth and calculates standard MOT metrics (MOTA, MOTP, IDF1, IDSW, net IDSW) and small-object tracking metrics (Tracking Rate, CLE, Precision@k, Success AUC). |

The rest of the repository is organized into two types of folders: **individual test-clip folders** (results and evaluation for one video) and **aggregate folders** (results combined across all six clips and used to calculate averages or totals in the bachelor's thesis).

### Test-Clip Folders

| Folder | Description |
|---|---|
| `ars-che/` | Results for the Arsenal–Chelsea test clip. |
| `bay-psg-base/` | Results for the baseline Bayern–PSG test clip. |
| `bay-psg-shot/` | Results for the Bayern–PSG clip segment containing a shot on goal. |
| `lev-dor/` | Results for the Bayer Leverkusen–Borussia Dortmund test clip. |
| `liv-mci-pass/` | Results for the Liverpool–Manchester City clip segment containing a pass. |
| `liv-mci-shot/` | Results for the Liverpool–Manchester City clip segment containing a shot on goal. |
| `liv-mci/` | The original, uncut Liverpool–Manchester City match video from which the `liv-mci-pass` and `liv-mci-shot` segments were extracted. |

Each folder above, except `liv-mci/` (which serves only as the source video and has no separate evaluation), includes the following evaluation files, among others. Their results are used in the bachelor's thesis:

| File | Description |
|---|---|
| `tracker_comparison_players.csv` | Player-tracking results (MOTA, MOTP, IDF1, IDSW) for each clip, comparing DeepSORT, ByteTrack, SAM2 (vanilla), and SAM2 (proposed). |
| `net_idsw_comparison.csv` | Standard and net (persistent) player identity-switch counts for each clip. |
| `ball_tracking_comparison_xml.csv` | Ball-tracking results (Tracking Rate, CLE, Precision@10/20px, Success AUC) for each clip. |
| `team_sorting_evaluation.csv` | Accuracy, precision, recall, and F1-score for assigning players to teams in each clip. |
| `keypoints_stats.csv` | Alignment-error statistics for each pitch keypoint in each clip. |
| `reprojection_details.csv` | Raw alignment-error measurements (distance and X/Y offsets) for each keypoint and frame in each clip. |
| `shot-events_evaluation_report.csv` | Event-recognition results (pass, shot, goalkeeper save, interception) for each clip. |

### Aggregate Folders

For the bachelor's thesis, results from the six clips above were combined in the following folders. Relative metrics (percentages and averages) were averaged, while absolute event counts were summed across all six clips:

| Folder | Contains copies of | Aggregate file |
|---|---|---|
| `tracker-comparison/` | `tracker_comparison_players.csv` and `net_idsw_comparison.csv` for all six clips | `tracker_comparison_players_avg.csv`, `net_idsw_comparison_sum.csv` |
| `ball_tracking/` | `ball_tracking_comparison_xml.csv` for all six clips | `ball_tracking_comparison_avg.csv` |
| `team-clustering/` | `team_sorting_evaluation.csv` for all six clips | `team_sorting_evaluation_avg.csv` |
| `keypoints/` | `keypoints_stats.csv` and `reprojection_details.csv` for all six clips | `field_alignment_avg.csv` |
| `events-tracker/` | `shot-events_evaluation_report.csv` for all six clips | `event_detection_sum.csv` |
| `timerun/` | Runtime reports for all six clips | `performance_speed_sum.csv` |

The aggregate files (`*_avg.csv`, `*_sum.csv`) contain the final aggregated values used in the Results section of the bachelor's thesis.

## Videos

The system and evaluation were run on six test clips from football matches obtained from YouTube:

| Folder | Match / clip | Source video |
|---|---|---|
| `ars-che` | Arsenal–Chelsea | [YouTube](https://www.youtube.com/watch?v=iQ7Em5lzEKo) |
| `bay-psg-base` | Bayern–PSG (baseline clip) | [YouTube](https://www.youtube.com/watch?v=fg3DmFxMWZ4) |
| `bay-psg-shot` | Bayern–PSG (shot-on-goal segment) | [YouTube](https://www.youtube.com/watch?v=fg3DmFxMWZ4) |
| `lev-dor` | Bayer Leverkusen–Borussia Dortmund | [YouTube](https://www.youtube.com/watch?v=6-AQE74o460) |
| `liv-mci-pass` | Liverpool–Manchester City (pass segment) | [YouTube](https://www.youtube.com/watch?v=XckOkRuE3-4) |
| `liv-mci-shot` | Liverpool–Manchester City (shot-on-goal segment) | [YouTube](https://www.youtube.com/watch?v=XckOkRuE3-4) |

> The videos remain the property of their original creators and rights holders (football leagues and broadcasters). They were used in this repository and the bachelor's thesis solely for research and educational purposes.

## Usage

1. Open `sistem_za_analizo.ipynb` in Google Colab or a local Jupyter environment and run it on a selected video from one of the test-clip folders to generate predicted tracks and other intermediate system results.
2. Open `evalvacija.ipynb` and set the paths to the generated predictions and the corresponding ground-truth annotations for the same clip.
3. Run the evaluation. The results (player MOT metrics, ball-tracking metrics, net IDSW analysis, team assignment, pitch alignment, and event recognition) are printed and saved in the corresponding test-clip folder.

To calculate aggregate values across all six test clips (the folders listed in the Aggregate Folders section), the evaluation notebook had to be run separately on each clip. The resulting files were then combined as described above.

## License

The source code for this project is released under the MIT License. The full license text is available in [`LICENSE`](LICENSE).

## Citation

If you use code or data from this repository, please cite the following contribution:

> G. Vidrih and B. Batagelj, “Sistem za analizo nogometnih tekem z računalniškim vidom” [A Computer Vision System for Football Match Analysis], ERK 2026.

BibTeX citation:

```bibtex
@inproceedings{vidrih2026football,
  author    = {Vidrih, Gregor and Batagelj, Borut},
  title     = {Sistem za analizo nogometnih tekem z računalniškim vidom},
  booktitle = {ERK 2026},
  year      = {2026}
}
```

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
nogometnih tekem, pridobljenih z YouTuba:

| Mapa | Tekma | Vir posnetka |
|---|---|---|
| `ars-che` | Arsenal – Chelsea | [YouTube](https://www.youtube.com/watch?v=iQ7Em5lzEKo) |
| `bay-psg-base` | Bayern – PSG (osnovni posnetek) | [YouTube](https://www.youtube.com/watch?v=fg3DmFxMWZ4) |
| `bay-psg-shot` | Bayern – PSG (izsek s strelom na gol) | [YouTube](https://www.youtube.com/watch?v=fg3DmFxMWZ4) |
| `lev-dor` | Leverkusen – Dortmund | [YouTube](https://www.youtube.com/watch?v=6-AQE74o460) |
| `liv-mci-pass` | Liverpool – Manchester City (izsek s podajo) | [YouTube](https://www.youtube.com/watch?v=XckOkRuE3-4) |
| `liv-mci-shot` | Liverpool – Manchester City (izsek s strelom na gol) | [YouTube](https://www.youtube.com/watch?v=XckOkRuE3-4) |

> Video posnetki so last njihovih izvornih avtorjev/imetnikov pravic
> (nogometne lige, TV hiše) in so bili v tem repozitoriju oziroma diplomski
> nalogi uporabljeni izključno v raziskovalne in izobraževalne namene.

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

## Licenca

Izvorna koda tega projekta je objavljena pod licenco MIT.
Celotno besedilo licence je na voljo v datoteki [LICENSE](LICENSE).

## Citiranje

Če uporabite izvorno kodo ali podatke iz tega repozitorija, je obvezno citirati naslednji prispevek:

> G. Vidrih in B. Batagelj, »Sistem za analizo nogometnih tekem z računalniškim vidom«, ERK 2026.

Za lažje citiranje:

```bibtex
@inproceedings{vidrih2026football,
  author    = {Vidrih, Gregor and Batagelj, Borut},
  title     = {Sistem za analizo nogometnih tekem z računalniškim vidom},
  booktitle = {ERK 2026},
  year      = {2026}
}
```
