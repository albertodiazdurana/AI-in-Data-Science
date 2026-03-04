# D001: PyCaret as Primary AutoML Tool

**Date:** 2026-03-04
**Status:** Accepted
**Context:** W2 — Predicting House Sale Prices

## Decision

Use PyCaret as the primary AutoML tool instead of H2O AutoML.

## Rationale

- Simpler API: 3 lines from data to leaderboard vs. manual H2OFrame conversion and column specification
- Built-in preprocessing (imputation, encoding, scaling) — H2O requires manual prep
- Presentation-ready output: styled comparison table with multiple metrics
- Native SHAP integration via `interpret_model()` — H2O requires separate SHAP code or manual Explanation construction
- Lower resource overhead: no JVM process, no memory allocation concerns
- Better fit for the 2-hour time budget

## Alternatives Considered

- **H2O AutoML:** More powerful stacked ensembles, but heavier setup, JVM dependency, and more manual work. Could be included as optional secondary run if time permits.

## Research

Based on parallel agent research comparing both tools across setup, API, memory, output quality, and known issues. Full details in `docs/research/w2/tool-research.md`.
