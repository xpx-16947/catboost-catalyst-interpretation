# Reproducible machine-learning analysis of CO₂ methanation catalysts

This repository contains the curated database, trained CatBoost model, precomputed SHAP arrays, and Jupyter notebooks used for the machine-learning analysis and manuscript figures. All notebooks are provided without execution outputs.

## Repository contents

```text
.
├── database.xlsx
├── catboost_nce.cbm
├── shap_values.npy
├── shap_interaction_values.npy
├── requirements.txt
└── notebooks/
    ├── 01_EDA_and_data_preparation.ipynb
    ├── 02_Figure1_dataset_overview.ipynb
    ├── 03_Six_model_cross_validation.ipynb
    ├── 04_Grid_search.ipynb
    ├── 05_Bayesian_optimization.ipynb
    └── 06_Manuscript_figures.ipynb
```

`database.xlsx` contains 4,026 records and 28 columns. The modeling workflow uses 18 predictors, including 13 numerical variables and five categorical variables: active metal `AM`, promoter `Pr`, primary support `Sp`, secondary support `Sp2`, and preparation method `PM`. The prediction target is CO₂ conversion, stored as `Rate`. The corrected metal-to-support proportion column is named `MSP` throughout the public files.

## Installation

Python 3.11 is recommended.

```bash
python -m venv .venv
source .venv/bin/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
jupyter lab
```

## Notebook order

1. `01_EDA_and_data_preparation.ipynb` checks the database schema, missing values, descriptive statistics, and category frequencies.
2. `02_Figure1_dataset_overview.ipynb` generates the dataset composition, numerical distributions, and correlation analyses used in Figure 1.
3. `03_Six_model_cross_validation.ipynb` compares RF, ET, LightGBM, XGBoost, CatBoost with one-hot encoding, and CatBoost with native categorical encoding using a common train-test split and shuffled five-fold cross-validation.
4. `04_Grid_search.ipynb` performs model-specific grid searches. This notebook is computationally intensive.
5. `05_Bayesian_optimization.ipynb` performs Bayesian optimization of the CatBoost model. This notebook is computationally intensive.
6. `06_Manuscript_figures.ipynb` loads the supplied model and SHAP arrays and regenerates the interpretation and counterfactual-analysis figures.

The notebooks automatically locate the project root by searching for `database.xlsx`. Generated files are written to subfolders under `outputs/`, which is excluded from version control.

## CPU and GPU execution

The CatBoost training notebooks retain GPU execution as the default to match the supplied workflow. Set the environment variable below to run them on CPU:

```bash
# Linux or macOS
export CATBOOST_TASK_TYPE=CPU

# Windows PowerShell
$env:CATBOOST_TASK_TYPE='CPU'
```

CPU and GPU backends, package versions, thread scheduling, and stochastic training can produce small numerical differences. Random seeds used by each analysis are defined near the beginning of the corresponding notebook.

## Reproducibility notes

- Keep the row order of `database.xlsx` unchanged when using the supplied SHAP arrays. Both arrays correspond directly to the 4,026 database rows.
- `catboost_nce.cbm`, `shap_values.npy`, and `shap_interaction_values.npy` are required by the manuscript-figure notebook.
- Grid search and Bayesian optimization can require substantial computation. They are intentionally distributed without cached results.
- The repository contains no notebook outputs, temporary checkpoints, generated figures, or optimizer cache files.
