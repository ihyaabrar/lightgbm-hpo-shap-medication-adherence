# Perbandingan Metode Optimasi Hiperparameter dan Interpretasi SHAP pada LightGBM untuk Prediksi Kepatuhan Pengobatan Pasien PTM

Kode dan hasil eksperimen tesis Magister Informatika, Universitas Ahmad Dahlan (2026).

**Penulis:** Ihya' Nashirudin Abrar
**Pembimbing:** Dr. Eng. Ir. Muhammad Kunta Biddinika, S.T., M.Eng. · Ir. Herman Yuliansyah, S.T., M.Eng., Ph.D.

*English title: Comparison of Hyperparameter Optimization Methods and SHAP Interpretation of LightGBM for Medication Adherence Prediction in Non-Communicable Disease Patients.*

---

## Ringkasan

Penelitian ini membandingkan **enam metode optimasi hiperparameter** pada LightGBM (Grid Search, Random Search, Halving Random Search, Optuna-TPE, Hyperopt-TPE, dan scikit-optimize-GP). Perbandingan disertai uji Friedman dan *post hoc* Nemenyi, lalu model final ditafsirkan dengan **SHAP empat level**. Data yang dipakai adalah 24.084 rekaman klaim asuransi pasien diabetes dan hipertensi dari Cimas Medical Aid Society, Zimbabwe.

| Temuan | Nilai |
|---|---|
| Akurasi 10-fold CV tertinggi (Random Search) | 0,8241 (rentang 7 konfigurasi: 0,8211–0,8241) |
| Uji Friedman antar 7 konfigurasi | χ² = 9,127; p = 0,167 (tidak signifikan) |
| Model final pada data *testing* | akurasi 0,8201 · MCC 0,6433 · AUC-ROC 0,9043 · *recall* NON-ADHERENT 0,7897 |
| Final vs *baseline* (konfigurasi bawaan) | selisih akurasi +0,0002; IK 95% [−0,004; 0,005]; McNemar p = 1,000 |
| Fitur dengan kontribusi SHAP terbesar | `UNITSTOTAL` (mean \|SHAP\| 1,3048) |
| Ablasi seluruh fitur turunan unit klaim | akurasi turun 0,8201 → 0,7580 |

Pada dataset ini, optimasi hiperparameter **tidak memberikan peningkatan kinerja yang terukur** dibandingkan konfigurasi bawaan LightGBM. Sebagian besar daya prediksi berasal dari fitur berbasis unit klaim yang dekat dengan definisi target (frekuensi *refill*), sehingga model lebih tepat dipandang sebagai alat **klasifikasi kepatuhan secara retrospektif**.

## Struktur repositori

```
.
├── data/
│   ├── Final Prepared Dataset - Diabetes and Hypertension Data.xlsx
│   └── README.md                 # sumber, lisensi, dan definisi variabel
├── notebooks/
│   └── eksperimen_lengkap.ipynb  # seluruh eksperimen dari awal sampai akhir (sudah dieksekusi)
├── figures/                      # semua gambar yang dihasilkan notebook
├── outputs/                      # hasil numerik (CSV/JSON)
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

## Alur notebook

| Bagian | Isi | Bab/subbab tesis |
|---|---|---|
| 1 | Setup dan pustaka (`random_state = 42`) | 3.2.1 |
| 2 | Pemuatan data dan eksplorasi | 3.2.2, 4.1 |
| 3 | Pra-pemrosesan: deduplikasi, *encoding*, *feature engineering* (17 fitur), *stratified split* 80:20 | 3.3.2, 4.1 |
| 4 | Studi pendahuluan: lima algoritma *boosting* | Lampiran 2 |
| 5 | **Tahap 1**: 6 metode HPO, 10-fold CV, Friedman + Nemenyi | 3.3.3, 4.2 |
| 6 | Pelatihan model final | 4.2 |
| 7 | **Tahap 2**: SHAP L1–L4 + statistik arah kontribusi | 3.3.4, 4.3 |
| 8 | Evaluasi final: metrik per kelas, *confusion matrix*, ROC, Youden (eksploratif) | 3.3.5, 4.4 |
| 9 | Analisis sensitivitas: pembobotan kelas, ablasi fitur unit, *baseline* vs final (*bootstrap* + McNemar) | 4.5 |
| 10 | Ringkasan | Bab 5 |

## Menjalankan ulang

Diuji dengan Python 3.11 di Windows 11 (Intel Core i7, RAM 16 GB).

```bash
python -m venv .venv
.venv\Scripts\activate          # Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/eksperimen_lengkap.ipynb
```

Eksekusi penuh tanpa membuka antarmuka:

```bash
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=-1 notebooks/eksperimen_lengkap.ipynb
```

Waktu eksekusi penuh sekitar 35–45 menit di CPU; sebagian besar dipakai oleh Tahap 1. Hasil metrik dapat bergeser sangat sedikit jika versi pustaka berbeda dari `requirements.txt`.

## Catatan metodologis

- **Ruang pencarian tidak sepenuhnya identik.** Random Search dan ketiga metode Bayesian memakai 9 hiperparameter dengan 40 *trial*. Halving memakai distribusi yang sama dan menghasilkan 1.444 evaluasi pada subset data. Grid Search hanya mencakup 5 hiperparameter (162 kombinasi).
- **`subsample` tidak aktif** karena `subsample_freq = 0` (nilai bawaan LightGBM), sehingga nilainya tidak ditafsirkan.
- **Waktu komputasi hanya indikatif**, karena paralelisasi antarmetode tidak seragam. Waktu di notebook ini berasal dari eksekusi ulang dan berbeda dari angka di Tabel 4.1 tesis (eksekusi awal), sedangkan seluruh metrik kinerja identik.
- **Bukan *nested cross-validation* penuh.** Uji Friedman–Nemenyi pada fold dari satu dataset diperlakukan sebagai analisis internal.
- **Ketidakseimbangan kelas** (1,49:1) ditangani dengan *stratified split* dan metrik yang robust terhadap ketidakseimbangan (MCC, AUC-ROC, *recall* per kelas), tanpa pembobotan kelas maupun SMOTE. Pengaruh `class_weight='balanced'` diuji pada Bagian 9.
- **SHAP bersifat asosiatif**, bukan kausal.

## Data

Kanyongo, W. (2024). *Dataset for Analysing Medication Adherence among Diabetes and Hypertension Patients: Patient-Level and Medication Refill Data* (Version 2) [Data set]. Mendeley Data. https://doi.org/10.17632/zkp7sbbx64.2 — lisensi **CC0 1.0**. Rincian ada di [`data/README.md`](data/README.md).

## Sitasi

Jika memakai kode ini, silakan sitasi tesis berikut (lihat juga `CITATION.cff`):

> Abrar, I. N. (2026). *Perbandingan Metode Optimasi Hiperparameter dan Interpretasi SHAP pada LightGBM untuk Prediksi Kepatuhan Pengobatan Pasien Penyakit Tidak Menular*. Tesis, Magister Informatika, Universitas Ahmad Dahlan, Yogyakarta.

## Lisensi

Kode dirilis dengan lisensi MIT (lihat `LICENSE`). Dataset mengikuti lisensi aslinya (CC0 1.0).
