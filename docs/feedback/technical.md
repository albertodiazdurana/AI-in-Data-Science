# Technical Progress Reports

Sprint-level technical progress for this project.
Reference: DSM_0.2 (Technical Progress Reporting).

## W3: Customer Sentiment on Twitter (Complete)
**Pushed:** 2026-03-05

**Notebook:** `notebooks/w3_customer_sentiment.ipynb`
**Status:** All 8 cells + 5 markdown sections complete, Colab-compatible

### Tool Stack Results
| Tool | Phase | Value | Notes |
|------|-------|-------|-------|
| Gemini API (2.5-flash) | Exploration | Moderate | Generic but useful orientation; free tier quota=0 in Brazil, required billing |
| AutoViz | Visualization | Low | Poor fit for text data; data quality report was the only useful output |
| HF Transformers (RoBERTa) | Sentiment | High | 49 tweets classified, 0.78 mean confidence, no labeled data needed |

### Environment Issues Encountered
- Gemini free tier: `limit: 0` for all models in Brazil region; required billing setup with CPF
- Gemini model deprecation: `gemini-2.0-flash` and `gemini-2.0-flash-lite` both deprecated for new projects; switched to `gemini-2.5-flash`
- AutoViz import: IPython/matplotlib `warnings.warn` incompatibility; fixed with monkey-patch on `IPython.core.pylabtools.backend2gui`
- AutoViz execution: `pandas_dq` calls deprecated `DataFrame.applymap`; fixed by suppressing `warnings.warn` for entire cell

### Key Metrics
- Sentiment: 55% negative, 29% neutral, 16% positive
- Confidence: negative 0.83, neutral 0.72, positive 0.75
- Company patterns: AppleSupport 78% negative, SpotifyCares 57% positive

## W4: Model Explainability (Complete)
**Pushed:** 2026-03-05

**Notebook:** `notebooks/w4_explainability.ipynb`
**Status:** 7 code cells + 5 markdown sections complete

### Tool Stack Results
| Tool | Phase | Value | Notes |
|------|-------|-------|-------|
| SHAP TreeExplainer | Global + Local | High | Beeswarm, bar, waterfall plots; top features match clinical literature |
| LIME TabularExplainer | Local | High | Rule-based explanations; intuitive for non-technical audiences |

### Key Metrics
- RF accuracy: 0.837, AUC: 0.901, Recall: 0.882
- Top features (cross-method): exang, oldpeak, chest pain type
- SHAP + LIME ranking agreement confirmed via normalized comparison

## W3 Challenge: Sentiment at Scale (Complete)
**Pushed:** 2026-03-05

**Notebooks:** `notebooks/w3_customer_sentiment_challenge.ipynb` (10K), `*_50k.ipynb` (50K)

### Scale Results
| Sample | Negative | Neutral | Positive | Confidence | Companies (50+) |
|--------|----------|---------|----------|------------|-----------------|
| 10K | 51.6% | 33.4% | 15.0% | 0.782 | 49 |
| 50K | 52.1% | 32.9% | 15.1% | 0.781 | 113 |

### Key Metrics
- GPU throughput: 51 tweets/sec (Quadro T1000, batch_size=64)
- Distribution converged by 10K (<0.5pp shift to 50K)
- Estimated full corpus (1.5M inbound): ~504 min on GPU
