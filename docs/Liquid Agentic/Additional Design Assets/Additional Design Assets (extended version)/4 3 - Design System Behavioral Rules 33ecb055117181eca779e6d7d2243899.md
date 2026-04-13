# 4 3 - Design System Behavioral Rules

# 4.3 - Behavioral UX Architecture

This document instructs AI agents on **how Liquid OS behaves** in specific, domain-relevant situations. It sits between the token/component layer (`ds/`) and the abstract strategy layer (`4.x`). When a design decision involves a conditional, a state, a persona, or an interruption event specific to Liquid’s domain, this document is the primary authority.

Do not duplicate design system definitions. Reference them via `→ see ds/...`.

---

## 1. Information Hierarchy by Persona and Context

### 1.1 Persona Definitions

| Persona | Core Job-to-be-Done | Primary Metric | Dominant Flow |
| --- | --- | --- | --- |
| Credit Analyst | Evaluate creditworthiness and produce a formal decision | Risk score accuracy | Chat → Analysis → Decision |
| Transfer Coordinator | Orchestrate repasse process from intake to settlement | Pipeline velocity | Channel → Kanban → Actions |
| Executive / Manager | Monitor portfolio health and approve high-stakes decisions | Portfolio KPIs | Dashboard → Drill-down |
| Operator (Operating Side) | Manage multiple clients and cross-entity operations | Task throughput across clients | Cross-client list views |

---

### 1.2 Credit Analyst

**RULE:** When the active screen is a credit analysis task, the agent must surface the following in this exact priority order:

1. Current risk classification badge (approved / rejected / pending_review / requires_human)
2. Score value + source + timestamp (never without all three)
3. Blocking factors (any negative signal that would prevent approval)
4. Supporting documents status (received / missing / expired)
5. Historical credit events (only if present)
6. Audit trail access (persistent, 1-click, never hidden)

**RULE:** Secondary information (entity address, contact data, portfolio history) must not compete visually with items 1–4. Place in a collapsible context panel or secondary tab.

**RULE:** When the screen is a list of credit tasks, sort default is: `requires_human` first → `pending_review` → `rejected` (for review) → `approved` (for audit). Never sort alphabetically by default for this persona.

**ANTI-PATTERN:** Displaying a credit score as a standalone number without `source` and `consultation_date`. This violates Principle 5 (Regulated Market) and must never occur.

**ANTI-PATTERN:** Auto-collapsing the blocking factors section on load. It must be expanded by default when the risk state is `requires_human` or `pending_review`.

---

### 1.3 Transfer Coordinator

**RULE:** When the active screen is a Channel (repasse process), prioritize layout in this order:

1. Current stage in the funnel (which kanban column, how many tasks in it)
2. Overdue or blocked tasks (badge count, not buried in list)
3. Pending actions requiring human input
4. Chat/agent interaction area (lateral, always visible but not dominant)
5. Timeline of completed steps (collapsed by default, expandable)

**RULE:** Data density must be moderate. The Transfer Coordinator works in process sequences, not data grids. Prefer Kanban + action cards over data tables as the primary layout.

**RULE:** When the user is X = Transfer Coordinator and the screen is Y = Channel detail, prioritize Z = stage progress + blocked items over W = document listing or financial metrics.

**ANTI-PATTERN:** Showing a full data table as the primary view in a Channel. Process flows must be visualized as Kanban or timeline, with data tables accessible via secondary tab only.

---

### 1.4 Executive / Manager

**RULE:** When the active screen is a Dashboard Hub, apply maximum information density only in the macro KPI row. All drill-down content must be accessible but not dominant.

Priority order for Executive screens:

1. Portfolio-level KPIs (total in pipeline, at-risk count, pending approvals)
2. Trend indicators (delta vs. previous period, directional arrows)
3. Alerts requiring executive action (approvals, risk escalations)
4. Team/agent performance summary
5. Individual entity details (always behind a drill-down interaction)

**RULE:** When the user is X = Executive and the screen is Y = Dashboard Hub, prioritize Z = macro KPIs + alerts over W = task-level detail or document lists.

**RULE:** Executives must never be shown raw bureau data or unprocessed credit scores. All financial data must be summarized through a Policy Decision or aggregated KPI.

