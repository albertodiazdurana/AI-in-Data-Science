# D005: Virtual Environment + requirements.txt

**Date:** 2026-03-04
**Status:** Accepted
**Context:** All sub-projects

## Decision

Use a single `.venv` virtual environment for the project with a `requirements.txt` for dependency tracking.

## Rationale

- Each project has a unique combination of libraries that may conflict with system packages
- `requirements.txt` ensures reproducibility
- Single venv for all 3 sub-projects since they share a Jupyter kernel
- `pip freeze > requirements.txt` after installing all dependencies
- Install order matters: autofeat first (downgrades numpy to 1.26.4 for numba), then remaining packages

## Alternatives Considered

- **Poetry:** Overkill for a notebook-based project with no package publishing
- **Conda:** Heavier, less portable for Colab compatibility
- **Per-project venvs:** Unnecessary since all 3 sub-projects run in the same repo
