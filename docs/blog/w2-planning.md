# Planning an Automation-Driven ML Project: Tool Research Before Code

*Part 1 of a 3-part series on AI-enhanced productivity in data science*

---

When the assignment said "automate as much of the machine learning workflow as possible," the temptation was to jump straight into code. Install the libraries, load the data, run the tools. But the most valuable hour I spent on this project was the one where I wrote zero code.

## The Setup

The task: predict house sale prices using the Ames Housing dataset (~2,900 homes, 80+ features). The twist: use automation tools at every stage — automated EDA, automated feature engineering, automated model training — and reflect on what automation actually delivers.

The tool stack from the course: YData Profiling and SweetViz for EDA, Featuretools and Autofeat for feature engineering, H2O AutoML and PyCaret for modeling. That is six tools for one project. The question was not *can* I use all of them, but *should* I?

## Research Before Assumptions

I ran parallel research on each tool pair, focusing on practical fit rather than feature lists:

**EDA tools (YData Profiling vs SweetViz):** These are genuinely complementary. YData Profiling provides deep statistical profiles and data quality alerts. SweetViz excels at visual target analysis — showing how each feature relates to SalePrice. Using both adds value without adding complexity. Decision: use both.

**Feature engineering (Autofeat vs Featuretools):** This is where research saved me from a painful mistake. Featuretools is designed for relational, multi-table data. On a single flat table, it is limited to basic transforms and produces thousands of features with no built-in selection. Autofeat, by contrast, is built for exactly this scenario — flat numeric data — and includes L1-regularized feature selection that outputs a curated set of interpretable formulas. But there is a critical gotcha: with `feateng_steps=2` and 30 columns, Autofeat generates over 16,000 candidate features and takes 35+ minutes. The safe path is `feateng_steps=1` with 10-15 carefully chosen columns.

**AutoML (H2O vs PyCaret):** PyCaret wins on practicality for a time-boxed project. Three lines to a presentation-ready leaderboard. Built-in preprocessing. Native SHAP integration. H2O is more powerful (automatic stacked ensembles), but the JVM overhead, manual data conversion, and separate SHAP setup cost time without proportional benefit on a 1,500-row dataset.

## The Value of a Baseline

The project description asked for automated modeling, not a comparison. But adding a simple sklearn LinearRegression as a baseline — five lines of code — creates the anchor that makes the entire automation narrative meaningful. Without it, "the best PyCaret model achieved R² = 0.91" is a number. With it, "automation improved R² from 0.74 (manual baseline) to 0.91" is a story about productivity.

## What I Learned Before Writing a Single Cell

1. **Tool fit matters more than tool count.** Featuretools on flat data is a mismatch, not a demonstration of skill.
2. **Defaults can be dangerous.** Autofeat's default settings would have consumed my entire time budget on feature generation alone.
3. **Plan at the right abstraction level.** A phase-based plan with objectives is more useful than a 20-cell outline that pre-commits to implementation details before the data has been explored. The specifics should emerge from execution, not precede it.

Next: the notebook itself — from data loading to reflections.

---

## Part 2 TODO (deferred from session 2)

Material to cover:
- Automated EDA generated too much output; focused cells were more actionable ("take a bite" on tool output)
- Autofeat added R²=+0.004, PyCaret model selection added +0.12 (tool selection > tool quantity)
- Real numbers: baseline R²=0.80, PyCaret GBR tuned R²=0.93
- The plan changed mid-session: cell-by-cell v2 was replaced by phase-based v3 because data should drive decisions
- Pre-generation briefs: findings ≠ implementation approach
