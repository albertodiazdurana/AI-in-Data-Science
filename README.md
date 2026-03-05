# AI in Data Science

**AI Enhanced Productivity Series** - MasterSchool Data Science Course

## The Question

When data science tools promise to automate EDA, feature engineering, and model selection, what do they actually deliver? This series explores that question across three projects, each using a different set of AI and automation tools.

## Sub-Projects

| Week | Project | Dataset | AI/Automation Tools |
|------|---------|---------|---------------------|
| w2 | Predicting House Sale Prices | Ames Housing (~2,900 homes, 80+ features) | YData Profiling, SweetViz, Autofeat, Featuretools, PyCaret, SHAP |
| w3 | Customer Sentiment on Twitter | Customer Support on Twitter | Gemini, AutoViz, Hugging Face Transformers |
| w4 | Why Did My Model Predict That? | Heart Disease UCI (303 records) | SHAP, LIME |

### Narrative Arc

- **w2 (Automation):** How much does automating EDA, feature engineering, and model selection actually improve results?
- **w3 (AI-Assisted Analysis):** Can Gemini, AutoViz, and Hugging Face accelerate text and sentiment analysis?
- **w4 (Trust & Transparency):** Do SHAP and LIME make model predictions understandable to non-technical audiences?

### Automation vs Conventional Baselines

| AI/Automation Tool | Conventional Baseline | Verdict |
|--------------------|----------------------|---------|
| **W2: EDA** | | |
| YData Profiling, SweetViz | pandas + focused analysis cells | Low value. 82-feature reports produced too much output to analyze; 3 targeted cells (missing values, correlations, near-zero-variance) drove all preprocessing decisions |
| **W2: Feature Engineering** | | |
| Autofeat (10 engineered features) | scikit-learn LinearRegression (10 numeric) | Low value. R² improved only +0.004 (0.80 → 0.81); polynomial transforms added minimal signal to a linear model |
| Featuretools (992 features) | Autofeat (10 curated features) | Not recommended. Generated 992 unfiltered features from a flat table with no built-in selection; designed for relational data, poor fit here |
| **W2: Model Selection** | | |
| PyCaret GBR (automated) | scikit-learn LinearRegression (manual) | High value. R² jumped +0.12 (0.80 → 0.93), RMSE dropped $15,480; 3 lines of code to compare 15 models and tune the winner |
| **W3: Analysis** | | |
| Gemini | Manual pandas exploration | *TBD* |
| AutoViz | matplotlib/seaborn | *TBD* |
| Hugging Face Transformers | Labeled data validation | *TBD* |
| **W4: Explainability** | | |
| SHAP, LIME | Cross-method agreement | *TBD* |

## W2 Results

| Approach | R² | RMSE |
|----------|-----|------|
| Baseline LinearRegression (10 numeric features) | 0.80 | $39,568 |
| Autofeat LinearRegression (10 original + 10 engineered) | 0.81 | $39,197 |
| PyCaret GradientBoosting (tuned, full feature set) | 0.93 | $24,088 |

**Key finding:** Automated feature engineering (Autofeat) added R² = +0.004. Automated model selection (PyCaret) added R² = +0.12. Tool selection matters more than tool quantity.

## W2 Key Findings

1. **Automated EDA is reference material, not analysis.** YData Profiling and SweetViz generated comprehensive reports across 82 features, but three focused cells asking specific questions were more actionable than scanning the full output.
2. **Model selection delivered 30x more improvement than feature engineering.** Switching from LinearRegression to GradientBoosting mattered far more than adding engineered features.
3. **Plans should match your certainty level.** A 20-cell outline written before loading data became wrong after EDA. Phase-based objectives adapted; cell-level prescriptions did not.

## Project Structure

```
├── notebooks/
│   └── w2_house_prices.ipynb        # W2 notebook (Colab-compatible)
├── data/                            # Datasets (local, not committed)
├── docs/
│   ├── plans/                       # Phase-based project plans
│   ├── research/                    # Tool research and audits
│   ├── decisions/                   # Decision records (D001-D009)
│   ├── blog/                        # Project blog posts
│   ├── feedback/                    # DSM methodology feedback
│   ├── guides/                      # AI collaboration guide
│   └── checkpoints/                 # Session state snapshots
├── requirements.txt                 # Python dependencies (140 packages)
└── _reference/                      # Course materials (not committed)
```

## Run the Notebook

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/w2_house_prices.ipynb
```

**Data:** Ames Housing dataset downloads automatically from Google Drive on first run if `data/AmesHousing.csv` is not present.

## Evaluation

Live presentation: reasoning, insight, and productivity impact (not code walkthrough).

## Author

**Alberto Diaz Durana** - March 2026
