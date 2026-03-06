# W2 Preliminary Plan: Predicting House Sale Prices

**Time budget:** 2 hours
**Dataset:** Ames Housing (~1,500 homes, 80+ features) via Google Drive
**Target:** SalePrice (regression)
**Environment:** Local Jupyter, Colab-compatible

---

## Notebook Outline (~15-18 cells)

### Section 1: Setup & Data Loading (~15 min)
- Cell 1 (md): Title, author, date, objective, notebook structure
- Cell 2 (code): Installs + imports (subprocess.check_call pattern)
- Cell 3 (code): Data loading (local-first, Google Drive fallback)
- Cell 4 (code): Initial inspection (shape, dtypes, target distribution, missing values overview)

### Section 2: Automated EDA (~25 min)
- Cell 5 (md): EDA approach — why two tools, what each excels at
- Cell 6 (code): YData Profiling report generation
- Cell 7 (code): SweetViz report generation (with SalePrice as target)
- Cell 8 (md): EDA findings — what reports revealed, data quality issues, key correlations

### Section 3: Data Preprocessing (~15 min)
- Cell 9 (code): Handle missing values + encode categoricals + train/test split
- Cell 10 (md): Preprocessing decisions — informed by EDA findings

### Section 4: Automated Feature Engineering (~20 min)
- Cell 11 (code): Autofeat and/or Featuretools
- Cell 12 (md): Feature engineering assessment — what was generated, relevance

### Section 5: Automated Model Training (~25 min)
- Cell 13 (code): H2O AutoML or PyCaret — model training + leaderboard
- Cell 14 (code): Evaluation metrics (RMSE, MAE, R²) on test set
- Cell 15 (md): Model results interpretation

### Section 6: Interpretation (~10 min)
- Cell 16 (code): Feature importance (SHAP or model-native)
- Cell 17 (md): Key predictors discussion

### Section 7: Reflections (~10 min)
- Cell 18 (md): Reflections on automation — productivity, limitations, insights

---

## Tool Selection (to validate via research)

| Stage | Tool | Rationale |
|-------|------|-----------|
| EDA | YData Profiling + SweetViz | Assignment requires both; complementary strengths |
| Feature Engineering | Autofeat (primary) | Better fit for flat numeric data; Featuretools if feasible |
| Modeling | TBD (H2O or PyCaret) | Assess objectively — research needed |
| Interpretation | SHAP (light) | w4 covers SHAP/LIME deeply; keep brief here |

## Research Needed
1. H2O AutoML vs PyCaret for regression — which is more practical in 2hrs?
2. Autofeat current best practices and gotchas
3. Featuretools feasibility with single-table data (Ames Housing)
4. SHAP quick usage patterns for regression
5. YData Profiling + SweetViz latest API / any breaking changes
