# Technical Progress Reports

Sprint-level technical progress for this project.
Reference: DSM_0.2 (Technical Progress Reporting).

## W3: Customer Sentiment on Twitter (Complete)

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
