# 4 5 - Dashboard Generation

# 4.5 - Dashboard Generation

## 1. Dashboard Definition

A Liquid OS dashboard answers ONE central analytical question. Elements follow a strict semantic hierarchy: `Anchor KPI` → `Context/Chart` → `Action/Interpretation`.

## 2. Strict Generation Limits

- **Max 4 KPIs** per dashboard.
- **Max 2 Charts** per dashboard. They MUST be structurally different types (e.g., Bar + Line). Never duplicate chart types.
- **Max 1 Datatable**.
- **Mandatory Closing:** If a dashboard has ≥3 visual artifacts, it MUST conclude with an `ArtifactDocument` providing the agent’s interpretation and recommending a next step.

## 3. Chart Rules

- Every chart requires a title, labeled axes, and a defined `empty` state.
- `-chart-1` through `-chart-5` tokens ONLY.
- **Autonomous Chart Rule:** ANY standalone chart must be accompanied by 1-3 sentences of interpretive agent text. Data without interpretation is not an insight.
- Do NOT use a chart for 1 or 2 data points (use a KPI instead).

## 4. Generation Protocol (10 Steps)

1. **IDENTIFY:** State the core analytical query.
2. **CLASSIFY:** Choose domain archetype (Credit, Portfolio, Liquidity).
3. **SELECT_PRIMARY:** Trend -> Chart. Broad set -> Datatable. Narrow set -> List.
4. **CHECK_LIMITS:** Confirm element caps (4 KPIs, 2 Charts).
5. **VALIDATE_CHARTS:** Enforce labeling and differing types.
6. **ADD_NARRATIVE:** Append interpretation if 3+ visuals.
7. **CHECK_REDUNDANCY:** Remove charts if data is already visible in KPI.
8. **VERIFY_DATA:** BRL formatting. Dates are Brazilian (DD/MM/YYYY) and timestamps are present.
9. **SUGGEST_ACTION:** State the concrete next operational step.
10. **OUTPUT:** Render or ask permission if extremely complex.