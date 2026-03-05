# W4 Plan: Why Did My Model Predict That?

**Budget:** 1 hour
**Notebook:** `notebooks/w4_explainability.ipynb`
**Dataset:** Heart Disease UCI (303 records, 13 features + target)
**Data source:** Google Drive (file ID from project description)

## Phase 1: Setup and Data Loading
**Objective:** Install dependencies, load dataset, initial inspection.

**Packages:** shap, lime, xgboost, pandas, numpy, matplotlib, seaborn, scikit-learn

**Data loading:** Local path first (`data/heart.csv`), otherwise download from Google Drive. Print shape, dtypes, sample rows, missing values.

**Key first look:**
- 303 records, 13 clinical features
- Target column: `num` (0 = no disease, 1-4 = disease presence)
- Check for missing values (dataset has `?` markers in some versions)
- Feature types: mix of continuous (age, trestbps, chol, thalach, oldpeak) and categorical (sex, cp, fbs, restecg, exang, slope, ca, thal)

## Phase 2: Preprocessing
**Objective:** Prepare data for modeling.

**Steps:**
- Binarize target: `num > 0` = 1 (disease), 0 = no disease
- Handle missing values (replace `?` with NaN, then impute or drop)
- Encode categorical variables if needed
- Train/test split (80/20, stratified)

## Phase 3: Model Training and Evaluation
**Objective:** Train tree-based models, evaluate with standard classification metrics.

**Models:** Random Forest + XGBoost
**Metrics:** Accuracy, precision, recall, F1, AUC (ROC curve)
**Deliverable:** Classification report + comparison; select best model for explainability

## Phase 4: SHAP Analysis
**Objective:** Global and local explanations using SHAP.

**Approach:**
- `shap.TreeExplainer` (optimized for tree models)
- Summary plot (beeswarm): feature impact distribution across all predictions
- Bar chart: global feature importance
- Waterfall plot: explain 1-2 individual predictions (one positive, one negative case)
- Markdown cell: interpret which clinical features matter most and why

## Phase 5: LIME Analysis
**Objective:** Local explanations for individual predictions, compare with SHAP.

**Approach:**
- `LimeTabularExplainer` with mode='classification'
- Explain at least 2 test cases (same cases used in SHAP waterfall for direct comparison)
- Visualize feature contributions
- Markdown cell: compare LIME results with SHAP; note agreements and disagreements

## Phase 6: Business Interpretation and Reflections
**Objective:** Connect technical findings to real-world healthcare context.

**Deliverables:**
- Which clinical features most strongly influence predictions (and clinical plausibility)
- How explainability affects trust in healthcare models
- When to rely on SHAP vs LIME (trade-offs)
- Limitations of post-hoc explanations
- Connection to W2/W3: how the role of AI changed across projects

## Tool Decisions

| Stage | Tool | Rationale |
|-------|------|-----------|
| Modeling | Random Forest + XGBoost | Tree-based; optimal for TreeExplainer; compare two |
| SHAP | shap.TreeExplainer | Fast, accurate for tree models; global + local views |
| LIME | LimeTabularExplainer | Model-agnostic local explanations; required comparison |

## Notebook Cell Map

| Cell | Content |
|------|---------|
| 0 | Markdown header |
| 1 | Installs + imports |
| 2 | Data loading + exploration |
| MD | EDA observations |
| 3 | Preprocessing + train/test split |
| 4 | Model training + evaluation |
| MD | Model comparison |
| 5 | SHAP analysis (summary, bar, waterfall) |
| MD | SHAP interpretation |
| 6 | LIME analysis (2+ cases) |
| MD | LIME interpretation + SHAP comparison |
| 7 | Side-by-side comparison visualization |
| MD | Business interpretation + reflections |

## Success Criteria
- Both SHAP and LIME used with interpreted outputs
- Model evaluation metrics interpreted, not just reported
- Reflections connect technical explanations to healthcare decision-making
- Notebook is reproducible and Colab-compatible