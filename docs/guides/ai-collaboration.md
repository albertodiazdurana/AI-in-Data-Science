# AI Collaboration Guide

Reference: DSM_3 Section 6.7.3

## Notebook Workflow

The agent generates one cell at a time. The user runs each cell, shares output, and the agent generates the next based on actual results. This iterative loop catches errors early and lets data drive decisions.

## Pre-Generation Brief

Before generating any code cell, the agent explains what the cell will do, why, and its expected structure. The user approves before code is written. This prevents wasted cells and keeps the user in control of direction.

## Cell Delivery

Cells are presented as code blocks with copy buttons. The user pastes them into the notebook manually. This avoids cell ordering issues and gives full control over placement.

## DataFrame Output

Always use `print()` for DataFrame display. Bare expressions produce rich HTML output that is not copy/paste friendly for sharing results back to the agent.

## Session Transcript

The agent appends reasoning to `.claude/session-transcript.md` before acting and output summaries after completing work. Conversation text contains results, summaries, and questions only. The user can monitor the transcript in real time.

## Cross-Repo Protocols

**README Change Notification:** When `README.md` is modified, evaluate relevance (DSM_0.2 filter: project description, scope, audience, external metrics, structure, license, contact). If externally relevant, send inbox entry to the portfolio project and to DSM Central. Internal-only changes (version bumps, formatting) are logged but not sent.

## Lessons from W2

- Automated tool output is reference material, not analysis. Write focused code that asks specific questions of the data.
- Plan at the abstraction level matching current certainty. Phase-based objectives over cell-by-cell prescriptions.
- When a tool produces more output than you can process, take a bite, not the whole plate.
