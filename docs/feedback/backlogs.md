# Backlog Proposals

DSM improvement proposals from this project.
Reference: DSM_0.2 (DSM Feedback Tracking).

## From W3

### BACKLOG: Lighter transcript protocol for notebook-heavy sessions
**Context:** W3 had ~20 turns of short notebook iterations (provide cell, run, review, next). The transcript-before-acting rule added a Bash append call to every turn, which felt heavy for turns that were essentially "here's the next cell."
**Proposal:** Allow a batch-transcript mode for notebook sessions where reasoning is logged every N turns or at phase boundaries, not every single turn.

### BACKLOG: Gemini API region-specific documentation
**Context:** Free tier quota was `limit: 0` for all Gemini models in Brazil. No error message indicated this was region-specific; the retry delay suggested a transient issue. Debugging took multiple model switches before identifying the root cause.
**Proposal:** When API errors include quota limits, check if the limit is 0 (disabled) vs exhausted (was >0), and surface region as a possible factor in troubleshooting guidance.
