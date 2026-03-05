# W3 Challenge: Full Customer Support on Twitter Dataset

**Status:** Deferred (after w4 completion)
**Notebook:** `notebooks/w3_customer_sentiment_challenge.ipynb`
**Dataset:** Full Kaggle dataset (~2.8M tweets, ~500MB CSV)
**Source:** `kaggle datasets download -d thoughtvector/customer-support-on-twitter`

## What Makes This Different from W3

The assignment notebook uses a 93-row course-provided sample. This challenge notebook uses the full dataset, which changes the analysis in three ways: scale forces sampling decisions, GPU acceleration becomes meaningful, and patterns that are invisible in 93 rows become statistically significant.

## Hardware

- RAM: 31GB (dataset loads as ~1.5GB DataFrame)
- GPU: Quadro T1000 4GB VRAM, CUDA 12.8, PyTorch cu128
- Disk: 866GB free

## Phase 1: Setup and Data Loading

- Download via Kaggle CLI or kagglehub (requires Kaggle credentials)
- Local path: `data/twcs_full.csv`
- Full inspection: shape, dtypes, missing values, inbound/outbound split
- Expect ~2.8M rows, ~1.4M inbound

## Phase 2: Gemini Exploration

Same approach as w3: send statistics + sample rows to Gemini API. The larger dataset means richer statistics (more companies, wider time range, more patterns to identify).

## Phase 3: AutoViz Visualization

- Sample 10K-20K rows for AutoViz (full dataset will crash or hang)
- Use `inbound` as depVar
- Compare to Gemini output (same deliverable as w3)

## Phase 4: Sentiment Analysis (GPU-Accelerated)

- Filter to inbound tweets only (~1.4M)
- Run on stratified sample of 10K-20K tweets with GPU batching (batch_size=64-128)
- Compare distilbert default vs cardiffnlp/twitter-roberta-base-sentiment
- Sentiment by company (statistically meaningful with full dataset)
- Manual validation on a random sample of 50-100 tweets

## Phase 5: Synthesis

- Scale impact: what changes when you go from 93 to 2.8M rows?
- GPU vs CPU comparison (timing)
- Company-level sentiment patterns (enough data for per-company breakdowns)
- Same reflection structure as w3

## Prerequisites

- [ ] Kaggle account + API token (`~/.kaggle/kaggle.json`)
- [ ] Gemini API key (reuse from w3)
- [ ] W3 and W4 assignment notebooks complete