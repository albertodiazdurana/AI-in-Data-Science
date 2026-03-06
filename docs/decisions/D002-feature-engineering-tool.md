# D002: Autofeat as Primary Feature Engineering Tool

**Date:** 2026-03-04
**Status:** Accepted
**Context:** W2 — Predicting House Sale Prices

## Decision

Use Autofeat as the primary feature engineering tool, with a brief Featuretools demonstration for contrast.

## Rationale

- Autofeat is designed for flat numeric data (the Ames Housing structure)
- Built-in L1-regularized feature selection — outputs a curated set, not a raw explosion
- Produces interpretable symbolic formulas (e.g., `log(GrLivArea) * OverallQual`)
- Featuretools on a single table is limited to transform primitives only (no aggregation without relationships), producing thousands of unselected features

## Constraints

- Must use `feateng_steps=1` and limit to 10-15 columns to avoid combinatorial runtime explosion
- `feateng_steps=2` with 30 cols generated 16,141 candidates and 35+ min runtime

## Alternatives Considered

- **Featuretools only:** Designed for relational/multi-table data; limited value on flat table. Included briefly to demonstrate awareness and provide reflective contrast.

## Research

Empirically tested on local machine. Full details in `docs/research/w2/tool-research.md`.
