# W3 Plan: Understanding Customer Sentiment on Twitter

**Budget:** 2 hours
**Notebook:** `notebooks/w3_customer_sentiment.ipynb`
**Dataset:** Customer Support on Twitter (Google Drive)

## Prerequisites
- [ ] Gemini API key from Google AI Studio (ai.google.dev, free, ~2 min setup)

## Phase 1: Setup and Data Loading
**Objective:** Install dependencies, load dataset, initial inspection.

**Packages:** google-generativeai, autoviz, transformers, torch, pandas, matplotlib, seaborn

**Data loading:** Local path first (`data/twcs.csv`), otherwise download from Google Drive via gdown (file ID: `1dvTN_tIwqd6eqncyWGzpTN788klfhKpR`). Print shape, dtypes, sample rows, missing values.

**Key first look:**
- How many tweets total?
- What fraction are inbound (customer) vs outbound (company)?
- Which companies appear most frequently?
- How much text is missing or empty?

## Phase 2: Gemini Exploration
**Objective:** Use Gemini API to generate an automated dataset summary and analytical suggestions.

**Approach:**
- Send dataset statistics and sample rows to Gemini (not the full dataset, it's too large for a prompt)
- Ask Gemini to: describe structure, identify patterns, suggest analytical directions for sentiment analysis
- Capture and display the API response in the notebook
- Evaluate: what did Gemini get right? What did it miss? What required human correction?

**API key handling:** Load from environment variable (`GOOGLE_API_KEY`) or a `.env` file. Never hardcode in the notebook.

## Phase 3: AutoViz Visualization
**Objective:** Generate automated visualizations to reveal patterns Gemini's text summary might miss.

**Approach:**
- Filter or prepare data for AutoViz (text columns don't visualize well)
- Consider using `inbound` as depVar to compare customer vs company tweet patterns
- Run AutoViz, review generated plots
- **Key deliverable:** Compare AutoViz outputs to Gemini's suggestions (required by project description). What did the visual analysis reveal that the text summary missed? What overlaps?

**Note:** AutoViz may struggle with very large datasets. Sample if needed.

## Phase 4: Sentiment Analysis
**Objective:** Classify customer tweet sentiment using Hugging Face Transformers.

**Approach:**
- Filter to inbound (customer) tweets only
- Run sentiment pipeline on tweet text (sample first for speed, then scale)
- Model comparison: default distilbert vs Twitter-specific cardiffnlp model
- Analyze results: sentiment distribution, sentiment by company, example tweets per category
- Manual validation: spot-check a sample of classifications for accuracy

**Performance consideration:** If dataset has >100K tweets, run on a sample (e.g., 5K-10K) first, then decide whether to scale.

## Phase 5: Synthesis and Reflections
**Objective:** Connect findings across all three tools, reflect on AI-assisted analysis.

**Deliverables:**
- How each tool contributed to understanding (Gemini: structure, AutoViz: visual patterns, HF: sentiment classification)
- Where AI tools accelerated the work vs where human judgment was needed
- Strengths and limitations of using LLMs for sentiment on support tweets
- Connection to W2: how does AI-assisted analysis compare to ML automation?

## Tool Decisions

| Stage | Tool | Rationale |
|-------|------|-----------|
| Exploration | Gemini API (`google-generativeai`) | Reproducible, version-controlled; Colab sidebar is not portable |
| Visualization | AutoViz | Required by project; no API key needed |
| Sentiment | HF Transformers (local pipeline) | No API key needed; runs locally; compare default vs Twitter model |

## Success Criteria
- All three tools used purposefully with interpreted outputs
- Sentiment distribution analyzed and contextualized
- Reflections demonstrate understanding, not just tool execution
- Notebook is reproducible and Colab-compatible