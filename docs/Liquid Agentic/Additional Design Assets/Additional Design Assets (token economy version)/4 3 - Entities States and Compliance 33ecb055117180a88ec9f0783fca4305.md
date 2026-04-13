# 4 3 - Entities States and Compliance

# 4.3 - Entities States and Compliance

## 1. Entity Priority (Archetype Lenses)

- **Analyst:** Focus on Tasks. Priority: 1) Risk badge, 2) Score+Source, 3) Blockers.
- **Coordinator:** Focus on Channels. Priority: 1) Stage, 2) Overdue/blocked counts.
- **Executive:** Focus on Dashboards. Priority: 1) Portfolio KPIs, 2) Trend, 3) High-stakes alerts.
- **Operator:** Focus on cross-client. Priority: 1) Client list + urgency states.

## 2. Visual States & Badges

- `active`: Default / Green status.
- `waiting_approval` / `stalled`: Warning token (Yellow/Orange) + Clock icon.
- `error` / `blocked`: Destructive token (Red). Must surface error count and be visible in list views.
- `suspended` (Policy): Blocks all dependent ops. Shows a persistent red banner on governed screens.

## 3. Compliance and PII Masking (Regulated Environment)

- **Credit Scores:** Standalone scores are FORBIDDEN. Must include: `value` + `source` + `consultation_timestamp`.
- **PII Masking:** CPF/CNPJ are masked by default (`**.123.456-**`). Unmasking requires `pii_authorized` flag AND user role permission, generating a persistent session indicator.
- **Currency:** Currency format is defined by the location of the workspace.
- **Decision Provenance:** All credit decisions must bear the “Verified by Liquid” or “Policy Decision” badge, linking strictly to the Audit ID.
- **Audit Trail:** Accessible within 1 click from ANY entity that generated a financial decision.

## 4. Interruption Rules

- **P0 (Fraud / Policy failure / Human approval req):** Modal or Persistent Banner. Blocks UI. Never auto-dismisses.
- **P1/P2 (Liquidity event / Action needed):** Toast (8s timeout) + Inbox entry.
- **Silence Rule:** Background system checks, file uploads, routine completions generate Inbox entries ONLY. No toasts. No more than 2 toasts visible simultaneously.