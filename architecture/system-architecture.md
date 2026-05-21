# Contract Redline Engine — System Architecture

\`\`\`
╔══════════════════════════════════════════════════════════════════╗
║           AI CONTRACT REDLINE & NEGOTIATION ENGINE              ║
║                   End-to-End Architecture                       ║
╚══════════════════════════════════════════════════════════════════╝

  ┌──────────────────────────┐
  │     INGESTION LAYER      │
  │  Contract Upload via     │
  │  Browser UI              │
  │  (PDF / DOCX / TXT)      │
  └────────────┬─────────────┘
               │ INPUT: raw contract text
               ▼
  ┌──────────────────────────┐
  │    CONTEXT PACKAGER      │
  │  · contract_text         │
  │  · contract_type         │
  │  · party_name            │
  │  · timestamp + run_id    │
  └────────────┬─────────────┘
               │
               ▼
  ┌──────────────────────────┐
  │  AGENT 1                 │
  │  Clause Extractor        │──► Outputs: clause inventory
  │                          │    C1...Cn with types + text
  └────────────┬─────────────┘
               │ 15 second delay
               ▼
  ┌──────────────────────────┐
  │  AGENT 2                 │
  │  Risk Classifier         │──► Outputs: risk scores per
  │  (receives clause list)  │    clause + overall score
  └────────────┬─────────────┘
               │ 15 second delay
               ▼
  ┌──────────────────────────┐
  │  AGENT 3                 │
  │  Negotiation Strategist  │──► Outputs: counter-proposals
  │  (receives risk scores)  │    per HIGH/CRITICAL clause
  └────────────┬─────────────┘
               │ 15 second delay
               ▼
  ┌──────────────────────────┐
  │  AGENT 4                 │
  │  Redline Orchestrator    │──► Outputs: final redline
  │  (receives all 3 outputs)│    package + executive summary
  └────────────┬─────────────┘
               │
               ▼
  ┌──────────────────────────┐
  │  GOOGLE SHEETS           │
  │  CONTRACT DASHBOARD      │
  │  · Contract registry     │
  │  · Clause risk matrix    │
  │  · Redline tracker       │
  │  · Negotiation log       │
  └──────────────────────────┘
\`\`\`

## Why Sequential Not Parallel

Contract analysis has strict logical dependencies:

\`\`\`
CANNOT score risk   → before extracting clauses
CANNOT propose fix  → before scoring risk
CANNOT synthesise   → before all three complete

Agent 1 → [wait] → Agent 2 → [wait] → Agent 3 → [wait] → Agent 4
\`\`\`

This is fundamentally different from the Vendor Risk Engine where
SecOps, Compliance, and Legal are independent domains that can
run simultaneously.
