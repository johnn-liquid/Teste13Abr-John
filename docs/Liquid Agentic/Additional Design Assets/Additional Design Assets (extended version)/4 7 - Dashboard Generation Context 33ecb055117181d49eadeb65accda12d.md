# 4 7 - Dashboard Generation Context

# Dashboard Generation Context — Liquid OS

This document defines the rules, taxonomy, composition logic, and anti-patterns that govern dashboard generation in Liquid OS. It is written for AI agent consumption and must be followed precisely when any dashboard is created, rendered, or suggested.

---

## 1. What Is a Dashboard in Liquid OS

A dashboard is an **intentional composition of artifacts that answers one central analytical question**. It is not a collection of charts. It is a visual narrative with a beginning, middle, and end.

**Formal definition:**

> A Liquid OS dashboard is a structured set of artifacts — ordered by analytical priority — that takes the user from a key number (anchor KPI), through the context that explains it (supporting visuals), to a clear interpretation and next action (narrative).
> 

**The dashboard title is the question it answers.** Examples:
- “How is my repasse portfolio performing in SP?”
- “What is the credit risk of this buyer?”
- “Are there liquidity opportunities in my portfolio?”

A title like “Portfolio Dashboard” or “Credit Overview” is not acceptable. It communicates no analytical intent.

**Every dashboard must contain:**

1. **One anchor KPI** — the single most important number for the question being asked
2. **Context** — how that number came to be (chart, list, or datatable showing composition or trend)
3. **A suggested action** — what the user should do based on what the data shows

If any of these three elements is missing, the dashboard is incomplete.

---

## 2. Dashboard Taxonomy

| Type | Central Question | Persona | Required Components | Prohibited Components |
| --- | --- | --- | --- | --- |
| **Credit Analysis Dashboard** | What is the credit risk of this buyer? | Analyst | Score KPI + Policy result + Bureau summary | Trend chart (not applicable to point-in-time analysis) |
| **Portfolio Overview** | What is the health of my repasse portfolio? | Coordinator, Executive | KPI group + Status distribution chart + Datatable | More than 1 chart of the same type |
| **Market Intelligence** | What is happening in the real estate market? | Executive | Trend chart + Highlight KPIs + Document (analysis) | Datatable (excessive volume for this context) |
| **Liquidity Monitor** | Are there liquidity opportunities in my portfolio? | Executive, Operator | KPI (active opportunities) + List (candidate assets) | Chart without a threshold reference line |
| **Operational Health** | How are my team’s processes performing right now? | Coordinator, Admin | KPI (active, pending, overdue tasks) + Priority list | Temporal chart (focus is today, not trend) |
| **Repasse Pipeline** | What is the status of the repasse pipeline? | Coordinator | Kanban-like list + KPI (per stage) + Evolution chart | Individual credit data |

Use this table as the first classification step before composing any dashboard. If the user’s question does not map to one of these types, break it down into sub-questions until each maps to a type.

---

## 3. Dashboard Composition Rules

### 3.1 Mandatory Visual Hierarchy

Every dashboard must follow this structure, in order:

```
Dashboard
├── Dashboard Title (heading-h4) — the central question
├── Dashboard Description (body-md, muted) — context in 1–2 lines
├── KPI Section (always at the top)
│   └── 1–4 KPIs relevant to the central question
├── Primary Visual (middle)
│   └── 1 primary Chart OR 1 primary Datatable (never both as primary)
├── Supporting Section (optional, subordinate to primary)
│   └── 1 supporting element (List, Card, or a second chart of a DIFFERENT type)
└── Narrative Section (required when the dashboard has 3+ visual elements)
    └── ArtifactDocument: interpretation of what the data shows + suggested action
```

The hierarchy is fixed. KPIs always come before charts. Charts come before lists. The narrative always closes.

### 3.2 Absolute Numerical Limits

| Constraint | Limit |
| --- | --- |
| KPIs per dashboard | Maximum 4 |
| Charts per dashboard | Maximum 2, and they MUST be of different types |
| Datatables per dashboard | Maximum 1 |
| Minimum viable dashboard | At least 1 KPI + 1 text context element |
| ArtifactDocument requirement | Mandatory if the dashboard has 3 or more visual elements |

**RULE:** Two charts of the same type (e.g., bar + bar, line + line) are never permitted in the same dashboard. If you need two charts, they must be of structurally different types (e.g., bar + line, area + donut).

**RULE:** If the dashboard contains 3 or more visual artifacts, it must close with an `ArtifactDocument` that interprets the combined picture and states a suggested action.

---

## 4. Decision Tree: What to Include in a Dashboard

Use this tree after identifying the central question. Execute each step in order.

```
User requested a dashboard about [TOPIC].

STEP 1 — Identify the central question:
├── Is it about an INDIVIDUAL (buyer, property, contract)?
│     └── → Use Credit Analysis Dashboard or entity detail view
│           (this is NOT a generic portfolio dashboard)
│
├── Is it about a SET (portfolio, pipeline, market)?
│     ├── Set is small (≤ 20 items) → ArtifactList as primary visual
│     └── Set is large (> 20 items) → ArtifactDatatable as primary visual
│
├── Is it about TREND or COMPARISON over time?
│     └── → ArtifactChart as primary (line or area type)
│
├── Is it about DISTRIBUTION or PROPORTION?
│     └── → ArtifactChart as primary (pie or donut, maximum 6 categories)
│
└── Is it about CURRENT STATUS (no temporal dimension)?
      └── → KPI group as primary (no temporal chart)

STEP 2 — After defining the primary visual, ask:
├── Can the user understand the "why" behind the anchor KPI without text?
│     └── No → add ArtifactDocument with interpretation
│
├── Does the number need temporal context to be meaningful?
│     └── Yes → add a sparkline or trend Chart as supporting element
│
└── Are there individual items the user will want to drill into?
      └── Yes → add ArtifactDatatable or ArtifactList as drill-down
```

