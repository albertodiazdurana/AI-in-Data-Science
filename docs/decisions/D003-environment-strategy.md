# D003: Local Jupyter with Colab Compatibility

**Date:** 2026-03-04
**Status:** Accepted
**Context:** All sub-projects (w2, w3, w4)

## Decision

Develop locally in Jupyter notebooks with Colab-compatible cells. Use the pattern from the disaster-tweets reference notebook.

## Rationale

- Local development provides a stable, controllable environment
- Colab compatibility ensures notebooks can be submitted and run by evaluators
- Reference pattern (`subprocess.check_call` for installs, local-first data loading with remote fallback, `os.makedirs` for output dirs) is proven

## Implementation

- Installs via `subprocess.check_call([sys.executable, '-m', 'pip', 'install', '-q', ...])` — works in both environments
- Data loading: check local path first, fall back to Google Drive download
- Output directories: `os.makedirs('../outputs/', exist_ok=True)`
- Local venv + `requirements.txt` for local reproducibility (Colab users get installs from notebook cells)
