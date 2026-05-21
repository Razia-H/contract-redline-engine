# Google Sheets — Contract Redline Dashboard Schema

## Tab Name: CONTRACT_REDLINES

| Col | Header | Type | Filled By |
|-----|--------|------|-----------|
| A | Run_ID | Text | System |
| B | Timestamp | DateTime | System |
| C | Party_Name | Text | System |
| D | Contract_Type | Dropdown: MSA, NDA, SOW, DPA, EULA | System |
| E | Overall_Risk_Score | Number 0-100 | System |
| F | Risk_Tier | Dropdown: LOW, MEDIUM, HIGH, CRITICAL | System |
| G | Approval_Status | Dropdown: APPROVED_TO_SIGN, APPROVED_WITH_REDLINES, REQUIRES_NEGOTIATION, DO_NOT_SIGN | System |
| H | Total_Clauses | Number | System |
| I | Critical_Clauses | Number | System |
| J | Must_Change_Count | Number | System |
| K | Walk_Away_Clauses | Text | System |
| L | Executive_Summary | Long Text | System |
| M | Reviewed_By | Text | Human |
| N | Negotiation_Outcome | Text | Human |
| O | Final_Status | Dropdown: SIGNED, REJECTED, IN_NEGOTIATION | Human |

## Conditional Formatting — Column E (Risk Score)
- 0–24: Background #C8E6C9 (Green)
- 25–49: Background #FFF9C4 (Yellow)
- 50–74: Background #FFE0B2 (Orange)
- 75–100: Background #FFCDD2 (Red)

## Human-in-the-Loop Rules
- Score 0–24: No review needed, proceed to sign
- Score 25–49: Legal spot-check required
- Score 50–74: Full legal review + negotiation session
- Score 75–100: Escalate to GC + CFO, consider alternative vendor
