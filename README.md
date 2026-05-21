# AI Contract Redline & Negotiation Engine
### Agentic AI Portfolio Project | Claude (OpenRouter) + Google Sheets

## What It Does
Analyses enterprise contracts using a 4-agent sequential AI pipeline.
Upload any contract — MSA, NDA, SOW, DPA — and receive a complete
redline package with risk scores, counter-proposal language, and
negotiation strategy in under 90 seconds.

## Stack
- AI Agents: Claude Haiku via OpenRouter API
- Orchestration: Browser-based (HTML/JS) + Make.com blueprint
- Dashboard: Google Sheets
- Input: Paste contract text or upload via browser UI

## The 4-Agent Pipeline

| Agent | Role | Output |
|-------|------|--------|
| Agent 1 | Clause Extractor | Maps every significant clause with ID, type, exact text |
| Agent 2 | Risk Classifier | Scores each clause 0-100 for enterprise risk exposure |
| Agent 3 | Negotiation Strategist | Generates counter-proposal replacement language |
| Agent 4 | Redline Orchestrator | Synthesises final redline package + executive summary |

## Risk Scoring & Approval Status

| Score | Tier | Status |
|-------|------|--------|
| 0–24 | LOW | APPROVED TO SIGN |
| 25–49 | MEDIUM | APPROVED WITH REDLINES |
| 50–74 | HIGH | REQUIRES NEGOTIATION |
| 75–100 | CRITICAL | DO NOT SIGN |

## Why Sequential Architecture (Not Parallel)
Unlike the Vendor Risk Engine, contract redlining has strict dependencies:
- You cannot score risk before extracting clauses
- You cannot write counter-proposals before scoring risk
- You cannot synthesise before all three are complete

The sequential chain enforces logical dependency explicitly — making
the system more reliable than a single-prompt approach.

## How to Run the Demo
1. Download app/contract-redline-engine.html
2. Open in Notepad → find YOUR_OPENROUTER_KEY_HERE
3. Replace with your sk-or-... key from openrouter.ai
4. Save → open in Chrome (drag and drop or Ctrl+O)
5. Click Load Demo Contract → Run Contract Analysis
6. Wait ~75 seconds for all 4 agents to complete

## Google Sheets Dashboard Setup
See schemas/google-sheets-column-map.md for exact column configuration.

## Portfolio Defense Points
1. Dependency-aware sequential architecture prevents logical errors
2. Clause IDs (C1, C2...) create a shared reference key across all agents
3. Structured JSON output maps directly to Sheets — no middleware needed
