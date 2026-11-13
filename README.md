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
├── notebooks/                 the single primary analysis notebook (to be created)
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
| 0B | Repository and environment setup, guardrails | Complete (pending review) |
| 1 | Data integrity checks, stratified split, training-set exploration | Not started |
| 2 | Pipelines, baseline, cross-validation of four models | Not started |
| 3 | Model selection and threshold selection | Not started |
| 4 | Final test evaluation, ranking precision, permutation importance | Not started |
| 5 | Notebook finalization and documentation | Not started |

No modeling or evaluation has been performed yet.
