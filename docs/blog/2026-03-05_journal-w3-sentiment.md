# AI-Assisted Sentiment Analysis: What Gemini, AutoViz, and Hugging Face Actually Deliver

*Part 3 of a 3-part series on AI-enhanced productivity in data science*

---

The W2 project was about automation: can tools replace manual EDA, feature engineering, and model selection? The W3 project asks a different question: can AI tools accelerate analysis on text data, where traditional automation has less to offer?

## The Setup

The dataset: 93 customer support tweets, a curated sample from the larger Customer Support on Twitter corpus. The tool stack from the course: Gemini for exploration, AutoViz for visualization, Hugging Face Transformers for sentiment analysis. Three tools, each aimed at a different stage of the workflow.

The 93-row sample was a surprise. The original dataset contains 2.8 million tweets; the course version is a hand-picked subset. This limits generalizability but keeps the focus on tool evaluation rather than scale engineering.

## Gemini: Orientation Without Code

The first tool, Gemini's API, received a structured summary of the dataset: shape, column types, missing values, sample tweets from both customers and companies. In a single prompt, it returned a detailed analysis covering dataset structure, patterns (conversation threading, inbound/outbound segmentation), and analytical directions.

The output was competent and thorough. It correctly identified that `in_response_to_tweet_id` being float64 indicates NaN values, that the `created_at` column needs datetime conversion, and that the inbound/outbound split enables clean segmentation. It suggested VADER and TextBlob for sentiment, which are reasonable but not what we used.

The value is real but narrow. Gemini replaced the initial "what is this data?" phase that normally takes 15-20 minutes of pandas exploration. On a familiar dataset shape, this acceleration is modest. On an unfamiliar dataset with dozens of columns and unclear semantics, the time savings would be more significant.

The limitation: everything Gemini suggested was generic enough to apply to any customer support dataset. It did not surface anything surprising or counterintuitive. The tool works best as a starting accelerator, not as a source of domain-specific insight.

## AutoViz: Wrong Tool for the Job

AutoViz classified the columns, generated scatter plots, and flagged three duplicate rows, all in one second. But with a text-heavy dataset containing only two derived numeric features (text length and word count), there was little for it to visualize.

The data quality report was useful: duplicate detection, outlier flagging, correlation warnings. But this is table-stakes data profiling, not visualization insight. AutoViz is built for tabular datasets with many numeric features; on text data, it confirms what you already know rather than revealing something new.

The honest assessment: for this dataset, AutoViz added less value than the five minutes it took to set up and debug its compatibility issues with IPython and matplotlib.

## Hugging Face: The Core Analytical Value

The sentiment analysis pipeline was the payoff. A single function call loaded a RoBERTa model fine-tuned specifically on tweets (`cardiffnlp/twitter-roberta-base-sentiment-latest`), classified all 49 inbound customer tweets into negative, neutral, and positive categories, and returned confidence scores.

The results:

| Sentiment | Count | Share | Mean Confidence |
|-----------|-------|-------|-----------------|
| Negative  | 27    | 55%   | 0.83            |
| Neutral   | 14    | 29%   | 0.72            |
| Positive  | 8     | 16%   | 0.75            |

The distribution makes sense: people contact support when something is wrong. The model's highest confidence is on negative tweets (0.83 mean), suggesting complaints are linguistically more distinct than neutral or positive messages. Spot-checking confirmed the labels: iOS slowness complaints scored 0.91-0.96 negative, while "Thanks!" replies to Spotify scored positive.

At the company level, AppleSupport received the most negative sentiment (7 of 9 tweets), while SpotifyCares was the only company with a positive lean (4 of 7). Tesco's interactions were predominantly neutral (5 of 8), suggesting factual exchanges rather than emotional complaints.

Preprocessing was minimal: only @mention removal. The model handles tweet-style text natively, abbreviations, hashtags, informal language, all without tokenization, stemming, or stop-word removal. This is a significant productivity advantage over classical NLP pipelines that require extensive text preprocessing before any analysis can begin.

## The Environment Tax

The biggest time investment was not analysis but infrastructure. The Gemini API key required creating a new Google Cloud project and enabling billing because the free tier quota was zero in the region. Model names changed between SDK versions (`gemini-2.0-flash` deprecated for new users, requiring `gemini-2.5-flash`). AutoViz's import chain triggered an IPython/matplotlib compatibility bug that needed a monkey-patch.

None of these issues are about the tools' analytical capabilities. They are about the cost of integrating multiple AI services into a single workflow. Each tool works well in isolation; combining them multiplies the surface area for version conflicts, API changes, and environment mismatches.

## What This Project Demonstrates

The three tools form a clear value gradient:

1. **Hugging Face Transformers (high value):** Pre-trained, domain-specific model that delivers production-quality sentiment classification with no labeled data and minimal preprocessing. This is automation that directly produces analytical results.

2. **Gemini (moderate value):** Useful for initial orientation on unfamiliar data. The time savings are real but modest on small, well-structured datasets. More valuable as dataset complexity increases.

3. **AutoViz (low value for text data):** Built for tabular numeric data. On a text-heavy dataset, it functions as a basic data quality checker rather than a visualization tool.

The W2 lesson was that model selection automation delivers more value than feature engineering automation. The W3 lesson is similar: the tool closest to the analytical goal (sentiment classification) delivers the most value. Tools aimed at generic exploration (Gemini, AutoViz) help, but their contribution is smaller and more substitutable.

## What Comes Next: The Challenge Notebook

This analysis used a 93-row sample and a single pre-trained model. The planned challenge notebook will work with the full 2.8 million tweet corpus from Kaggle and introduce a comparison that this small sample could not support.

In a [previous project on disaster tweet classification](https://github.com/albertodiazdurana/tfidf-to-transformers-with-disaster-tweets/blob/main/docs/blog-post-draft.md), I found that TF-IDF with Logistic Regression nearly matched Sentence Transformers (F1 = 0.764 vs 0.770), a result that challenged assumptions about which tools are worth the complexity. The challenge notebook will apply the same thinking to sentiment: compare the pre-trained Hugging Face pipeline against a TF-IDF + ML classifier trained on labeled sentiment data. If the pattern holds, a simple bag-of-words model with proper feature engineering may perform surprisingly well against a transformer, especially when the task is classification rather than comprehension.

The question is whether the convenience of a zero-label pre-trained pipeline justifies its use when a labeled training set is available and a classical approach might match or exceed its accuracy. Scale will also matter: processing 2.8 million tweets with a transformer model requires GPU batching and memory management that TF-IDF sidesteps entirely. The challenge notebook is where tool evaluation meets engineering tradeoffs.

Next: explainability with SHAP and LIME, and the same question about what these tools reveal versus what requires human interpretation.