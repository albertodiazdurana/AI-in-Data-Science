# W2 Plan v2: Predicting House Sale Prices with Automation-Driven ML

**Time budget:** 2 hours
**Dataset:** Ames Housing (~1,500 homes, 80+ features) — [Google Drive](https://drive.google.com/file/d/1-KhJhFnj5vmESlknOtpra-2E86_thEWO/view?usp=sharing)
**Target:** SalePrice (regression)
**Environment:** Local Jupyter, Colab-compatible
**Research:** [tool-research.md](../research/w2/tool-research.md)
**Audit:** [plan-audit.md](../research/w2/plan-audit.md) (v1 gaps addressed below)

---

## Tool Decisions

| Stage | Tool | Rationale |
|-------|------|-----------|
| EDA | YData Profiling + SweetViz | Complementary: YData for deep stats, SweetViz for visual target analysis |
| Feature Engineering | Autofeat (primary) + Featuretools (brief) | Autofeat fits flat numeric data; Featuretools demo for contrast |
| Modeling | PyCaret (primary) | Simpler API, built-in preprocessing, presentation-ready output |
| Baseline | sklearn LinearRegression | Manual baseline to quantify what automation improves (GAP-2 fix) |
| Interpretation | PyCaret `interpret_model()` | SHAP wrapped natively; w4 covers SHAP/LIME in depth |

---

## Pre-Notebook Setup

### Environment (one-time, before notebook work)

1. Create virtual environment: `python -m venv .venv`
2. Activate: `source .venv/bin/activate`
3. Install Jupyter kernel: `pip install ipykernel && python -m ipykernel install --user --name ai-data-science`
4. Install dependencies (order matters due to numpy constraints):
   ```
   pip install autofeat
   pip install ydata-profiling[notebook] sweetviz featuretools pycaret shap
   ```
5. Generate `requirements.txt`: `pip freeze > requirements.txt`

The `requirements.txt` ensures reproducibility locally. The notebook's Cell 2
handles installs for Colab users who don't have the venv.

---

## Notebook Structure (~20 cells)

**Markdown style note:** All markdown cells should be written as short prose
paragraphs (not just bullet lists) to support presentation narrative flow. (GAP-3 fix)

### Section 1: Setup & Data Loading

**Cell 1** (markdown) — Title, author, date, objective, dataset description, notebook structure outline

**Cell 2** (code) — Installs + imports
- `subprocess.check_call` pattern for Colab compatibility
- Libraries: ydata-profiling, sweetviz, autofeat, featuretools, pycaret, shap, sklearn
- `os.makedirs` for output dirs
- Version prints

**Cell 3** (code) — Data loading
- Local path first, Google Drive download fallback
- Initial inspection: shape, dtypes, head, target distribution
- Skip/note Id-like columns

### Section 2: Automated EDA

**Cell 4** (markdown) — EDA approach: why two tools, what each excels at

**Cell 5** (code) — YData Profiling report
- `ProfileReport(df, title="Ames Housing", minimal=True)`
- Save to HTML + display iframe

**Cell 6** (code) — SweetViz report
- `sv.analyze(df, target_feat="SalePrice", pairwise_analysis="off")`
- Skip Id column via `FeatureConfig`
- Save to HTML + display notebook

**Cell 7** (markdown) — EDA findings synthesis
- Key correlations with SalePrice
- Data quality issues (missing values, outliers, skewness)
- What each tool surfaced that the other missed
- Preprocessing decisions informed by reports

### Section 3: Data Preprocessing

**Cell 8** (code) — Preprocessing
- Handle missing values (strategy from EDA findings)
- Encode categoricals if needed (PyCaret handles this, but prepare data)
- Address outliers if flagged
- Train/test split (80/20)

**Cell 9** (markdown) — Preprocessing decisions explained

### Section 4: Automated Feature Engineering

**Cell 10** (code) — Autofeat
- Select 10-15 top numeric columns (informed by EDA correlations)
- `AutoFeatRegressor(feateng_steps=1, feateng_cols=selected_cols, verbose=1)`
- Print new features and formulas

**Cell 11** (code) — Featuretools (brief)
- Single-table EntitySet, transform primitives only
- Note the limitation: no aggregation without relationships

**Cell 12** (markdown) — Feature engineering assessment
- What Autofeat generated and why those features make sense
- Featuretools contrast: designed for relational data, limited on flat table
- Final feature set decision

### Section 5: Modeling

**Cell 13** (code) — Manual baseline model
- sklearn `LinearRegression` on preprocessed features (no engineered features)
- Evaluate: RMSE, MAE, R²
- This establishes the "without automation" reference point

**Cell 14** (code) — PyCaret setup + model comparison
- `setup(data=train_df, target='SalePrice', session_id=42)`
- `compare_models(include=['lr', 'rf', 'gbr', 'xgboost', 'lightgbm'])`
- Display leaderboard

**Cell 15** (code) — Tune + evaluate best model
- `tuned = tune_model(best)` (GAP-1 fix)
- `predict_model(tuned, data=test_df)`
- Metrics: RMSE, MAE, R²
- Compare tuned metrics against baseline from Cell 13

**Cell 16** (markdown) — Model results interpretation
- Which algorithms performed best and why
- Baseline vs. automated comparison: quantify the improvement
- Impact of tuning on performance
- Impact of automated feature engineering on results

### Section 6: Interpretation

**Cell 17** (code) — Feature importance via SHAP
- `interpret_model(tuned, plot='summary')`
- `interpret_model(tuned, plot='reason', observation=0)`

**Cell 18** (markdown) — Key predictors discussion
- Top features driving SalePrice
- Any surprises vs. domain expectations

### Section 7: Reflections

**Cell 19** (markdown) — Reflections on automation
- What automation improved (speed, breadth of exploration)
- Where human judgment was essential (feature selection, preprocessing decisions)
- Limitations encountered (Autofeat runtime, Featuretools on flat data)
- Baseline vs. automated results: what automation actually delivered
- Key insights about automated ML workflows
- Productivity assessment

**Cell 20** (markdown) — Summary and next steps (optional, brief)

---

## Risk Mitigations

| Risk | Mitigation |
|------|-----------|
| Autofeat runtime explosion | `feateng_steps=1`, limit to 10-15 cols |
| PyCaret install conflicts | Use `pip install pycaret` (not `[full]`) if needed |
| Numpy version conflict (Autofeat needs 1.26) | Install autofeat first, then other packages |
| EDA reports slow on 80+ features | `minimal=True` (YData), `pairwise_analysis="off"` (SweetViz) |

---

## Deliverables

1. `requirements.txt` — pinned dependencies for reproducibility
2. Notebook: `notebooks/w2_house_prices.ipynb` (~20 cells)
3. HTML reports: YData Profiling + SweetViz (saved to outputs/)
4. Presentation-ready: clear markdown narrative throughout

## Success Criteria

- All required tools demonstrated (EDA, feature engineering, AutoML)
- Baseline comparison quantifies automation value
- Model tuning explicitly shown
- Clear analytical reasoning in prose markdown cells
- Model evaluation interpreted, not just reported
- Reflections show genuine understanding of automation trade-offs

## Changes from v1

| Gap | Fix | Cells Affected |
|-----|-----|----------------|
| GAP-1: Model tuning not explicit | Added `tune_model(best)` | Cell 15 |
| GAP-2: No baseline comparison | Added sklearn LinearRegression baseline | Cell 13, Cell 16 |
| GAP-3: Markdown prose format | Added style note; Cell 19 references baseline comparison | All markdown cells |