**ANTI-PATTERN:** Placing individual task cards in the Executive dashboard primary view. Tasks are an Analyst/Coordinator concern. The Executive sees counts, rates, and escalations only.

---

### 1.5 Operator (Operating Side)

**RULE:** The Operator view is inherently cross-client. The primary organizing dimension is always **client/entity** first, then status, then recency.

Priority order for Operator screens:

1. Client list with active task count and highest-urgency state per client
2. Cross-client alerts (anything requiring action across any client)
3. Current open tasks grouped by client
4. Performance metrics per client (volume, velocity, errors)

**RULE:** When the user is X = Operator and the screen is Y = any list page, prioritize Z = client-grouped view with status badges over W = flat chronological task list.

**RULE:** The Operator must always have a visible “context switcher” showing the current active client scope. This element is permanent, never hidden.

**ANTI-PATTERN:** Merging all clients into a single flat list without grouping or client identity markers. This creates context confusion and violates the Multiplayer Principle (#6).

---

## 2. Entity States and Mandatory Visual Treatment

### 2.1 State Registry

### Task / Chat

| State | Visual Treatment | Badge Color | Icon | Position |
| --- | --- | --- | --- | --- |
| `active` | Default, no special marking | — | — | — |
| `waiting_approval` | Yellow badge + pulse dot | `warning` token | clock icon | Left of title |
| `completed` | Muted opacity (0.6), checkmark | `success` token | check icon | Left of title |
| `error` | Red left border + badge | `destructive` token | alert-triangle | Left of title |
| `archived` | Greyed out, not in default view | `muted` token | archive icon | Hidden by default |

→ see `ds/tokens/colors` for token values

→ see `ds/rulebook.md #03` for badge composition rules

**RULE:** A Task in `error` state must always be visible in default list views. It must never be filtered out by default.

**RULE:** Priority resolution when multiple states conflict: `error` > `waiting_approval` > `active` > `completed` > `archived`. Only one state badge is shown at a time.

---

### Channel

| State | Visual Treatment | Badge | Position |
| --- | --- | --- | --- |
| `active` | Normal | — | — |
| `stalled` | Orange left border + “Stalled” label | `warning` | Sidebar item |
| `blocked` | Red left border + “Blocked” label | `destructive` | Sidebar item |
| `completed` | Archive icon, moved to Completed section | `muted` | Separate list section |
| `awaiting_external` | Blue dashed border + “Awaiting” label | `info` | Sidebar item |

**RULE:** Channels in `blocked` or `stalled` state must appear above `active` channels in the sidebar, sorted by staleness duration (longest first).

---

### Agent

| State | Visual Treatment |
| --- | --- |
| `idle` | Grey dot indicator |
| `running` | Green pulse dot + spinner micro-animation |
| `error` | Red dot + error count badge |
| `paused` | Yellow dot + pause icon |
| `disabled` | Greyed out, toggle off state |

**RULE:** An Agent in `error` state must surface its error count visibly in any view where the agent is referenced (Channel header, task card, sidebar agent list).

---

### Policy

| State | Visual Treatment |
| --- | --- |
| `active` | Green dot, “Active” label |
| `draft` | Dashed border, “Draft” label |
| `under_review` | Yellow dot, “Under Review” label |
| `deprecated` | Red strikethrough label, read-only |
| `suspended` | Orange badge, all dependent operations blocked |

**RULE:** A `suspended` Policy must trigger a banner on any screen where that policy governs an active operation. The banner must link directly to the policy entity.

---

### Dashboard

| State | Visual Treatment |
| --- | --- |
| `live` | Green dot in header |
| `stale` | Orange “Data stale” warning + last-updated timestamp |
| `loading` | Skeleton placeholders (never spinners for full-page loads) |
| `error` | Red banner with retry action |

**RULE:** A stale dashboard must always display the last successful data timestamp. Never show data without its freshness indicator.

---

### Routine

| State | Visual Treatment |
| --- | --- |
| `scheduled` | Clock icon + next run time |
| `running` | Pulse dot + progress bar (if duration > 5s) |
| `completed` | Checkmark + last run timestamp |
| `failed` | Red dot + error summary + retry CTA |
| `paused` | Yellow dot + “Paused by [actor]” label |

---

### File

| State | Visual Treatment |
| --- | --- |
| `uploaded` | Default file icon |
| `processing` | Spinner overlay on icon |
| `verified` | Green checkmark badge |
| `rejected` | Red X badge + rejection reason on hover |
| `expired` | Orange badge + expiry date |
| `missing` | Red placeholder with “Missing” label |

**RULE:** A `missing` file that is required for a credit decision must block the approval action. The block must be visually communicated on the action button itself (disabled + tooltip explaining the blocker).

---

### Credit Decision States (Special — Regulated Domain)

| State | Visual Treatment | Human Intervention Required |
| --- | --- | --- |
| `approved` | Green “Approved” badge + “Policy Decision” label | No |
| `rejected` | Red “Rejected” badge + reason summary + “Policy Decision” label | No |
| `pending_review` | Yellow “Pending Review” badge + assigned analyst name | No (but tracked) |
| `requires_human` | Orange “Requires Human” badge + escalation CTA | YES — blocks auto-progression |

**RULE:** The `requires_human` state must render an explicit escalation action visible without scrolling. This action must identify the responsible human by name or role.

**RULE:** Every credit decision state must display: (1) the policy that generated it, (2) the timestamp, (3) the data source. Absence of any of these three is a rendering error.

**ANTI-PATTERN:** Showing `approved` or `rejected` without the “Policy Decision” or “Verified by Liquid” badge. A decision without provenance is non-compliant.

---

## 3. Layout Patterns by Screen Type

### 3.1 Layout: AIWorkflow (Chat + Artifacts)

The primary interface for agentic interaction. Used for credit analysis, document review, market queries.

```
┌──────────────┬──────────────────────────────┬──────────────┐
│   Sidebar    │       Artifact Area          │ Context Panel│
│   (fixed)    │      (primary, scrollable)   │ (contextual) │
│   220px      │      flex-1                  │  320px       │
│              │                              │              │
│              ├──────────────────────────────┤              │
│              │       Chat Input Bar         │              │
│              │       (fixed bottom)         │              │
└──────────────┴──────────────────────────────┴──────────────┘
```

**Column proportions:** Sidebar 220px fixed | Artifact area flex (min 480px) | Context panel 320px collapsible

**Permanent elements:** Sidebar, Chat Input Bar
**Contextual elements:** Context Panel (appears when an entity is selected or agent surfaces related context)

**Scroll behavior:** Artifact area scrolls independently. Chat input is always pinned to bottom. Context panel has its own scroll.

**RULE:** The Chat Input Bar must always be visible. It must never scroll out of view.

**RULE:** The Context Panel must auto-open when the agent attaches a related entity (e.g., surfacing a borrower’s profile during credit chat). It must never auto-open for purely informational responses.

**ANTI-PATTERN:** Placing the chat history and artifacts in the same scroll container. They are distinct interaction layers.

---

### 3.2 Layout: ListPage with Active Agent

Used for task lists, channel lists, and entity lists where an agent is actively working or available.

```
┌──────────────┬────────────────────────────────────────────┐
│   Sidebar    │  Page Header (title + filters + actions)   │
│   (fixed)    ├────────────────────────────────────────────┤
│   220px      │  Agent Status Bar (collapsible, top)       │
│              ├────────────────────────────────────────────┤
│              │  Entity List (primary, scrollable)         │
│              │                                            │
│              │                                            │
│              ├────────────────────────────────────────────┤
│              │  Agent Input (docked, appears on focus)    │
└──────────────┴────────────────────────────────────────────┘
```

**RULE:** The Agent Status Bar must show: agent name, current state, last action taken. It is collapsed by default when the agent is `idle`.

**RULE:** The Agent Input docked bar appears when the user focuses the list (click or keyboard) and disappears when navigating away. It is not a persistent chat — it is a contextual command input.

**ANTI-PATTERN:** Replacing the list with a full chat interface. The list remains the primary view; the agent is an overlay layer.

---

### 3.3 Layout: Dashboard Hub

Used for Executive and Operator macro views.

```
┌──────────────┬────────────────────────────────────────────┐
│   Sidebar    │  Dashboard Header (title + date range)     │
│   (fixed)    ├────────────────────────────────────────────┤
│   220px      │  KPI Row (4–6 KPIs, full width)            │
│              ├──────────────────────────────────┬─────────┤
│              │  Primary Chart / Main Widget     │ Alerts  │
│              │  (60% width)                     │ Panel   │
│              │                                  │ (40%)   │
│              ├──────────────────────────────────┴─────────┤
│              │  Secondary Grid (2–3 col, responsive)      │
└──────────────┴────────────────────────────────────────────┘
```

**RULE:** KPI Row is always the first visible content. It must render within 1 viewport without scrolling on any supported breakpoint.

**RULE:** Alerts Panel is sticky within its column during vertical scroll of secondary grid.

**RULE:** Responsive breakpoint behavior: at < 1280px, Alerts Panel drops below Primary Chart. At < 768px, KPI Row wraps to 2 columns.

**ANTI-PATTERN:** Using the Dashboard Hub layout for entity detail. Dashboards aggregate; detail pages (→ Layout DetailPage) individualize.

---

### 3.4 Layout: Channel (Process Kanban)

Used for repasse process management and structured multi-step workflows.

```
┌──────────────┬────────────────────────────────┬───────────┐
│   Sidebar    │  Channel Header (name + status)│  Chat     │
│   (fixed)    ├────────────────────────────────┤  Panel    │
│   220px      │  Kanban Board (horizontal      │  (fixed   │
│              │  scroll, primary)              │  right)   │
│              │                                │  360px    │
│              │                                │           │
│              ├────────────────────────────────┤           │
│              │  Timeline Strip (collapsed)    │           │
└──────────────┴────────────────────────────────┴───────────┘
```

**RULE:** The Chat Panel is always visible in Channel view. It must never be fully hidden. Minimum collapsed width: 48px (icon-only mode).

**RULE:** Kanban Board scrolls horizontally. Each column has a fixed width of 280px. The board area has its own scroll context independent of the page.

**RULE:** Timeline Strip is collapsed by default. It expands upward from the bottom of the Kanban area when toggled. It never replaces the Kanban view.

**ANTI-PATTERN:** Using vertical scroll for the Kanban columns. Kanban always scrolls horizontally at the board level, vertically within each column card stack.

---

### 3.5 Layout: DetailPage

Used for individual entity inspection: a specific task, a borrower profile, a repasse file, a routine detail.

```
┌──────────────┬────────────────────────────────────────────┐
│   Sidebar    │  Entity Header (name + state badge + meta) │
│   (fixed)    ├────────────────────────────────────────────┤
│   220px      │  Primary Action Bar (fixed, below header)  │
│              ├────────────────────────────────────────────┤
│              │  Context Tabs (Overview / Docs / History   │
│              │  / Audit)                                  │
│              ├────────────────────────────────────────────┤
│              │  Tab Content (scrollable)                  │
│              │                                            │
└──────────────┴────────────────────────────────────────────┘
```

**RULE:** Entity Header is always fixed. It must never scroll out of view on DetailPage.

**RULE:** The Primary Action Bar contains at most 3 primary actions. Overflow actions go into a “More” dropdown. Actions must be context-aware to the current entity state.

**RULE:** The Audit tab must always be present on any DetailPage for an entity that participates in financial decisions. It must be accessible within 1 click from the header.

**RULE:** Default active tab is “Overview” on first load. The system may deep-link to a specific tab, in which case that tab is active on load.

**ANTI-PATTERN:** Placing audit trail data only at the bottom of a scrollable list. Audit is a first-class tab, not a footer section.

---

## 4. Attention and Interruption Rules

### 4.1 Interruption Mechanism Decision Tree

```
EVENT RECEIVED
│
├── Is this a blocking event? (requires_human, policy_suspended, fraud_flag)
│   └── YES → Modal (if user is idle) OR Banner (if user is active in a task)
│
├── Is this a high-urgency domain event? (liquidity_trigger, risk_alert_level_1)
│   └── YES → Toast (persistent, must be manually dismissed) + Inbox entry
│
├── Is this a medium-urgency event? (task_completed_by_agent, approval_needed, stalled_channel)
│   └── YES → Toast (auto-dismiss 8s) + Inbox entry
│
├── Is this a low-urgency event? (routine_completed, file_uploaded, agent_idle)
│   └── YES → Inbox entry only (no toast, no interruption)
│
└── Is this a system/background event? (sync completed, cache refresh, log written)
    └── YES → Silent. No notification of any kind.
```

→ see `ds/rulebook.md #02` for generic notification component rules

---

### 4.2 Domain Event Urgency Hierarchy

| Priority | Event | Mechanism | Auto-dismiss |
| --- | --- | --- | --- |
| P0 | `fraud_flag` on any entity | Modal (blocks UI) | Never |
| P0 | `policy_suspended` affecting active operation | Banner (persistent) | Never |
| P0 | `requires_human` credit decision | Modal or Banner | Never |
| P1 | `liquidity_trigger` (asset ready for transaction) | Persistent Toast + Inbox | No |
| P1 | `risk_alert_level_1` (score below critical threshold) | Persistent Toast + Inbox | No |
| P2 | `approval_needed` (waiting_approval task assigned to user) | Toast (8s) + Inbox | Yes (8s) |
| P2 | `stalled_channel` (no activity > 48h) | Toast (8s) + Inbox | Yes (8s) |
| P3 | `task_completed_by_agent` | Toast (5s) + Inbox | Yes (5s) |
| P3 | `document_expired` | Inbox only | N/A |
| P4 | `routine_completed` | Inbox only | N/A |
| P4 | `file_uploaded` | Inbox only | N/A |
| P5 | Any system/infrastructure event | Silent | N/A |

**RULE:** No more than 2 toasts may be visible simultaneously. If a third arrives while 2 are active, it is queued and shown after the oldest dismisses (or immediately if it is P0/P1).

**RULE:** P0 events always interrupt regardless of the user’s current active state (including during form input).

---

### 4.3 Silence Rules — When the Agent Must NOT Notify

**RULE:** The agent must not send any notification when:

1. The event was triggered by the user’s own action in the last 30 seconds (echo suppression)
2. The event is a background system process with no user-facing consequence
3. The user is in a `focus_mode` session (explicitly activated) — only P0 events break through
4. The entity the event belongs to is not within the user’s current role scope (→ Multiplayer Principle #6)
5. The event duplicates an unread Inbox item for the same entity within the last 60 minutes
6. The event is a `routine_completed` for a routine the user did not configure and has never interacted with

**ANTI-PATTERN:** Sending a toast for every agent action step. Agents work in the background. Only outcomes and blockers surface to the user.

---

## 5. Progression Behavior (Funnel Journey)

### 5.1 Onboarding State

**Definition:** User has 0 active Channels, 0 configured Agents, 0 Policies.

**RULE:** In Onboarding State, the interface must show a maximum of 3 visible actions at any time. Additional capabilities must not be discoverable until the first anchor action is completed.

Anchor actions (presented in sequence — never all at once):

1. “Create your first Channel” (establishes workspace context)
2. “Add a document or client to analyze” (triggers first agent interaction)
3. “Review your first result” (completes the value loop)

**RULE:** Sidebar in Onboarding State shows only: Home, + New Channel, Help. All other nav items are hidden.

**RULE:** The Chat interface in Onboarding State shows a guided prompt (not blank). The agent opens the conversation with a context-aware first message.

**ANTI-PATTERN:** Showing the full sidebar navigation to a new user. Cognitive overload at zero value violates PLG Principle (#2).

---

### 5.2 Active State

**Definition:** User has ≥ 1 active Channel.

**Unlocks:**
- Full sidebar with Channels list
- Inbox (now has real entries)
- Agents panel (visible, not yet configurable)
- Search (global)
- Notifications

**RULE:** Transition from Onboarding to Active State is silent — no modal, no celebration animation. The sidebar expands naturally as channels populate.

**RULE:** In Active State, the Chat interface defaults to the most recently active Channel context, not a blank slate.

---

### 5.3 Power State

**Definition:** User has ≥ 1 configured Agent AND ≥ 1 active Policy.

**Unlocks:**
- Agents configuration panel
- Routines (schedule and automate)
- Policy management
- Advanced Dashboard with drill-down
- Audit & Compliance section
- API / MCP access (if role permits)

**RULE:** Routines and Policy management must not be surfaced before Power State. They must not appear as locked/disabled items — they must be entirely absent from the UI until unlocked.

**RULE:** The transition to Power State triggers a single contextual tooltip on the newly unlocked nav item, visible for one session only.

---

### 5.4 Progressive Unlock Rules

```
FEATURE VISIBILITY DECISION
│
├── Is the feature part of Onboarding State baseline?
│   └── YES → Always visible from first login
│
├── Has the user completed the Active State trigger?
│   (≥1 active Channel)
│   └── YES → Active State features are now visible
│   └── NO  → Hide (not lock, not disable — hide)
│
├── Has the user completed the Power State trigger?
│   (≥1 Agent configured AND ≥1 Policy active)
│   └── YES → Power State features are now visible
│   └── NO  → Hide
│
└── Is this a role-gated feature?
    (Audit, API, Policy Management)
    └── Check role permissions → see Multiplayer Principle #6
        └── If permitted AND Power State: show
        └── If not permitted: hide (never show as "upgrade required" to non-admin roles)
```

**ANTI-PATTERN:** Showing locked/greyed-out features with “Upgrade to unlock” messaging in any state. Progressive disclosure in Liquid is about removing noise, not upselling friction.

---

## 6. Artifact Composition Rules in Chat

### 6.1 Standard Composition Sequence

When a response is complex and involves multiple artifact types, follow this sequence:

```
COMPLEX RESPONSE COMPOSITION
│
1. ArtifactKPI    — Lead with the headline numbers (max 4 KPIs)
2. ArtifactChart  — Visualize the trend or distribution that explains the KPIs
3. ArtifactDatatable — Provide the underlying data for exploration (if needed)
4. ArtifactDocument — Conclusions, recommendations, policy references (if needed)
│
Optional additions (only when contextually relevant):
├── ArtifactCard    — For a single entity spotlight within a larger response
├── ArtifactList    — For enumerated action items or ranked results
├── ArtifactGraph   — For relationship visualization (not default, only when topology matters)
└── ArtifactLink    — Always last, for external references or related entities
```

**RULE:** Never open a complex response with ArtifactDocument. Context (KPIs, charts) must precede narrative conclusions.

**RULE:** ArtifactDatatable must only appear when the user needs to explore or verify data. If the response is a recommendation or decision, omit the raw data table.

---

### 6.2 When NOT to Combine Artifacts

**RULE:** Do not combine more than 4 artifact types in a single Chat response. Combinations of 5+ indicate the response should be split into a multi-turn conversation or a dedicated Document artifact.

**RULE:** Do not combine ArtifactChart + ArtifactGraph in the same response unless they represent fundamentally different data dimensions. They compete visually.

**RULE:** Do not append ArtifactDatatable after ArtifactDocument. Tables are exploratory; documents are conclusive. The sequence must not invert.

**ANTI-PATTERN:** Generating a response that is all artifacts with no agent text. Every artifact sequence must be accompanied by at minimum one sentence of agent interpretation framing the artifacts.

---

### 6.3 Credit Analysis — Artifact Composition

Canonical sequence for a credit analysis response:

```
1. ArtifactKPI
   - Credit score (with source + timestamp)
   - Risk classification badge
   - Recommended limit
   - Decision state

2. ArtifactChart
   - Score history over time (if available)
   - OR debt composition breakdown

3. ArtifactDatatable
   - Bureau data breakdown (fields, values, consultation dates)
   - Restricted fields masked per Section 7 rules

4. ArtifactDocument
   - Formal decision summary
   - Policy applied
   - Blocking factors (if any)
   - Required next steps
   - Audit reference
```

**RULE:** In a credit analysis response, ArtifactKPI must always be the first artifact rendered. The decision state must be visible without any scrolling.

**RULE:** If the decision state is `requires_human`, the ArtifactDocument must include an explicit escalation instruction with the name or role of the responsible human.

---

### 6.4 Portfolio Overview — Artifact Composition

Canonical sequence for a portfolio overview response:

```
1. ArtifactKPI
   - Total assets in pipeline
   - At-risk count
   - Liquidity index
   - Average cycle time

2. ArtifactChart
   - Portfolio distribution by status
   - OR pipeline velocity over time

3. ArtifactList
   - Top 5 items requiring attention (ranked by urgency)

4. ArtifactCard (optional)
   - Single spotlight entity if one dominates the portfolio signal

5. ArtifactLink (optional)
   - Link to full dashboard or detailed drill-down
```

**RULE:** Portfolio overviews must not include ArtifactDatatable as the primary artifact. Raw data grids are not executive-facing. Use ArtifactList or ArtifactKPI to summarize.

---

## 7. Financial and Regulated Data Treatment

### 7.1 Mandatory Display Rules

| Data Type | Rule |
| --- | --- |
| Credit score | Must display: value + source name + consultation timestamp. All three are required. |
| CPF / CNPJ | Always masked by default: `***.123.456-**`. Unmasked only on screens with explicit `pii_authorized` permission flag. |
| Financial values | Always formatted as BRL: `R$ 1.234.567,89`. Never raw (e.g., never `1234567.89`). |
| Credit decision | Must carry “Verified by Liquid” OR “Policy Decision” badge. No exceptions. |
| Bureau data | Must show `Consulted on: [date]` indicator adjacent to the data block. |
| Audit trail | Must be accessible within 1 click from any entity that participated in a financial decision. |
| Interest rates | Always as percentage with 2 decimal places: `12,50% a.a.`. Never raw decimals. |
| Dates | Always in Brazilian format: `DD/MM/AAAA`. Never ISO-only format in user-facing views. |

---

### 7.2 CPF / CNPJ Masking Rules

```
CPF DISPLAY DECISION
│
├── Is the current screen flagged as `pii_authorized`?
│   └── NO  → Display masked: ***.NNN.NNN-**
│   └── YES → Is the current user role permitted for PII?
│             └── NO  → Display masked
│             └── YES → Display full CPF with "PII Visible" indicator
│
CNPJ DISPLAY DECISION
│
├── Is the current screen flagged as `pii_authorized`?
│   └── NO  → Display masked: **.NNN.NNN/NNNN-**
│   └── YES → Is the current user role permitted for PII?
│             └── NO  → Display masked
│             └── YES → Display full CNPJ with "PII Visible" indicator
```

**RULE:** The “PII Visible” indicator must be persistent for the duration of the session on any screen showing unmasked data. It must not be dismissible.

**ANTI-PATTERN:** Unmasking PII data without a visible session-level indicator. Silent unmasking violates the audit trail requirement.

---

### 7.3 Credit Decision Provenance

Every credit decision rendered in the UI must include the following provenance block, regardless of artifact type used:

```
Decision:    [approved | rejected | pending_review | requires_human]
Policy:      [policy_name + policy_version]
Generated:   [DD/MM/AAAA HH:MM]
Source data: [bureau_name + consultation_date]
Verified by: Liquid OS Policy Engine  |  Audit: [audit_id]
```

**RULE:** The audit_id in the provenance block must be a clickable link that navigates directly to the audit trail entry.

**RULE:** If any field in the provenance block is unavailable, the decision must not be rendered. The UI must show a “Decision data incomplete — contact support” error state instead.

**ANTI-PATTERN:** Rendering a credit decision as a simple text label (e.g., “Approved”) without the full provenance block. This is a compliance violation.

---

### 7.4 Audit Trail Accessibility

**RULE:** For every entity that has participated in a financial decision, the audit trail must be reachable via a single persistent action:

- On DetailPage: always present as a tab (“Audit”)
- On ArtifactDocument containing a decision: always present as a link in the footer
- On List views: accessible via the row action menu (“View Audit”)
- In Chat responses: always present as the last element when a decision artifact is rendered

**RULE:** The audit trail view must show: timestamp, actor (human or agent), action taken, data state before and after, policy version applied. This is non-negotiable for any `approved`, `rejected`, or `requires_human` event.

---

### 7.5 Anti-Patterns Summary for Regulated Context

**ANTI-PATTERN:** Displaying any credit score as a standalone number without source and timestamp.

**ANTI-PATTERN:** Rendering financial values without BRL formatting (R$ prefix, dot separators, comma decimal).

**ANTI-PATTERN:** Showing a bureau data block without a consultation date indicator.

**ANTI-PATTERN:** Allowing a credit decision to be shown without the “Verified by Liquid” or “Policy Decision” badge.

**ANTI-PATTERN:** Making the audit trail accessible only via a settings page or footer link. It must be contextual to the entity.

**ANTI-PATTERN:** Displaying an unmasked CPF/CNPJ without a persistent PII Visible indicator for the session.

**ANTI-PATTERN:** Allowing hallucinated or unverified values to appear in credit score fields. Credit score fields are deterministic only — → Regulated Market Principle (#5).

---

*End of 4.3 - Behavioral UX Architecture*