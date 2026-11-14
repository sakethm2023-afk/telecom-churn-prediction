# Telecom Customer Churn Prediction

**DADS 7275 — Machine Learning and Data Analytics**, Northeastern University (Professor Xuemin Jin).
Author: Saketh Mandava.

## Objective
Classify recorded customer churn and rank customers for possible retention outreach using demographics,
services, contract terms, tenure, and billing. The `Churn` field records customers who left within the last
month, so this is a **retrospective study, not a validated forecast**. It makes no claims about advance
prediction, causal effects, or retention impact. Approved plan: `docs/Final_Project_Proposal.pdf`.

## Dataset
Telco Customer Churn CSV (Kaggle, blastchar): 7,043 rows, 21 columns (`customerID`, 19 candidate predictors,
binary `Churn`). Stored unmodified at `data/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv`. The dataset has no
usage, complaint, or timestamp information, which limits generalizability.

## Planned methods
- Majority-class dummy baseline
- Logistic Regression, Decision Tree, Random Forest, Gradient Boosting (scikit-learn)
- Stratified 75/25 train/test split (seed 42); five-fold stratified cross-validation on the training set
- Model selection by mean validation F1 (ROC-AUC tie-break); threshold chosen from out-of-fold training predictions

## Evaluation metrics
Precision, recall, F1, ROC-AUC, confusion matrices; precision among the top 10% and top 20% of customers
ranked by predicted churn probability (compared with the test-set churn rate); repeated permutation importance
(five leading predictors).

## Repository structure
```
telecom-churn-prediction/
├── CLAUDE.md                  development guardrails
├── README.md
├── requirements.txt           exact package versions
├── .gitignore
├── data/raw/                  original dataset (read-only)
├── docs/
│   ├── Final_Project_Proposal.pdf
│   └── requirements_traceability.md
├── notebooks/
│   └── telecom_churn_analysis.ipynb   the single primary analysis notebook
└── results/                   saved tables and figures
```

## Setup (Windows, Python 3.12)
```powershell
cd C:\Users\saket\Documents\GitHub\telecom-churn-prediction
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m ipykernel install --user --name telecom-churn --display-name "Python 3.12 (telecom-churn)"
```
If PowerShell blocks activation, run tools directly, e.g. `.\.venv\Scripts\python.exe -m jupyter lab`.

## Launching Jupyter
```powershell
.\.venv\Scripts\Activate.ps1
jupyter lab
```
Open the notebook in `notebooks/` and select the "Python 3.12 (telecom-churn)" kernel.

## Status and milestones
| Milestone | Scope | Status |
|---|---|---|
| 0A | Discovery, proposal review, planning | Complete |
| 0B | Repository and environment setup, guardrails | Complete |
| 1 | Data integrity checks, stratified split, training-set exploration | Complete |
| 2 | Pipelines, baseline, cross-validation of four models | Complete (pending review) |
| 3 | Model selection and threshold selection | Not started |
| 4 | Final test evaluation, ranking precision, permutation importance | Not started |
| 5 | Notebook finalization and documentation | Not started |

Milestones 1 and 2 are implemented in `notebooks/telecom_churn_analysis.ipynb`, which is the primary source for
detailed findings and metric tables.

**Milestone 1 (Sections 1-10):** data integrity, stratified 75/25 split with seed 42, training-set-only EDA.

**Milestone 2 (Sections 11-19):** leakage-safe preprocessing and baseline comparison.
- One scikit-learn `ColumnTransformer` inside every `Pipeline`: median imputation + standard scaling for
  `tenure`, `MonthlyCharges`, `TotalCharges`; one-hot encoding (unknown-safe, all levels kept) for `SeniorCitizen`
  and the 15 categorical columns; `customerID` excluded.
- Five models, untuned initial settings: majority-class Dummy, Logistic Regression, Decision Tree, Random Forest,
  Gradient Boosting (`HistGradientBoostingClassifier`).
- Evaluation: `StratifiedKFold(5, shuffle=True, random_state=42)` on the 75% training set only, with identical folds
  for every model; the pipeline is re-fitted inside each fold. Churn-class precision, recall, F1, ROC-AUC (from
  probabilities) and accuracy at the default 0.5 threshold; ranked by mean F1, ROC-AUC as tie-break.
- Results are training-set cross-validation results, not test performance. The test set is untouched, and no tuning
  or threshold optimization has been done (Milestone 3).

## Running the notebook
The notebook needs only `data/raw/WA_Fn-UseC_-Telco-Customer-Churn.csv` (unmodified) and the pinned environment.
It locates the project root automatically, so it can be launched from the project root or from `notebooks/`.
```powershell
.\.venv\Scripts\python.exe -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.kernel_name=telecom-churn notebooks/telecom_churn_analysis.ipynb
```
Or open it in `jupyter lab` and choose Run All with the "Python 3.12 (telecom-churn)" kernel.
