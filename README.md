# Classical Machine Learning for Land-Clearing Detection in the Merauke PSN Area (T01)

[![DOI](https://zenodo.org/badge/1462952398.svg)](https://doi.org/10.5281/zenodo.24346282)

Code, data and results for the study *"Comparing Classical Machine Learning Algorithms and Sentinel-2 Feature Configurations for Land-Clearing Detection in the Merauke National Strategic Project Area, South Papua, Indonesia"*.

> **Ringkasan (ID):** Repositori ini berisi notebook, dataset sampel, dan tabel hasil untuk perbandingan Random Forest, SVM, Gradient Boosting, dan KNN pada lima konfigurasi fitur Sentinel-2 untuk deteksi pembukaan lahan di Blok A kawasan PSN Merauke (2023 vs 2025), dengan data acuan tanpa survei lapangan dan validasi silang spasial.

## Study design

| Item | Value |
|---|---|
| Study area | Block A, 20 × 20 km, centre 140.546° E, 8.232° S (Merauke Regency) |
| Periods | 2023 (pre) vs 2025 (post) |
| Classes | Cleared land, stable forest, stable non-forest |
| Reference data | Consensus of Hansen GFC v1.13, JRC TMF 2025, RADD (no Sentinel-2, no field survey) |
| Features | 5 sets (FS1–FS5), up to 32 Sentinel-2 features |
| Algorithms | RF, SVM, GBT, KNN (default and tuned) |
| Validation | 70/30 spatial split + GroupKFold (k = 5) on 2 km cells |
| Sample | 2,100 points (700 per class; 1,503 train / 597 test) |

## Main results

| | Value |
|---|---|
| Best score | GBT + FS5: OA 0.814, Kappa 0.720, macro-F1 0.821 |
| Algorithm differences | Not significant (McNemar, all p > 0.05) |
| Multi-temporal gain | Mean macro-F1 0.744 → 0.812 |
| Cleared land (best model) | Precision 0.928, recall 0.971, F1 0.949 |
| Cleared area in Block A | 7,835.2 ha (Hansen 2024–2025 loss: 7,935.9 ha) |

## Repository structure

```
notebooks/T01_notebook.ipynb   full pipeline (Google Colab + Earth Engine + scikit-learn)
data/                          Block A boundary and 2,100 labelled sample points (CSV, GeoJSON)
results/tables/                all result tables (CSV, XLSX) and T01_hasil_ringkas.json
results/figures/               figures used in the manuscript
docs/diagrams/                 flowchart, context diagram, level-1 DFD (ID & EN; PNG, SVG)
```

## Reproduce

1. Open `notebooks/T01_notebook.ipynb` in [Google Colab](https://colab.research.google.com).
2. Set `GEE_PROJECT` to an Earth Engine Cloud project registered for noncommercial use.
3. *Runtime → Run all* (about 40–60 minutes on a free Colab runtime).

`SEED = 42` gives the same sample as long as the Earth Engine dataset versions are unchanged.

## Data sources

All inputs are public and accessed through Google Earth Engine: Sentinel-2 SR Harmonized (Copernicus), Cloud Score+ (Google), Hansen Global Forest Change v1.13 (UMD), JRC Tropical Moist Forest 2025 (European Commission JRC), RADD alerts (Wageningen University), JRC Global Surface Water v1.4 and FAO GAUL 2015. Please cite the original providers when reusing the data.

## License

- Code (`notebooks/`): MIT License, see `LICENSE`.
- Data, results and figures (`data/`, `results/`, `docs/`): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

## Citation

See `CITATION.cff`. After the article is published, please cite the article as well.
