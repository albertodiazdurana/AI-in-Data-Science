# Planning an Automation-Driven ML Project: Tool Research Before Code

*Part 1 of the AI-enhanced productivity series: Automation, Sentiment, and Explainability*

---

When the assignment said "automate as much of the machine learning workflow as possible," the temptation was to jump straight into code. Install the libraries, load the data, run the tools. But the most valuable hour I spent on this project was the one where I wrote zero code.

## The Setup

The task: predict house sale prices using the Ames Housing dataset (~2,900 homes, 80+ features). The twist: use automation tools at every stage — automated EDA, automated feature engineering, automated model training — and reflect on what automation actually delivers.

The tool stack from the course: YData Profiling and SweetViz for EDA, Featuretools and Autofeat for feature engineering, H2O AutoML and PyCaret for modeling. That is six tools for one project. The question was not *can* I use all of them, but *should* I?

## Research Before Assumptions

I ran parallel research on each tool pair, focusing on practical fit rather than feature lists:

**EDA tools (YData Profiling vs SweetViz):** These are genuinely complementary. YData Profiling provides deep statistical profiles and data quality alerts. SweetViz excels at visual target analysis — showing how each feature relates to SalePrice. Using both adds value without adding complexity. Decision: use both.

**Feature engineering (Autofeat vs Featuretools):** This is where research saved me from a painful mistake. Featuretools is designed for relational, multi-table data. On a single flat table, it is limited to basic transforms and produces thousands of features with no built-in selection. Autofeat, by contrast, is built for exactly this scenario — flat numeric data — and includes L1-regularized feature selection that outputs a curated set of interpretable formulas. But there is a critical gotcha: with `feateng_steps=2` and 30 columns, Autofeat generates over 16,000 candidate features and takes 35+ minutes. The safe path is `feateng_steps=1` with 10-15 carefully chosen columns.

**AutoML (H2O vs PyCaret):** PyCaret wins on practicality for a time-boxed project. Three lines to a presentation-ready leaderboard. Built-in preprocessing. Native SHAP integration. H2O is more powerful (automatic stacked ensembles), but the JVM overhead, manual data conversion, and separate SHAP setup cost time without proportional benefit on a ~2,900-row dataset.

## The Value of a Baseline

The project description asked for automated modeling, not a comparison. But adding a simple sklearn LinearRegression as a baseline — five lines of code — creates the anchor that makes the entire automation narrative meaningful. Without it, "the best PyCaret model achieved R² = 0.93" is a number. With it, "automation improved R² from 0.80 (manual baseline) to 0.93" is a story about productivity.

## What I Learned Before Writing a Single Cell

1. **Tool fit matters more than tool count.** Featuretools on flat data is a mismatch, not a demonstration of skill.
2. **Defaults can be dangerous.** Autofeat's default settings would have consumed my entire time budget on feature generation alone.
3. **Plan at the right abstraction level.** A phase-based plan with objectives is more useful than a 20-cell outline that pre-commits to implementation details before the data has been explored. The specifics should emerge from execution, not precede it.

Next: the notebook itself — from data loading to reflections.

---

## Part 2: What the Notebook Actually Taught Me

The plan survived first contact with the data for about twenty minutes.

### The Information Dump Problem

YData Profiling generated 40 alerts across 82 features. SweetViz produced an HTML report with distribution plots, association scores, and per-feature breakdowns for every column. Both tools ran in under a minute. Both produced far more output than any person could meaningfully analyze in the time available.

This is a pattern worth naming: automation without restraint. The tools work exactly as advertised, the problem is that comprehensive output is not the same as useful output. Scanning 82 feature profiles to find the three that matter is not analysis, it is reading.

The fix was simple. Instead of trying to parse the HTML reports, I wrote three focused cells: one for missing values grouped by imputation strategy, one for correlations with anomaly flags, one for near-zero-variance features. Each produced a short, actionable summary. The HTML reports stayed available as reference, but the actual decisions came from targeted code that asked specific questions of the data.

The lesson applies beyond EDA. When a tool produces more output than you can process, the tool is not the analysis; it is raw material. Take a bite, not the whole plate.

### Where the Gains Actually Were

The three-way model comparison told a clear story:

| Approach | R² | RMSE |
|----------|-----|------|
| Baseline LinearRegression (10 numeric features) | 0.80 | $39,568 |
| Autofeat LinearRegression (10 original + 10 engineered) | 0.81 | $39,197 |
| PyCaret GradientBoosting (tuned, full feature set) | 0.93 | $24,088 |

Autofeat's feature engineering added R² = +0.004 to the linear model. PyCaret's model selection added R² = +0.12. The entire feature engineering phase, two tools and multiple cells, contributed less than switching from LinearRegression to GradientBoosting with the right preprocessing.

This is not an argument against feature engineering. It is an observation about where to invest time when the budget is fixed. Tool selection matters more than tool quantity.

### The Plan That Changed

The original plan was a 20-cell outline, each cell specified before the data had been loaded. By the time the EDA was done, the plan was already wrong: preprocessing decisions depended on findings that did not exist when the plan was written.

The rewrite was straightforward. Replace cell-by-cell prescriptions with phase-based objectives: "identify and handle missing values based on EDA findings" instead of "Cell 8: drop columns with >50% missing." The phases stayed the same, but each one was free to adapt to what the previous phase revealed.

This is not a data-science-specific principle. It is a general one: plan at the abstraction level that matches your current certainty. Commit to goals early, commit to implementation details late.

### What Worked

The iterative workflow, one cell at a time with review between each, caught three API errors in PyCaret before they could compound. The pre-generation brief protocol, explaining what a cell would do before writing it, prevented at least two cells that would have been wasted effort. And the baseline comparison turned raw R² scores into a narrative about automation's actual contribution.

The notebook is done. The presentation story is not "I used six tools," it is "I used the right tools, measured what each one contributed, and found that the biggest productivity gain came from automated model selection, not from running more automation."

Next: sentiment analysis with a different set of tools, and the same question about what automation actually delivers.
