# W2 Plan v3: Predicting House Sale Prices with Automation-Driven ML

**Time budget:** 2 hours
**Dataset:** Ames Housing (~2,900 homes, 82 features) — [Google Drive](https://drive.google.com/file/d/1-KhJhFnj5vmESlknOtpra-2E86_thEWO/view?usp=sharing)
**Target:** SalePrice (regression)
**Environment:** Local Jupyter, Colab-compatible
**Previous version:** [v2](done/w2-plan-v2.md) (archived — over-specified cell-level details before data was explored)

---

## Tool Decisions

| Stage | Tool | Rationale |
|-------|------|-----------|
| EDA | YData Profiling + SweetViz | Complementary: YData for deep stats, SweetViz for visual target analysis |
| Feature Engineering | Autofeat (primary) + Featuretools (brief) | Autofeat fits flat numeric data; Featuretools demo for contrast |
| Modeling | PyCaret (primary) | Simpler API, built-in preprocessing, presentation-ready output |
| Baseline | sklearn LinearRegression | Manual baseline to quantify what automation improves |
| Interpretation | PyCaret `interpret_model()` | SHAP wrapped natively; w4 covers SHAP/LIME in depth |

---

## Phases

### 1. Setup & Data Loading

**Goal:** Working environment with data loaded and initially inspected.

- Installs via `subprocess.check_call` (Colab-compatible)
- Local path first, Google Drive fallback
- Confirm shape, dtypes, target distribution

### 2. Automated EDA

**Goal:** Understand the dataset's structure, quality issues, and strongest predictors — using automated tools to demonstrate their value over manual inspection.

- YData Profiling (`minimal=True`) — statistical overview, alerts, data quality
- SweetViz (`pairwise_analysis="off"`) — target-aware feature analysis
- Synthesize findings: what each tool surfaced, what it missed, and what this means for preprocessing

**Decisions that emerge here:** which features to drop, missing value strategies, outlier handling, feature selection candidates for engineering.

### 3. Data Preprocessing

**Goal:** Clean the data based on EDA findings, with each decision explicitly justified.

Specifics determined by Phase 2 findings. Expected concerns: missing values (structural vs. genuinely missing), ID columns, high-cardinality or near-zero-variance features.

### 4. Automated Feature Engineering

**Goal:** Demonstrate automated feature generation and assess whether it adds predictive value.

- Autofeat: select top numeric columns (informed by EDA correlations), `feateng_steps=1`
- Featuretools: brief demo to contrast approaches on flat vs. relational data
- Assess: which generated features make domain sense, which don't

### 5. Modeling

**Goal:** Quantify the difference between manual and automated ML approaches.

- Manual baseline: sklearn LinearRegression on preprocessed features (no engineered features)
- PyCaret: `compare_models()` across several algorithms, then `tune_model()` on the best
- Compare: baseline vs. automated, raw vs. engineered features, pre-tuning vs. post-tuning

### 6. Interpretation

**Goal:** Understand what drives predictions, not just accuracy numbers.

- SHAP via `interpret_model()` — summary and individual observation
- Do the top predictors align with domain expectations?

### 7. Reflections

**Goal:** Honest assessment of automation's impact on this workflow.

- What automation improved (speed, breadth)
- Where human judgment was essential (preprocessing decisions, feature selection)
- Limitations encountered
- Baseline vs. automated: what automation actually delivered

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

1. `requirements.txt` — pinned dependencies
2. Notebook: `notebooks/w2_house_prices.ipynb`
3. HTML reports: YData Profiling + SweetViz (saved to outputs/)
4. Presentation-ready: clear prose narrative throughout

## Success Criteria

- All required tools demonstrated (EDA, feature engineering, AutoML)
- Baseline comparison quantifies automation value
- Model tuning explicitly shown
- Clear analytical reasoning in prose markdown cells
- Model evaluation interpreted, not just reported
- Reflections show genuine understanding of automation trade-offs

## Changes from v2

v2 pre-specified 20 cells with implementation details (preprocessing strategies, column selections, specific code) before any data exploration. This locked in decisions that should have been informed by EDA findings. v3 defines phases with objectives and lets implementation details emerge from what each phase reveals.
