# Habitate_1
Project_Her2_low breast cancer
# MRI Habitat Radiomics for Prognosis Prediction in HER2-Low Breast Cancer

Code accompanying the manuscript: *"MRI Habitat Radiomics with Clinical Features for Prognosis
Prediction in HER2-Low Breast Cancer: A Multicenter Study."*

This repository contains the analysis pipeline for a multi-center study (401 patients, 4 centers)
that clusters DWI and DCE breast MRI into intratumoral **habitats** (K-means, K = 3), extracts
PyRadiomics features per habitat, and builds LASSO-Cox prognostic models of disease-free survival
(DFS) combined with clinical variables. Model performance is estimated by **repeated nested 5-fold
cross-validation** and **leave-one-center-out (LOCO)** validation, with bootstrap optimism, paired
discrimination comparisons, reclassification (NRI/IDI), calibration, IPCW decision-curve analysis,
and inter-observer reproducibility (ICC).

## Data availability
**Patient data (MRI images, radiomic feature tables, clinical data) are NOT included** in this
repository because they contain protected health information and are subject to institutional
ethics/IRB restrictions. The code expects a local data root; see *Configuration* below.
Case identifiers referenced in scripts (e.g., `Cey02`) are **de-identified codes**, not real names.

## Repository structure
```
scripts/                 All Python scripts (habitat clustering, radiomics, modeling, statistics, figures)
requirements.txt         Python dependencies
.gitignore               Excludes data, results, and generated outputs
```
`scripts/` contains the core analysis pipeline (36 scripts), grouped by stage: clustering -> radiomics extraction -> spatial features -> univariate/modeling -> nested CV + LOCO validation -> ICC -> IPCW DCA -> figures & tables. Key entry points:
- `1_habitat_clustering.py`, `3_dce_habitat_clustering.py` — DWI / DCE habitat clustering
- `2e_dce_pyradiomics.py` and related — PyRadiomics feature extraction per habitat
- `23b_spatial_incremental_modeling.py`, `40_final_4center_consolidated.py` — model development
- `200_loco_validation.py` — leave-one-center-out validation
- `201/202_revision_analyses*.py` — nested CV, paired bootstrap, NRI/IDI, calibration, K-sensitivity
- `220_icc.py` — inter-observer ICC
- `233_dca_ipcw.py` — IPCW-weighted decision-curve analysis

## Environment
Two environments were used:
- **PyRadiomics / SimpleITK** (e.g., Anaconda) for habitat clustering and feature extraction.
- **Python 3.11** with scikit-survival, lifelines, python-docx for modeling and reporting.

Install dependencies:
```bash
pip install -r requirements.txt
```

## Configuration (important)
Absolute local paths have been replaced with placeholders:
- `<PROJECT_ROOT>` — project root (data/features, results/…)
- `<DATA_ROOT>` — location of the merged clinical table (`merged_data_corrected.csv`)

Replace these placeholders (or define environment variables) with your local paths before running.

## Statistical methods (summary)
- Habitat clustering: StandardScaler -> (PCA) -> K-means (K = 3), relabelled L/M/H by mean signal.
- Feature selection & modeling: LASSO-Cox; tuning within inner CV folds only.
- Validation: repeated nested 5-fold CV (20 partitions) + leave-one-center-out.
- Metrics: Harrell C-index, time-dependent AUC (3/5-yr), Brier score (IPCW), log-rank, NRI, IDI,
  calibration slope/intercept, decision-curve analysis.

## License
No license is specified here; contact the corresponding author for reuse terms.

## Disclaimer
This code is provided for research reproducibility. It is not intended for clinical use.
