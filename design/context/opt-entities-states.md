---
layer: ux-entities
question: What are the compliance rules and visual states for domain entities?
---

# 4.3 - Entities States and Compliance

## 1. Entity Priority (Archetype Lenses)

Archetypes are operational modes, not identities. A user may shift between archetypes within a single session. The interface must prioritize information based on the mode the user is currently in, not a fixed persona assignment.

*   **Analyst** — Investigates data, validates credit, generates reports. Needs data density and drill-down access.
    *   Entity display priority: 1) Risk badge + severity, 2) Score + source + consultation timestamp, 3) Active blockers and unresolved flags.
    *   Avoids: shallow outputs, forced simplification, missing data provenance.

*   **Coordinator** — Manages process pipelines and agent channels. Needs queue and stage visibility.
    *   Entity display priority: 1) Current stage + next required action, 2) Overdue / blocked counts with urgency flag, 3) Agent status and pending approvals.
    *   Avoids: loss of process context, dead-end states, information that requires navigating away to understand stage.

*   **Decision Maker (Executive)** — Approves, rejects, and sets direction. Needs status at a glance and unambiguous required actions.
    *   Entity display priority: 1) Portfolio KPIs with delta indicators, 2) Trend context (direction, not raw data), 3) High-stakes alerts requiring approval.
    *   Avoids: cognitive overload, ambiguity about what action is expected, data without a recommended next step.

*   **Operator** — Runs cross-client processes, maintains workflows, monitors agent health.
    *   Entity display priority: 1) Client list with urgency states, 2) Process status and agent queue, 3) Active alerts and escalations.
    *   Avoids: dead ends, interfaces that hide queue state, losing context when switching between clients.

> For the full Archetype model including Interaction Intents and Governance Moments, see [design/context/03-experience-context.md](03-experience-context.md).

## 2. Visual States & Badges
*   `active`: Default / Green status.
*   `waiting_approval` / `stalled`: Warning token (Yellow/Orange) + Clock icon.
*   `error` / `blocked`: Destructive token (Red). Must surface error count and be visible in list views.
*   `suspended` (Policy): Blocks all dependent ops. Shows a persistent red banner on governed screens.

## 3. Compliance and PII Masking (Regulated Environment)
*   **Credit Scores:** Standalone scores are FORBIDDEN. Must include: `value` + `source` + `consultation_timestamp`.
*   **PII Masking:** CPF/CNPJ are masked by default (`***.123.456-**`). Unmasking requires `pii_authorized` flag AND user role permission, generating a persistent session indicator.
*   **Currency:** Financial values must follow the locale configured for the Workspace (currency symbol, decimal separator, thousands separator). Locale is determined by a combination of onboarding input and IP/location detection. Default fallback if locale is not set: BRL (`R$ 1.234.567,89`). UI configuration definition is pending (telas). No raw decimals regardless of locale.
*   **Decision Provenance:** All credit decisions must bear the "Verified by Liquid" or "Policy Decision" badge, linking strictly to the Audit ID.
*   **Audit Trail:** Accessible within 1 click from ANY entity that generated a financial decision.

## 4. Interruption Rules
*   **P0 (Fraud / Policy failure / Human approval req):** Modal or Persistent Banner. Blocks UI. Never auto-dismisses.
*   **P1/P2 (Liquidity event / Action needed):** Toast (8s timeout) + Inbox entry.
*   **Silence Rule:** Background system checks, file uploads, routine completions generate Inbox entries ONLY. No toasts. No more than 2 toasts visible simultaneously.
