# Contract Redline Engine — Data Mapping Schematic

\`\`\`
NODE 1: Browser File Input
  EMITS → {
    "contract_text": "<full extracted text>",
    "contract_type": "MSA",
    "party_name": "Acme Corp",
    "timestamp": "2025-05-21T14:00:00Z",
    "run_id": "uuid-xxxx"
  }

NODE 2: OpenRouter API — Agent 1 (Clause Extractor)
  SENDS → {
    "model": "anthropic/claude-haiku-4-5",
    "messages": [
      {"role":"system","content":"<AGENT 1 PROMPT>"},
      {"role":"user","content":"CONTRACT:\n{{contract_text}}"}
    ]
  }
  RETURNS → {
    "clauses": [
      {"clause_id":"C1","clause_type":"Liability Cap",
       "original_text":"...","location":"Section 5"},
      {"clause_id":"C2","clause_type":"Indemnification",
       "original_text":"...","location":"Section 6"}
    ],
    "total_clauses_found": 10,
    "missing_critical_clauses": ["Data Processing Agreement"]
  }

NODE 3: OpenRouter API — Agent 2 (Risk Classifier)
  SENDS → {
    "model": "anthropic/claude-haiku-4-5",
    "messages": [
      {"role":"system","content":"<AGENT 2 PROMPT>"},
      {"role":"user","content":"CONTRACT + CLAUSES:\n{{contract_text}}\n{{agent1_output}}"}
    ]
  }
  RETURNS → {
    "overall_contract_risk": 74,
    "critical_clause_count": 2,
    "walk_away_clauses": ["C1","C4"],
    "clause_risk_scores": [
      {"clause_id":"C1","risk_score":88,"risk_category":"CRITICAL",
       "risk_reason":"Liability cap limited to 30 days fees"},
      {"clause_id":"C2","risk_score":72,"risk_category":"HIGH",
       "risk_reason":"Unilateral indemnification only"}
    ]
  }

NODE 4: OpenRouter API — Agent 3 (Negotiation Strategist)
  SENDS → {
    "model": "anthropic/claude-haiku-4-5",
    "messages": [
      {"role":"system","content":"<AGENT 3 PROMPT>"},
      {"role":"user","content":"CONTRACT + RISK SCORES:\n{{agent2_output}}"}
    ]
  }
  RETURNS → {
    "negotiation_stance": "AGGRESSIVE",
    "counter_proposals": [
      {"clause_id":"C1",
       "original_text":"liability limited to 30 days fees",
       "proposed_replacement":"liability limited to 12 months fees",
       "negotiation_rationale":"Industry standard is 12 months",
       "priority":"MUST_CHANGE",
       "walk_away":true}
    ],
    "estimated_risk_reduction": "Score drops from 74 to 31"
  }

NODE 5: OpenRouter API — Agent 4 (Redline Orchestrator)
  SENDS → {
    "model": "anthropic/claude-haiku-4-5",
    "messages": [
      {"role":"system","content":"<AGENT 4 PROMPT>"},
      {"role":"user","content":"{{agent1}} + {{agent2}} + {{agent3}}"}
    ]
  }
  RETURNS → {
    "party_name": "Acme Corp",
    "contract_type": "MSA",
    "overall_risk_score": 74,
    "risk_tier": "HIGH",
    "approval_status": "REQUIRES_NEGOTIATION",
    "executive_summary": "2 sentences...",
    "must_change_count": 3,
    "should_change_count": 2,
    "acceptable_count": 5,
    "walk_away_clauses": ["Liability cap at 30 days"],
    "redline_items": [
      {"clause_id":"C1","clause_type":"Liability Cap",
       "risk_score":88,"priority":"MUST_CHANGE",
       "original_text":"...","redlined_text":"...",
       "change_rationale":"...","walk_away":true}
    ],
    "estimated_post_negotiation_score": 28
  }

NODE 6: Google Sheets — Add Row
  MAPS → {
    Column A (Run_ID):            {{run_id}},
    Column B (Timestamp):         {{timestamp}},
    Column C (Party_Name):        {{party_name}},
    Column D (Contract_Type):     {{contract_type}},
    Column E (Risk_Score):        {{overall_risk_score}},
    Column F (Risk_Tier):         {{risk_tier}},
    Column G (Approval_Status):   {{approval_status}},
    Column H (Total_Clauses):     {{total_clauses_reviewed}},
    Column I (Critical_Clauses):  {{critical_clause_count}},
    Column J (Must_Change):       {{must_change_count}},
    Column K (Walk_Away):         {{walk_away_clauses}},
    Column L (Exec_Summary):      {{executive_summary}},
    Column M (Reviewed_By):       [HUMAN FILLS IN],
    Column N (Negotiation_Outcome):[HUMAN FILLS IN],
    Column O (Final_Status):      [HUMAN FILLS IN]
  }
\`\`\`
