# W3 Reference Analysis: Understanding Customer Sentiment on Twitter

## Source Materials
- `_reference/w3-.../project-description.txt`
- `_reference/w3-.../class-notes.txt`
- `_reference/Information on Project Evaluation.txt`

## Dataset: Customer Support on Twitter

Conversational dataset of tweets and replies between customers and support agents.

**Columns:**
| Column | Description |
|--------|-------------|
| tweet_id | Unique anonymized tweet ID |
| author_id | Unique anonymized user ID (replaces @mentions) |
| inbound | Whether tweet is inbound to a company (customer → company) |
| created_at | Timestamp |
| text | Tweet content (phone/email masked as `__email__`, `__phone__`) |
| response_tweet_id | IDs of response tweets (comma-separated) |
| in_response_to_tweet_id | ID of parent tweet |

**Dataset characteristics:**
- Real-world customer support conversations (lost luggage, billing, canceled flights)
- Natural, modern language from diverse backgrounds
- Short messages (tweet length constraint)
- Size: unknown until downloaded, likely large (need to verify)

**Download:** Google Drive link in project description

## Required Tasks

### Task 1: Gemini for Initial Exploration
- Upload dataset, ask Gemini to describe structure, summarize key characteristics
- Goal: quickly grasp dataset structure, identify relevant columns

**Our approach:** Use Gemini API (`google-generativeai`) instead of Colab's built-in sidebar. Rationale: reproducible, version-controlled, portable, more technically rigorous. Colab sidebar is an interactive UI tied to a browser session.

**Setup required:** Free API key from Google AI Studio (ai.google.dev, ~2 min, no credit card).

**Prompts to consider:**
- Describe dataset structure, column types, missing values
- Summarize key patterns in the text data
- Identify which columns are most relevant for sentiment analysis
- Suggest analytical directions

### Task 2: AutoViz for Visualization
- Generate automated visualizations to reveal key data insights
- Compare AutoViz output to Gemini's suggestions

**Implementation:**
```python
from autoviz.AutoViz_Class import AutoViz_Class
AV = AutoViz_Class()
AV.AutoViz(filename="", sep=",", depVar="", dfte=df, header=0, verbose=1, lowess=False, chart_format="svg")
```

**Considerations:**
- No natural target variable in the raw data (sentiment is derived later)
- Could use `inbound` as depVar to see how inbound vs outbound tweets differ
- May need to filter/preprocess before feeding to AutoViz (text column won't visualize well)
- No API key needed

### Task 3: Sentiment Analysis with Hugging Face
- Apply HF model to the `text` column
- Use either Transformers library (local) or Inference API

**Our approach:** Local Transformers pipeline (no API key needed).

**Model options:**
| Model | Type | Notes |
|-------|------|-------|
| `distilbert-base-uncased-finetuned-sst-2-english` | Default pipeline model | General sentiment (POSITIVE/NEGATIVE) |
| `cardiffnlp/twitter-roberta-base-sentiment` | Twitter-specific | 3 classes (negative/neutral/positive), trained on ~58M tweets |

**Key decisions:**
- **Which tweets:** Only inbound (customer) tweets for sentiment; company replies are support responses
- **Sampling:** If dataset is large, sample for initial analysis, then scale up
- **Text preprocessing:** Anonymized @mentions and masked emails are already in the data; models should handle these
- **Comparison value:** Run both models to compare general vs Twitter-specific performance

## Required Artifacts

### Technical Artifacts
1. Gemini-generated dataset summaries (API responses)
2. AutoViz visualizations
3. Sentiment analysis outputs (distribution, examples, per-company breakdown)

### Analytical Reasoning
1. How Gemini shaped initial understanding and analytical direction
2. What AutoViz revealed beyond text summaries
3. How sentiment results complemented or challenged earlier insights

### Reflections on AI-Assisted Analysis
1. Where AI tools accelerated exploration or reduced manual effort
2. Where human interpretation was still essential
3. Strengths and limitations of LLMs for sentiment analysis

## Presentation Points
- Analytical goal of the project
- How Gemini, AutoViz, and HF were used
- Key sentiment insights uncovered
- What was learned about AI for text-based analysis
- Focus: interpretation and productivity gains, not implementation details

## Quality Standards
- AI tools used purposefully, not superficially
- Outputs clearly interpreted and contextualized
- Reflections show understanding of both data and tools
- Insights grounded in evidence from the analysis

## Narrative Arc Connection
W3 is "AI-assisted analysis" in the series arc. The critical question: can Gemini, AutoViz, and Hugging Face accelerate text and sentiment analysis? Where do they add real value vs where is human judgment still needed? This connects to W2's finding that automation value is uneven.

## Risks and Mitigations
| Risk | Mitigation |
|------|------------|
| Dataset too large for full sentiment analysis | Sample first, scale if time allows |
| Gemini API rate limits | Free tier is 15 RPM (Flash); send concise prompts, cache responses |
| AutoViz struggles with text-heavy data | Filter to numeric/categorical columns, or preprocess text features |
| Sentiment model accuracy on support tweets | Compare two models; manually validate a sample |
| 2-hour budget | Phase-based plan, prioritize required artifacts |