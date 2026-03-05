# AI in Data Science

**AI Enhanced Productivity Series** - MasterSchool Data Science Course

## The Question

When data science tools promise to automate EDA, feature engineering, and model selection, what do they actually deliver? This series explores that question across three projects, each using a different set of AI and automation tools.

## Sub-Projects

| Week | Project | Dataset | AI/Automation Tools |
|------|---------|---------|---------------------|
| w2 | Predicting House Sale Prices | Ames Housing (~2,900 homes, 80+ features) | YData Profiling, SweetViz, Autofeat, Featuretools, PyCaret, SHAP |
| w3 | Customer Sentiment on Twitter | Customer Support on Twitter | Gemini, AutoViz, Hugging Face Transformers |
| w4 | Why Did My Model Predict That? | Heart Disease UCI (920 records) | SHAP, LIME |

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
| SHAP (TreeExplainer) | LIME (LimeTabularExplainer) | High value for both. Both tools agree on top features (exang, oldpeak, chest pain type); SHAP provides global + local views with exact additive values, LIME provides intuitive rule-based local explanations. Complementary, not competing |

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

## W3 Challenge: Sentiment at Scale

The assignment used a 93-row sample. The challenge notebooks tested whether findings hold on the full 2.8M tweet dataset using GPU-accelerated sentiment analysis (Quadro T1000, batch_size=64).

| | W3 (93 rows) | 10K sample | 50K sample |
|---|---|---|---|
| Negative | 55% | 51.6% | 52.1% |
| Neutral | 29% | 33.4% | 32.9% |
| Positive | 16% | 15.0% | 15.1% |
| Confidence | 0.782 | 0.782 | 0.781 |
| Companies (50+ tweets) | 7 | 49 | 113 |

**Key finding:** The sentiment distribution converged by 10K (<0.5pp shift to 50K). Throughput held steady at 51 tweets/sec across both runs, confirming linear scaling. At 50K, 113 companies had sufficient data for statistically meaningful sentiment breakdowns, with negative sentiment ranging from 3.5% to 87.3%.

## W4 Results

| Metric | Random Forest | XGBoost |
|--------|--------------|---------|
| Accuracy | 0.837 | 0.826 |
| AUC | 0.901 | 0.877 |
| Recall | 0.882 | 0.853 |

**Top predictive features (SHAP + LIME agreement):** exercise-induced angina, ST depression (oldpeak), chest pain type, cholesterol, age.

**Key finding:** Both SHAP and LIME agree on which clinical features drive predictions, increasing confidence in the model's reasoning. SHAP provides mathematically grounded global views; LIME provides intuitive rule-based explanations accessible to non-technical stakeholders. In healthcare, explainability is not optional: clinicians need to verify, agree with, or override model reasoning.

## W4 Key Findings

1. **Explainability validates clinical plausibility.** The model's top features (exercise angina, ST depression, chest pain type) are well-established cardiac risk indicators. SHAP confirmed the model learned real clinical patterns, not spurious correlations.
2. **SHAP and LIME are complementary, not competing.** SHAP excels at global model understanding and exact additive contributions. LIME excels at communicating individual decisions through rule-based conditions (e.g., "oldpeak > 1.50"). Use SHAP during development, LIME when explaining to stakeholders.
3. **Wrong predictions need explanations too.** Initial waterfall plots showed misclassified cases where the model's reasoning was clinically plausible but the outcome was incorrect. Understanding why a model is wrong is as valuable as understanding why it is right.

## W4 Conclusions

**What explainability tools delivered:**
SHAP and LIME both identified the same top predictive features for heart disease, providing cross-method validation that the Random Forest model learned clinically meaningful patterns. The waterfall plots showed exactly how the model combined individual patient features to reach each prediction, enabling a clinician to verify the reasoning against actual test results.

**Where explainability tools differ:**
SHAP provides exact additive values that sum to the prediction (base value + contributions = output), making it suitable for model validation and auditing. LIME provides rule-based conditions ("oldpeak > 1.50", "exang <= 0.00") that map directly to clinical thresholds, making it more accessible for patient-facing explanations. The normalized comparison showed strong ranking agreement but different magnitude scales, expected since SHAP averages globally while LIME explains locally.

**The role of human judgment:**
Explainability tools do not replace clinical expertise; they support it. The true negative case (71-year-old female, predicted healthy despite elevated oldpeak) showed the model weighing protective factors against risk factors. A clinician reviewing this explanation might still order additional tests, and that is exactly the right outcome: the model informs the decision without making it.

**The series conclusion:**
Across w2, w3, and w4, AI tools shifted from automation (replacing manual steps) to assistance (augmenting analysis) to accountability (enabling transparency). The consistent finding: the most valuable AI tools are the ones that make human judgment easier, not the ones that try to replace it.

## Project Structure

```
├── notebooks/
│   ├── w2_house_prices.ipynb        # W2 notebook (Colab-compatible)
│   ├── w3_customer_sentiment.ipynb           # W3 notebook (Colab-compatible)
│   ├── w3_customer_sentiment_challenge.ipynb # W3 challenge (10K from full dataset)
│   ├── w3_customer_sentiment_challenge_50k.ipynb # W3 challenge (50K sample)
│   └── w4_explainability.ipynb               # W4 notebook (Colab-compatible)
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
