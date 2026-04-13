---
layer: reporting
question: How are quantitative dashboards constructed by agents?
---

# 4.5 - Dashboard Generation

## 1. Dashboard Definition
A Liquid OS dashboard answers ONE central analytical question. Elements follow a strict semantic hierarchy: `Anchor KPI` → `Context/Chart` → `Action/Interpretation`.

## 2. Compositional Judgment
The number and type of elements in a dashboard are chosen strategically by the agent based on the complexity of the answer the user needs. Each element must be analytically justified — if you cannot state why an element is present, remove it.

**Structural rules that always apply:**
*   Never duplicate chart types in the same dashboard (e.g., bar + bar). Two charts must be structurally different (e.g., Bar + Line).
*   If a dashboard has ≥3 visual artifacts, it MUST conclude with an `ArtifactDocument` providing interpretation and a recommended next step.
*   A minimum viable dashboard requires at least 1 anchor KPI and 1 context element.

## 3. Chart Rules
*   Every chart requires a title, labeled axes, and a defined `empty` state.
*   `--chart-1` through `--chart-5` tokens ONLY.
*   **Autonomous Chart Rule:** ANY standalone chart must be accompanied by 1-3 sentences of interpretive agent text. Data without interpretation is not an insight.
*   Do NOT use a chart for 1 or 2 data points (use a KPI instead).

## 4. Generation Protocol (10 Steps)
1. **IDENTIFY:** State the core analytical query.
2. **CLASSIFY:** Choose domain archetype (Credit, Portfolio, Liquidity). 
3. **SELECT_PRIMARY:** Trend -> Chart. Broad set -> Datatable. Narrow set -> List.
4. **CHECK_COMPOSITION:** Confirm each element is analytically justified. Remove anything that cannot be explained. Verify no duplicate chart types.
5. **VALIDATE_CHARTS:** Enforce labeling and differing types.
6. **ADD_NARRATIVE:** Append interpretation if 3+ visuals.
7. **CHECK_REDUNDANCY:** Remove charts if data is already visible in KPI.
8. **VERIFY_DATA:** Financial figures use the Workspace locale (default fallback: BRL). Dates follow the Workspace locale format and timestamps are present.
9. **SUGGEST_ACTION:** State the concrete next operational step.
10. **OUTPUT:** Render or ask permission if extremely complex.
