# W2 Tool Research: Automation-Driven ML

**Date:** 2026-03-04

---

## 1. Automated EDA

### YData Profiling (v4.18.1)
- Install: `pip install ydata-profiling[notebook]`
- Import: `from ydata_profiling import ProfileReport`
- Use `minimal=True` for 80+ features (avoids quadratic correlation computation)
- Display: `profile.to_notebook_iframe()` or save via `profile.to_file()`
- Gotcha: `visions` warning is cosmetic, harmless

### SweetViz (v2.3.1)
- Install: `pip install sweetviz`
- Import: `import sweetviz as sv`
- Target analysis: `sv.analyze(df, target_feat="SalePrice")` — shows per-feature correlation with target
- Display: `report.show_notebook(w="100%", h=700)` or `report.show_html()`
- Set `pairwise_analysis="off"` for speed with 80+ features
- Can compare train/test: `sv.compare([train, "Train"], [test, "Test"], target_feat="SalePrice")`
- Skip irrelevant columns: `feat_cfg = sv.FeatureConfig(skip="Id")`

**Sources:** PyPI (ydata-profiling 4.18.1, sweetviz 2.3.1), GitHub READMEs

---

## 2. Feature Engineering

### Autofeat (v2.1.3) — PRIMARY
- Install: `pip install autofeat`
- Class: `AutoFeatRegressor` (sklearn-compatible)
- **Critical:** Limit `feateng_steps=1` and `feateng_cols` to 10-15 numeric columns
  - Steps=2 with 30 cols → 16,141 candidates, 35+ min runtime
  - Steps=2 with 80+ cols → potentially hours
- Output: curated features with interpretable symbolic formulas (e.g., `log(GrLivArea) * OverallQual`)
- Built-in L1-regularized feature selection
- Gotcha: numpy downgraded to 1.26.4 (numba dependency)

### Featuretools (v1.31.0) — BRIEF DEMO ONLY
- Single-table usage works but limited to transform primitives only (no aggregation without relationships)
- Generates massive feature explosion with zero built-in selection
- 30 numeric cols × `multiply_numeric` + `add_numeric` → 900 features, no intelligence about relevance
- **Verdict:** Designed for relational data; limited value on flat table. Include briefly to demonstrate exploration + reflection insight

**Sources:** Direct testing on local machine, constructor docstrings

---

## 3. Automated Model Training

### PyCaret (v3.x) — PRIMARY
- Install: `pip install pycaret` (or `pycaret[full]` for all models)
- 3 lines to leaderboard:
  ```python
  reg = setup(data=train_df, target='SalePrice', session_id=42)
  best = compare_models(include=['lr', 'rf', 'gbr', 'xgboost', 'lightgbm'])
  preds = predict_model(best, data=test_df)
  ```
- Auto preprocessing (imputation, encoding, scaling)
- Presentation-ready comparison table (MAE, MSE, RMSE, R², MAPE)
- Built-in `interpret_model()` wrapping SHAP
- Gotcha: `setup()` can be interactive in older versions — use `verbose=False`
- Gotcha: `compare_models()` without `include=` trains all algorithms (slow)

### H2O AutoML — OPTIONAL SECONDARY
- Requires JVM, `h2o.init()`, data conversion to H2OFrame
- More manual prep, but auto-generates stacked ensembles
- Higher memory overhead (unnecessary for 1,500 rows)
- If included: quick run with `max_runtime_secs=120` for comparison

**Verdict:** PyCaret is more practical for a 2-hour project. H2O optional for breadth.

**Sources:** Package documentation, course class notes

---

## 4. Model Interpretation (SHAP — light)

- PyCaret: `interpret_model(best, plot='summary')` and `interpret_model(best, plot='reason', observation=0)` — 2 lines
- Standalone SHAP (if needed):
  ```python
  explainer = shap.Explainer(model, X_train)
  shap_values = explainer(X_test)
  shap.plots.bar(shap_values, max_display=15)
  shap.plots.beeswarm(shap_values, max_display=15)
  ```
- Gotcha: stacked ensembles require KernelExplainer (slow); use best single model instead
- Keep light — w4 covers SHAP/LIME in depth

**Sources:** SHAP docs (shap.readthedocs.io), PyCaret docs

---

## Time Allocation (2-hour budget)

| Phase | Time | Tool |
|-------|------|------|
| Setup + data loading | 10 min | subprocess, pandas |
| Automated EDA | 20 min | YData Profiling + SweetViz |
| Preprocessing | 15 min | pandas (informed by EDA) |
| Feature engineering | 25 min | Autofeat (+ brief Featuretools) |
| Model training | 25 min | PyCaret (+ optional H2O) |
| Interpretation | 10 min | PyCaret interpret_model / SHAP |
| Reflections | 15 min | Markdown cells |
