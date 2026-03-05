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
| **W3: Exploration** | | |
| Gemini (API) | Manual pandas exploration | Moderate value. Produced a structured dataset overview in a single prompt; useful for initial orientation on unfamiliar data, but suggestions were generic |
| AutoViz | matplotlib/seaborn | Low value for text data. Built for numeric tabular datasets; on text-heavy data it functions as a basic data quality checker, not a visualization tool |
| **W3: Sentiment** | | |
| Hugging Face Transformers (RoBERTa) | Classical NLP pipeline (TF-IDF + classifier) | High value. Pre-trained tweet-specific model classified 49 tweets with no labeled data and minimal preprocessing; confidence scores enable quality assessment |
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

## W2 Conclusions

**What automation delivered:**
PyCaret's automated model selection was the clear winner, improving R² by +0.12 over the manual baseline with three lines of code. It compared 15 regression models, tuned the best (GradientBoosting), and handled categorical encoding internally; replicating this manually would have required significant effort for each model configuration.

**Where automation fell short:**
Automated EDA (YData Profiling, SweetViz) generated comprehensive HTML reports across 82 features, but the volume made them impractical to analyze systematically. Three focused cells, one for missing values grouped by strategy, one for target correlations with anomaly flags, one for near-zero-variance detection, drove every preprocessing decision. The reports served as reference material, not as analysis. Autofeat's 10 engineered features (polynomial transforms) improved R² by only +0.004 with LinearRegression; the nonlinear transforms needed a nonlinear model to add value. Featuretools generated 992 unfiltered features from a flat table, confirming it is designed for relational data, not single-table regression.

**The value of running both manual and automated approaches:**
Implementing both conventional baselines alongside automated tools does not drastically increase effort, and the comparison provides a concrete reference point. Without the manual LinearRegression baseline (R² = 0.80), PyCaret's result (R² = 0.93) would be a number without context. The side-by-side comparison is what makes the automation's contribution measurable and the conclusions credible. The cost of building a baseline is low; the insight it enables is high.

**What required human judgment:**
Interpreting missing values as structural absences (no pool, no garage) rather than data gaps. Identifying Garage Yr Blt = 2207 as a data entry error. Deciding which near-zero-variance features to drop versus keep (Kitchen AbvGr at 95.4% dominant value still carries signal). Recognizing that SHAP's top feature (Overall Qual) aligned with EDA correlations, validating the model learned real patterns.

**Presentation takeaway:**
Automation's value is uneven. Model selection automation (PyCaret) delivered a 30x larger improvement than feature engineering automation (Autofeat). Automated EDA tools are better treated as reference generators than as analysis tools. The productivity gain comes from knowing which automation to trust, where human judgment remains essential, and having a manual baseline to make the comparison meaningful.

## W3 Results

| Sentiment | Count | Share | Mean Confidence |
|-----------|-------|-------|-----------------|
| Negative  | 27    | 55%   | 0.83            |
| Neutral   | 14    | 29%   | 0.72            |
| Positive  | 8     | 16%   | 0.75            |

**Key finding:** The pre-trained Hugging Face pipeline (`cardiffnlp/twitter-roberta-base-sentiment-latest`) delivered production-quality sentiment classification with zero labeled training data. The tool closest to the analytical goal produced the most value; generic exploration tools (Gemini, AutoViz) helped but were more substitutable.

## W3 Key Findings

1. **Pre-trained models eliminate the labeling bottleneck.** A RoBERTa model fine-tuned on tweets classified 49 customer messages with 0.78 mean confidence, no labeled training data needed. Classical NLP pipelines require labeled datasets, tokenization, stemming, and stop-word removal before analysis can begin.
2. **Tool value correlates with task specificity.** Hugging Face (sentiment-specific) delivered the core result. Gemini (general exploration) provided useful orientation but generic suggestions. AutoViz (built for numeric data) added little on a text-heavy dataset.
3. **Environment setup is the real time cost.** API quota issues, deprecated model names, and library version conflicts consumed more time than the analysis itself. Each AI tool works well in isolation; combining them multiplies the integration surface area.

## W3 Conclusions

**What AI-assisted analysis delivered:**
Hugging Face Transformers was the clear winner. A single pipeline call loaded a tweet-specific RoBERTa model, classified all 49 inbound tweets into three sentiment categories, and returned confidence scores, all without labeled data, custom preprocessing, or model training. The results were immediately interpretable: 55% negative (expected for support), highest confidence on complaints (0.83), and clear company-level patterns (AppleSupport heavily negative, SpotifyCares positive-leaning).

**Where AI-assisted analysis fell short:**
Gemini's dataset exploration was competent but generic. Everything it identified (threading columns, datetime conversion needs, inbound/outbound segmentation) could have been found with standard pandas inspection. Its suggestions applied to any customer support dataset, not specifically to this one. AutoViz was a poor fit for text data: with only two derived numeric features, it produced minimal visualization and spent most of its value on data quality checks that `df.duplicated().sum()` would have answered faster.

**The productivity pattern:**
The W2 lesson was that model selection automation delivers more value than feature engineering automation. The W3 lesson is analogous: the tool closest to the analytical goal delivers the most value. Generic exploration tools save time on orientation but do not produce analytical results. The main time investment was not analysis but infrastructure: resolving API quotas, model deprecation, and library compatibility. This is the realistic cost of multi-tool AI workflows.

## W4 Results

*Coming soon: SHAP and LIME explainability on the Heart Disease UCI dataset.*

## Project Structure

```
├── notebooks/
│   ├── w2_house_prices.ipynb        # W2 notebook (Colab-compatible)
│   └── w3_customer_sentiment.ipynb  # W3 notebook (Colab-compatible)
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
jupyter notebook notebooks/
```

**Data:** Both datasets download automatically from Google Drive on first run. W3 requires a `GOOGLE_API_KEY` in `.env` or Colab Secrets for the Gemini API cell.

## Evaluation

Live presentation: reasoning, insight, and productivity impact (not code walkthrough).

## Author

**Alberto Diaz Durana** - March 2026
