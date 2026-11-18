# Dataset T01 — Deteksi Pembukaan Lahan, Blok A PSN Merauke

Folder ini diisi otomatis oleh `06_sistem/T01_notebook.ipynb` (Tahap 1 dan 4). Claude tidak dapat mengunduh data satelit, jadi dataset **dibangun saat notebook dijalankan** di Google Colab dengan akun Earth Engine Anda.

## Berkas yang akan muncul

| Berkas | Isi |
|---|---|
| `T01_blokA.geojson` | Batas Blok A (sel 20 × 20 km dengan kehilangan tutupan pohon 2024–2025 terbesar di Kab. Merauke) |
| `T01_sampel.csv` | Titik sampel berstrata: label, 32 fitur Sentinel-2, koordinat, grup spasial |
| `T01_sampel_titik.geojson` | Titik sampel yang sama dalam format GeoJSON (bisa dibuka di QGIS) |

## Kamus data `T01_sampel.csv`

| Kolom | Keterangan |
|---|---|
| `label` | 1 = lahan dibuka, 2 = hutan stabil, 3 = non-hutan stabil |
| `kelas` | Nama kelas |
| `B2_post` … `B12_post` | Reflektansi permukaan Sentinel-2 median 2025 (0–1) |
| `NDVI_post`, `EVI_post`, `NDWI_post`, `NDMI_post`, `NBR_post`, `BSI_post` | Indeks spektral 2025 |
| `NDVI_pre` … `BSI_pre` | Indeks spektral 2023 |
| `dNDVI` … `dBSI` | Selisih indeks (2025 − 2023) |
| `GLCM_contrast`, `GLCM_entropy`, `GLCM_savg`, `GLCM_var` | Tekstur GLCM band B8 (jendela 3 × 3) |
| `lon`, `lat` | Koordinat WGS84 |
| `grup_spasial` | ID sel 2 km untuk validasi silang spasial |

## Sumber data (semua gratis)

| Data | ID Google Earth Engine | Peran |
|---|---|---|
| Sentinel-2 SR Harmonized | `COPERNICUS/S2_SR_HARMONIZED` | Fitur |
| Cloud Score+ | `GOOGLE/CLOUD_SCORE_PLUS/V1/S2_HARMONIZED` | Masking awan |
| Hansen Global Forest Change v1.13 | `UMD/hansen/global_forest_change_2025_v1_13` | Label & pemilihan blok |
| JRC Tropical Moist Forest 2025 | `projects/JRC/TMF/v1_2025/AnnualChanges` | Label |
| RADD alerts | `projects/radar-wur/raddalert/v1` | Label |
| FAO GAUL 2015 level 2 | `FAO/GAUL/2015/level2` | Batas Kabupaten Merauke |

## Catatan metodologis

- Label **tidak** memakai Sentinel-2 agar tidak bocor ke fitur.
- Aturan final: lahan dibuka = Hansen lossyear 2024/2025 dan (RADD 2024–2025 atau TMF Des-2025 = deforestasi/degradasi); hutan stabil = tutupan pohon 2000 ≥ 50%, tanpa loss, tanpa RADD; non-hutan stabil = tutupan pohon < 10%, tanpa loss, tanpa RADD, bukan air (JRC GSW < 50%).
- Hansen v1.13 menyimpan piksel tanpa loss sebagai *masked*, sehingga notebook memakai `unmask(0)`.
- Label konsensus adalah data acuan sekunder, bukan survei lapangan; ini dinyatakan sebagai batasan di naskah.
- Pembukaan lahan non-hutan (savana) yang tidak terdeteksi Hansen tidak masuk kelas 1. Topik ini dibahas terpisah di T04.
- Dengan `SEED = 42`, notebook menghasilkan sampel yang sama setiap kali dijalankan (selama versi dataset GEE tidak berubah).
