# Data

**Berkas:** `Final Prepared Dataset - Diabetes and Hypertension Data.xlsx` (24.084 baris × 12 kolom)

**Sumber:** Kanyongo, W. (2024). *Dataset for Analysing Medication Adherence among Diabetes and Hypertension Patients: Patient-Level and Medication Refill Data* (Version 2) [Data set]. Mendeley Data. https://doi.org/10.17632/zkp7sbbx64.2

**Lisensi:** CC0 1.0 (*Public Domain Dedication*). Berkas disertakan apa adanya untuk reprodusibilitas.

Proses *data wrangling* asal dijelaskan dalam:
Kanyongo, W., Ezugwu, A. E. S., Moyo, T., & Dombeu, J. V. F. (2025). Data Wrangling and Generation for Machine Learning Models in Medication Adherence Analytics. *Data Intelligence*, 7(2), 485–526. https://doi.org/10.3724/2096-7004.di.2024.0037

## Variabel

| Variabel | Tipe | Keterangan |
|---|---|---|
| AGE | Numerik | Usia pasien (tahun) |
| ANNUALCONTRIBUTION | Numerik | Kontribusi tahunan ke skema asuransi |
| ANNUALCLAIMAMOUNT | Numerik | Total klaim tahunan |
| UNITSTOTAL | Numerik | Total unit yang diklaim |
| GENDER_M | Biner | 1 = laki-laki |
| SCHEMETYPE_MEDIUM / SCHEMETYPE_PREMIUM | Biner | Tipe skema asuransi |
| DIAGNOSIS_HYPERTENSION | Biner | 1 = hipertensi, 0 = diabetes |
| COVERTYPE_STANDARD | Biner | Tipe pertanggungan Standard |
| COMORBIDITY_NO_COMORBIDITY | Biner | 1 = tanpa komorbiditas |
| COMPLICATIONDEVELOPMENT_NO_COMPLICATION | Biner | 1 = tanpa perkembangan komplikasi |
| **ADHERENCE** | Target | **ADHERENT**: *refill* obat 9–12 kali dalam 12 bulan (≥ 75%); **NON-ADHERENT**: 1–8 kali |

Catatan: label ADHERENCE dan `UNITSTOTAL` sama-sama diturunkan dari data klaim yang sama. Implikasinya diuji melalui analisis ablasi pada Bagian 9 notebook.
