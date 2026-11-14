# Requirements Traceability

Source: `docs/Final_Project_Proposal.pdf` (page references are to the proposal PDF). Guardrails: `CLAUDE.md`.

**Milestones:** 0B setup · 1 data integrity, split, training-set EDA · 2 pipelines, baseline, 5-fold CV ·
3 model and threshold selection · 4 final test evaluation, ranking, importance · 5 notebook finalization.

| ID | Requirement | Proposal ref | Milestone | Expected deliverable | Verification method | Status |
|---|---|---|---|---|---|---|
| R1 | Use the Kaggle Telco CSV (7,043 rows, 21 columns) as the sole analytical dataset; do not combine with IBM data | p.2 Dataset | 1 | Notebook loads the raw CSV unmodified | Shape check; raw file hash unchanged | Complete (Milestone 1: loads as 7,043 x 21; SHA-256 recorded and re-checked in notebook) |
| R2 | Check duplicates, missing values, class proportions | p.2 Dataset | 1 | Data-integrity section | Printed counts in executed cells | Complete (Milestone 1: 11 blank `TotalCharges`, 22 near-duplicates, 0 other missing, 26.54% churn) |
| R3 | Exclude `customerID`; convert blank `TotalCharges` to missing | p.3 Methodology | 1 | Preprocessing code and documented handling | Assertions on columns and NaN count | Complete (Milestone 1: blanks -> NaN verified; Milestone 2: `customerID` excluded from `X_train` and asserted absent from every fitted pipeline's features; 8 training blanks imputed inside folds) |
| R4 | Review predictors for outcome leakage | p.3 | 1 | Feature-review notes | Written justification per questionable feature | Complete (Milestone 1: feature-review table in notebook; no leakage found, redundancies noted) |
| R5 | Stratified 25% test set, fixed seed (42), created once | p.3 | 1 | Single split cell | Class ratio and sizes in train/test; seed recorded | Complete (Milestone 1: 5,282 / 1,761; 26.54% / 26.52% churn; no overlap; deterministic) |
| R6 | Explore distributions/relationships with churn on training data only | p.3 | 1 | Training-set EDA section and figures | No test rows used in EDA code | Complete (Milestone 1: EDA uses `train_df` only; `test_df` referenced only in split cells) |
| R7 | Pipelines fit imputation, one-hot encoding, scaling (logistic regression) within each training fold | p.3 | 2 | `Pipeline`/`ColumnTransformer` objects | Code review; transformers only inside pipelines | Complete (Milestone 2: one `ColumnTransformer` (median impute + scale, one-hot) inside each `Pipeline`; per-fold imputer/scaler statistics asserted equal to the fold's training rows) |
| R8 | Majority-class dummy baseline | p.1, p.3 | 2 | Baseline CV and test metrics | Baseline row in results table | Partial (Milestone 2: baseline CV metrics done, F1 0, ROC-AUC 0.50, accuracy 0.735; test metrics pending Milestone 4) |
| R9 | Compare Logistic Regression, Decision Tree, Random Forest, Gradient Boosting on same splits | p.1 Obj. 2 | 2 | Comparison table | All four models share identical folds | Partial (Milestone 2: untuned CV comparison of all four on identical folds, ranked, provisional leader Logistic Regression; tuned comparison and selection in Milestone 3) |
| R10 | Five-fold stratified CV on the 75% training data | p.3 | 2 | CV results with mean/std F1 and ROC-AUC | `StratifiedKFold(5, shuffle, seed 42)` used everywhere | Complete (Milestone 2: one materialized fold list reused by all five models; mean/SD of F1 and ROC-AUC reported; match `cross_validate` on the same folds) |
| R11 | Small parameter searches; class weighting where supported | p.3 | 2–3 | Documented search grids and results | Grids and chosen parameters listed | Pending |
| R12 | Select model by mean validation F1; ROC-AUC breaks ties | p.3 | 3 | Selection table and rationale | Selection rule applied programmatically | Pending |
| R13 | Choose threshold from training CV (out-of-fold) predictions | p.3 | 3 | Threshold curve and chosen value (maximize F1) | Threshold derived without test data | Pending |
| R14 | Freeze model and threshold; evaluate test set once; no tuning from test results | p.3 | 4 | Final-evaluation section | Single test-evaluation cell after freeze | Pending |
| R15 | Report precision, recall, F1, ROC-AUC, confusion matrix on held-out data | p.1 Obj. 2, p.3 | 4 | Metrics table, confusion matrix figure in `results/` | Metrics produced by executed code | Pending |
| R16 | Success: churn-class F1 > 0 and ROC-AUC > 0.50 and exceeds baseline; explain unmet objectives | p.1 Obj. 1, p.3 | 4 | Baseline vs selected model comparison and discussion | Comparison against baseline numbers | Pending |
| R17 | Precision in top 10% and top 20% of ranked customers; compare with test-set churn rate | p.1 Obj. 3, p.3 | 4 | Ranking-precision table | Computed from test probabilities | Pending |
| R18 | Repeated permutation importance on original columns (drop in test ROC-AUC); five leading predictors; report variability; note correlated features | p.1 Obj. 3, p.3 | 4 | Importance table and plot | Repeats and std reported; applied to the frozen model | Pending |
| R19 | Interpret importance as association, not cause | p.3 | 4–5 | Discussion text | Text review | Pending |
| R20 | Retrospective framing; no claims of advance prediction, retention impact, or deployment performance; note missing usage/complaint/timestamp data | p.1–3 | 5 | Limitations section | Text review against proposal | Pending |
| R21 | One reproducible Jupyter notebook as the primary deliverable | p.3 Expected Outcomes | 5 | Single notebook in `notebooks/` | Restart-and-run-all completes | Pending (Milestone 1 and 2 sections execute end to end; notebook continues in later milestones) |
| R22 | Reproducible environment | CLAUDE.md | 0B | `.venv`, pinned `requirements.txt` | Imports and recreation test | Complete (see Milestone 0B report) |
| R23 | Guardrails documented | CLAUDE.md | 0B | `CLAUDE.md` | File review | Complete |