---

## 5. Rules for Charts Within a Dashboard

This section directly addresses the problem of autonomous charts generated without compositional logic.

### 5.1 Every Chart in a Dashboard MUST:

1. Have a title that describes what is being measured — not generic labels like “Performance” or “Results”
2. Have labeled axes (both X and Y where applicable)
3. Have a legend if it displays more than 1 data series
4. Be accompanied by at least 1 interpretive sentence — this can appear in the `ArtifactDocument` section
5. Use only `-chart-1` through `-chart-5` color tokens — never hardcoded hex values
6. Have a defined `empty` state: what is displayed when no data is available

### 5.2 A Chart MUST NOT Appear in a Dashboard:

- If the same data point is already represented as a KPI (redundancy)
- If there are only 1 or 2 data points (use a KPI instead)
- If the data series has only 1 category (use a KPI instead)
- If the user did not ask for temporal analysis and the chart is temporal
- As a second chart of the same type already present in the dashboard

### 5.3 Standalone Charts (Requested Directly by the User)

When a user asks “show me a chart of X” without requesting a full dashboard:

- Render `ArtifactChart` as a standalone artifact
- **RULE:** The agent MUST follow the chart with 1–3 sentences of interpretation
- **RULE:** The agent MUST suggest an action based on what the chart shows
- **RULE:** Returning only a chart with no surrounding context is never acceptable

---

## 6. Domain Metrics by Dashboard Type

These are the canonical metrics available for each dashboard type. Agents must not fabricate metrics outside this list without explicit instruction.

### Credit Analysis

- Credit score (source + query date)
- Declared income vs. estimated commitment ratio
- Default history (count of occurrences)
- Policy result (approved / rejected / pending)
- Bureau data — Serasa, SCR (with timestamps for each record)

### Portfolio / Repasse

- Total active contracts
- Distribution by stage (pre-analysis, analysis, submission, approved, disbursement)
- Total portfolio value (BRL)
- Overdue contracts (count + percentage)
- Average time per stage
- Approval rate (last 30 / 90 days)

### Market Intelligence

- Transaction volume by region
- Average ticket by property type
- Sales velocity index (IVV — Índice de Velocidade de Venda)
- Price per square meter variation
- Development ranking by liquidity

### Liquidity Monitor

- Assets eligible for liquidity (count + value in BRL)
- Eligibility threshold (gap between current asset value and next eligibility threshold)
- History of liquidity operations
- Conversion rate (eligible assets → completed operations)

### Operational Health

- Active tasks (total + by status)
- Tasks with pending approval (with urgency flag)
- Active agents (count + status)
- Working units cost (current period vs. plan limit)
- Unresolved alerts

---

## 7. Autonomous Generation Protocol

This is the structural protocol the agent must execute when creating any dashboard. Each step is mandatory.

```
DASHBOARD_GENERATION_PROTOCOL:

1. IDENTIFY
   → What does the user want to know?
   → State the central question as a single sentence.

2. CLASSIFY
   → Which dashboard type from Section 2 applies?
   → If unclear, ask the user before proceeding.

3. SELECT_PRIMARY
   → Apply the decision tree in Section 4.
   → Choose one primary visual artifact.

4. CHECK_LIMITS
   → Confirm: ≤ 4 KPIs, ≤ 2 Charts (different types), ≤ 1 Datatable.
   → If limits are exceeded, remove the least analytically relevant element.

5. VALIDATE_CHARTS
   → Every chart has: title, labeled axes, legend (if multi-series),
     interpretive sentence, --chart-* tokens, empty state defined.

6. ADD_NARRATIVE
   → If the dashboard has 3 or more visual elements:
     → Add ArtifactDocument at the end with interpretation + suggested action.

7. CHECK_REDUNDANCY
   → Is any data point represented in more than one artifact?
   → If yes, remove the less contextually rich representation.

8. VERIFY_DATA
   → All financial figures are in BRL with proper formatting (R$ 1.234.567,89).
   → All data has a source label and a query/reference timestamp.

9. SUGGEST_ACTION
   → The dashboard must close with a clear next step or recommended action.
   → If no action can be inferred, flag to the user that the data is inconclusive.

10. OUTPUT
    → For simple dashboards (≤ 3 elements): render directly.
    → For complex dashboards (4+ elements): confirm composition with user first.
```

---

## 8. Anti-Patterns

| Anti-Pattern | Why It Is Wrong | Correction |
| --- | --- | --- |
| 5 or more charts in a single dashboard | Cognitive overload; no clear visual hierarchy | Maximum 2 charts of different types |
| Two bar charts in the same dashboard | Type redundancy; no visual differentiation | Use bar + line, or replace one with a KPI |
| Chart with no interpretive text | Data without context is not an insight | Always follow with at least one interpretive sentence |
| KPI and chart displaying the same data | Redundancy — one negates the analytical value of the other | Choose the artifact that best fits the context; remove the other |
| Dashboard with no suggested action | Data without purpose does not drive decisions | Always close with a next step |
| Individual-level data mixed with aggregate data | Conflation of analytical levels creates confusion | Separate entity-level views from portfolio-level views |
| Financial data without timestamps | Regulatory risk — undated financial data is invalid | Always display query date and data source |
| “Everything about X” dashboard | No central question means no focus and no usable insight | Break into multiple thematic dashboards, each with one question |
| Chart returned without context when user asked for a chart | Standalone output with no interpretation is incomplete | Always append interpretation and suggested action |
| Dashboard title that is a label, not a question | Labels communicate category, not analytical intent | Rewrite as a question the dashboard directly answers |