# Comparison of Hyperparameter Optimization Methods and SHAP Interpretation of LightGBM for Medication Adherence Prediction in Non-Communicable Disease Patients

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23227735.svg)](https://doi.org/10.5281/zenodo.23227735)

Code, executed notebook, data and results for a master's thesis in Informatics at Universitas Ahmad Dahlan, Indonesia (2026).

**Author:** Ihya' Nashirudin Abrar
**Supervisors:** Dr. Eng. Ir. Muhammad Kunta Biddinika, S.T., M.Eng. · Ir. Herman Yuliansyah, S.T., M.Eng., Ph.D.

*Original thesis title (Indonesian): Perbandingan Metode Optimasi Hiperparameter dan Interpretasi SHAP pada LightGBM untuk Prediksi Kepatuhan Pengobatan Pasien Penyakit Tidak Menular.*

---

## Summary

The study compares **six hyperparameter optimization (HPO) methods** for LightGBM: Grid Search, Random Search, Halving Random Search, Optuna-TPE, Hyperopt-TPE and scikit-optimize-GP. The comparison uses the Friedman test with the Nemenyi post hoc test. The final model is then explained with **four-level SHAP analysis**. The data are 24,084 insurance-claim records of diabetes and hypertension patients from Cimas Medical Aid Society, Zimbabwe.

| Finding | Value |
|---|---|
| Highest mean 10-fold CV accuracy (Random Search) | 0.8241 (range across 7 configurations: 0.8211–0.8241) |
| Friedman test across 7 configurations | χ² = 9.127; p = 0.167 (not significant) |
| Final model on the test set | accuracy 0.8201 · MCC 0.6433 · AUC-ROC 0.9043 · NON-ADHERENT recall 0.7897 |
| Final vs baseline (default configuration) | accuracy difference +0.0002; 95% CI [−0.004, 0.005]; McNemar p = 1.000 |
| Largest SHAP contribution | `UNITSTOTAL` (mean \|SHAP\| 1.3048) |
| Ablation of all claim-unit-derived features | accuracy drops 0.8201 → 0.7580 |

On this dataset, hyperparameter optimization gave **no measurable performance gain** over LightGBM's default configuration. Most of the predictive power comes from claim-unit features that are close to the target definition (refill frequency). The model is therefore best viewed as a **retrospective adherence classifier**, not a prospective early-warning tool.

## Repository layout

```
.
├── data/
│   ├── Final Prepared Dataset - Diabetes and Hypertension Data.xlsx
│   └── README.md               # source, licence and variable definitions
├── notebooks/
│   └── full_experiment.ipynb   # the whole experiment, end to end (executed)
├── figures/                    # all figures produced by the notebook
├── outputs/                    # numerical results (CSV/JSON)
├── requirements.txt
├── CITATION.cff
├── .zenodo.json
└── LICENSE
```

## Notebook structure

| Part | Content | Thesis section |
|---|---|---|
| 1 | Setup and libraries (`random_state = 42`) | 3.2.1 |
| 2 | Data loading and exploration | 3.2.2, 4.1 |
| 3 | Pre-processing: deduplication, encoding, feature engineering (17 features), stratified 80:20 split | 3.3.2, 4.1 |
| 4 | Preliminary study: five boosting algorithms | Appendix 2 |
| 5 | **Stage 1**: 6 HPO methods, 10-fold CV, Friedman + Nemenyi | 3.3.3, 4.2 |
| 6 | Final model training | 4.2 |
| 7 | **Stage 2**: SHAP L1–L4 + direction-of-contribution statistics | 3.3.4, 4.3 |
| 8 | Final evaluation: per-class metrics, confusion matrix, ROC, exploratory Youden threshold | 3.3.5, 4.4 |
| 9 | Sensitivity analysis: class weighting, claim-unit feature ablation, baseline vs final (bootstrap + McNemar) | 4.5 |
| 10 | Summary | Chapter 5 |

## Reproducing the results

Tested with Python 3.11 on Windows 11 (Intel Core i7, 16 GB RAM).

```bash
python -m venv .venv
.venv\Scripts\activate          # Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/full_experiment.ipynb
```

Headless run:

```bash
jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=-1 notebooks/full_experiment.ipynb
```

A full run takes roughly 35–60 minutes on a CPU, mostly in Stage 1. Metrics may shift very slightly with library versions other than those in `requirements.txt`.

## Methodological notes

- **Search spaces are not fully identical.** Random Search and the three Bayesian methods use 9 hyperparameters with 40 trials. Halving uses the same distributions and ends up with 1,444 evaluations on data subsets. Grid Search covers only 5 hyperparameters (162 combinations).
- **`subsample` is inactive** because `subsample_freq = 0` (the LightGBM default), so its value is not interpreted.
- **Run times are indicative only**, because parallelism differs between methods. Times in this notebook come from a re-run and differ from Table 4.1 of the thesis (original run); all performance metrics are identical.
- **This is not full nested cross-validation.** The Friedman–Nemenyi tests on folds of a single dataset are treated as an internal analysis.
- **Class imbalance** (1.49:1) is handled with a stratified split and imbalance-robust metrics (MCC, AUC-ROC, per-class recall), without class weighting or SMOTE. The effect of `class_weight='balanced'` is tested in Part 9.
- **SHAP values are associative**, not causal.

## Data

Kanyongo, W. (2024). *Dataset for Analysing Medication Adherence among Diabetes and Hypertension Patients: Patient-Level and Medication Refill Data* (Version 2) [Data set]. Mendeley Data. https://doi.org/10.17632/zkp7sbbx64.2. Licence: **CC0 1.0**. See [`data/README.md`](data/README.md).

## Citation

If you use this code, please cite the Zenodo archive (see also `CITATION.cff`):

> Abrar, I. N. (2026). *Comparison of Hyperparameter Optimization Methods and SHAP Interpretation of LightGBM for Medication Adherence Prediction in Non-Communicable Disease Patients: Code and Results* [Software]. Zenodo. https://doi.org/10.5281/zenodo.23227735

## Licence

Code is released under the MIT licence (see `LICENSE`). The dataset keeps its original CC0 1.0 licence.
