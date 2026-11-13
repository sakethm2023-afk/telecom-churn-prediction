# CLAUDE.md — DADS 7275 Telecom Customer Churn Prediction

Governs all future development. `docs/Final_Project_Proposal.pdf` is the primary source of truth;
`docs/requirements_traceability.md` tracks each requirement.

**Framing:** retrospective churn-classification study. Never claim advance-prediction accuracy, causal
effects, retention impact, or deployment performance. Importances are associations, not causes.

## Environment
- Windows 11, Python 3.12.10, project-local `.venv/` (git-ignored). Use `.\.venv\Scripts\python.exe` for all commands.
- Install only via `requirements.txt` (exact pins). Do not install TensorFlow, PyTorch, XGBoost, LightGBM,
  GPU libraries, or experiment trackers. Any new dependency must be justified and approved.
- Gradient Boosting = scikit-learn (`HistGradientBoostingClassifier` preferred; supports `class_weight`).

## A. Proposal compliance
- Any deviation from the proposal must be identified and approved before implementation.

## B. Reproducibility
- Random seed 42 everywhere.
- One stratified 75/25 train/test split, created once.
- Same `StratifiedKFold(n_splits=5, shuffle=True, random_state=42)` folds for every model.
- Record modeling decisions (and why) in the notebook.
- The final notebook must run top to bottom. One primary notebook in `notebooks/`; no duplicates.

## C. Data integrity
- Never modify files in `data/raw/`.
- Exclude `customerID` from modeling.
- Blank `TotalCharges` -> NaN; document missing-value handling.
- Keep the 22 records that match other records except for customer ID unless later evidence justifies removal.
- No target leakage; review features before modeling.

## D. Machine learning methodology
- Use scikit-learn pipelines; fit imputers, encoders, scalers on training data only (inside each CV fold).
- Five-fold stratified CV on the 75% training set.
- Compare all four models (Logistic Regression, Decision Tree, Random Forest, Gradient Boosting) plus the
  majority-class dummy baseline.
- Select by mean validation F1; ROC-AUC breaks ties.
- Choose the threshold from out-of-fold training predictions, maximizing F1 unless approved otherwise.
- Freeze model and threshold before touching the test set; evaluate the test set once.
- Never use test results to guide tuning or feature selection.
- Report precision, recall, F1, ROC-AUC, confusion matrices, top-10%/20% precision vs test churn rate, and
  repeated permutation importance (five leading predictors, with variability).

## E. Academic integrity and reporting
- Never fabricate results; report only metrics produced by executed code.
- Separate findings from assumptions; explain limitations and unsuccessful experiments.
- No unsupported causal claims.
- Keep the analysis understandable and defensible in an academic presentation.

## F. Repository management
- Work only within the approved milestone scope; stop for review after each milestone.
- No unnecessary infrastructure, modules, or placeholder scripts. Add `src/` only if justified.
- Do not commit, push, merge, switch branches, or open pull requests without explicit authorization.
- Do not modify unrelated files. Explain major implementation decisions.

## G. PDF report handling
- Assessment/milestone PDFs are generated outside the repo (session scratchpad or other temp location) and
  delivered as downloadable attachments in the Claude conversation.
- Never place them in the repo, add them to Git, commit, or push them.
- If PDF generation fails, report the failure; do not silently substitute another format.
- Exception: academic deliverables required to be version-controlled, with approval.
