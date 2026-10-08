# Data

**File:** `Final Prepared Dataset - Diabetes and Hypertension Data.xlsx` (24,084 rows × 12 columns)

**Source:** Kanyongo, W. (2024). *Dataset for Analysing Medication Adherence among Diabetes and Hypertension Patients: Patient-Level and Medication Refill Data* (Version 2) [Data set]. Mendeley Data. https://doi.org/10.17632/zkp7sbbx64.2

**Licence:** CC0 1.0 (Public Domain Dedication). The file is included unchanged for reproducibility.

The original data-wrangling process is described in:
Kanyongo, W., Ezugwu, A. E. S., Moyo, T., & Dombeu, J. V. F. (2025). Data Wrangling and Generation for Machine Learning Models in Medication Adherence Analytics. *Data Intelligence*, 7(2), 485–526. https://doi.org/10.3724/2096-7004.di.2024.0037

## Variables

| Variable | Type | Description |
|---|---|---|
| AGE | Numeric | Patient age (years) |
| ANNUALCONTRIBUTION | Numeric | Annual contribution to the insurance scheme |
| ANNUALCLAIMAMOUNT | Numeric | Total annual claim amount |
| UNITSTOTAL | Numeric | Total claimed units |
| GENDER_M | Binary | 1 = male |
| SCHEMETYPE_MEDIUM / SCHEMETYPE_PREMIUM | Binary | Insurance scheme type |
| DIAGNOSIS_HYPERTENSION | Binary | 1 = hypertension, 0 = diabetes |
| COVERTYPE_STANDARD | Binary | Standard cover type |
| COMORBIDITY_NO_COMORBIDITY | Binary | 1 = no comorbidity |
| COMPLICATIONDEVELOPMENT_NO_COMPLICATION | Binary | 1 = no complications developed |
| **ADHERENCE** | Target | **ADHERENT**: 9–12 medication refills in 12 months (≥ 75%); **NON-ADHERENT**: 1–8 refills |

Note: the ADHERENCE label and `UNITSTOTAL` are both derived from the same claims data. The implications are tested with an ablation analysis in Part 9 of the notebook.
