# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository layout

- This is the AIOT course final project, **Smart Hydration Monitoring Agent**. It is a fully standalone git repo (branch `main`, remote `origin` = `github.com/AngJeng/AIOT_final_project`). Its sibling folders under `/Users/anzheng/Agent_testing` (`L1/`, `L3/`, `L6/`) are unrelated, finished course projects. Do not read or modify them.
- The project is at the **proposal stage**, with no source code yet. The repo holds the proposal docs and an OpenSpec change:
  - `docs/01`–`04`: the A–F abstract (5 cores / 3 trade-offs / 10 details), the 300-character and IEEE abstracts, the Introduction, and Related Work. `docs/references.md` holds the citation numbers used in all of them.
  - `DESIGN.md`: architecture, data, algorithms and implementation mapping (kept up to date as code lands).
  - `openspec/changes/smart-hydration-monitoring-agent/`: `proposal.md`, `design.md`, `tasks.md`, and 7 capability spec deltas under `specs/`.
  - The proposal slide deck is the PDF in the repo root.

## Working rules (from `openspec/config.yaml`)

- Spec-driven: change behavior in the spec first, then `DESIGN.md`, then code. Flow: proposal → specs → design → tasks → apply → archive.
- Never invent datasets, accuracy, satisfaction, cost or citations. Label untested numbers as Planned / Expected / Illustrative Example, and write uncalibrated thresholds as configurable parameters.
- No medical diagnosis or dehydration claims. The LLM only drafts notification text; volume calculation, event detection and guardrails stay deterministic or ML.
- Write docs in Traditional Chinese and keep technical terms in English.

## Commands

OpenSpec 2.0.0 (`npm install -g @fission-ai/openspec@2.0.0`):

```bash
openspec validate smart-hydration-monitoring-agent --strict
openspec show smart-hydration-monitoring-agent
openspec list
```

No build, lint or test tooling exists yet. Update this file when it is added.
