# Requirements Traceability

Source: `docs/Final_Project_Proposal.pdf` (page references are to the proposal PDF). Guardrails: `CLAUDE.md`.

**Milestones:** 0B setup · 1 data integrity, split, training-set EDA · 2 pipelines, baseline, 5-fold CV ·
3 model and threshold selection · 4 final test evaluation, ranking, importance · 5 notebook finalization.

| ID | Requirement | Proposal ref | Milestone | Expected deliverable | Verification method | Status |
|---|---|---|---|---|---|---|
| R1 | Use the Kaggle Telco CSV (7,043 rows, 21 columns) as the sole analytical dataset; do not combine with IBM data | p.2 Dataset | 1 | Notebook loads the raw CSV unmodified | Shape check; raw file hash unchanged | Pending (file present, loads as 7,043 x 21) |
| R2 | Check duplicates, missing values, class proportions | p.2 Dataset | 1 | Data-integrity section | Printed counts in executed cells | Pending |
| R3 | Exclude `customerID`; convert blank `TotalCharges` to missing | p.3 Methodology | 1 | Preprocessing code and documented handling | Assertions on columns and NaN count | Pending |
| R4 | Review predictors for outcome leakage | p.3 | 1 | Feature-review notes | Written justification per questionable feature | Pending |
| R5 | Stratified 25% test set, fixed seed (42), created once | p.3 | 1 | Single split cell | Class ratio and sizes in train/test; seed recorded | Pending |
| R6 | Explore distributions/relationships with churn on training data only | p.3 | 1 | Training-set EDA section and figures | No test rows used in EDA code | Pending |
| R7 | Pipelines fit imputation, one-hot encoding, scaling (logistic regression) within each training fold | p.3 | 2 | `Pipeline`/`ColumnTransformer` objects | Code review; transformers only inside pipelines | Pending |
| R8 | Majority-class dummy baseline | p.1, p.3 | 2 | Baseline CV and test metrics | Baseline row in results table | Pending |
| R9 | Compare Logistic Regression, Decision Tree, Random Forest, Gradient Boosting on same splits | p.1 Obj. 2 | 2 | Comparison table | All four models share identical folds | Pending |
| R10 | Five-fold stratified CV on the 75% training data | p.3 | 2 | CV results with mean/std F1 and ROC-AUC | `StratifiedKFold(5, shuffle, seed 42)` used everywhere | Pending |
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
| R21 | One reproducible Jupyter notebook as the primary deliverable | p.3 Expected Outcomes | 5 | Single notebook in `notebooks/` | Restart-and-run-all completes | Pending |
| R22 | Reproducible environment | CLAUDE.md | 0B | `.venv`, pinned `requirements.txt` | Imports and recreation test | Complete (see Milestone 0B report) |
| R23 | Guardrails documented | CLAUDE.md | 0B | `CLAUDE.md` | File review | Complete |
