# 4 6 - Interaction Model Behavioral

# 4.6 Behavioral Spec — Interaction Model

This document extends `4.6-interaction-model.md` with explicit behavioral rules for agent response composition, artifact anti-aglomeration, conversation states, human-in-the-loop checkpoints, Orbital Agent external interactions, and cost transparency. It is written for consumption by AI agents and UI implementers alike.

---

## 1. Agent Response Composition

When an agent must respond, it must select format before generating content. The following decision tree is normative.

```
AGENT RESPONSE DECISION TREE
─────────────────────────────
Did the user request an action (execute / create / analyze)?
│
├── YES → Is the action complete?
│         │
│         ├── YES → What is the result shape?
│         │         ├── Single number or metric        → ArtifactKPI
│         │         ├── Collection of rows/records     → ArtifactDatatable or ArtifactList
│         │         ├── Comparison, trend, distribution → ArtifactChart + Text (interpretation)
│         │         ├── Structured prose / report      → ArtifactDocument
│         │         └── Multiple result shapes         → see Section 2 (composition rules)
│         │
│         └── NO (in progress) → status message (role=agent) + progress indicator
│                                 NO artifacts until completion
│
├── NO → Did the user ask a question?
│         │
│         ├── Conversational / factual             → Text (role=agent), no artifact
│         ├── Answer involves a metric or number   → ArtifactKPI + Text (context, 1–2 sentences)
│         ├── Answer involves trend / comparison   → ArtifactChart + Text (interpretation, mandatory)
│         └── Answer is long or structured         → ArtifactDocument (standalone)
│
└── NO → Is this a proactive agent suggestion?
          ├── HIGH urgency (risk, liquidity breach) → Alert inline + Inbox notification
          ├── MEDIUM urgency (opportunity)          → Suggestion chip in inputArea
          └── LOW urgency (insight, FYI)            → Text (role=agent) surfaced at next
                                                       relevant context point, not immediately
```

**RULE:** The agent must resolve this tree before generating any response. Format selection is not optional or post-hoc.

**ANTI-PATTERN:** Generating a response and then wrapping it in an artifact type because it “looks better.” Artifact type must be determined by result shape, not aesthetics.

---

## 2. Artifact Composition Rules (Anti-Aglomeration)

### 2.1 The Single-Context Rule

**RULE:** Every set of artifacts in a single agent response must address ONE question or ONE analytical context. If the agent is implicitly answering two separate questions, it must produce TWO separate responses, each with its own artifact set.

**ANTI-PATTERN:** User asks “What is the current LTV?” and the agent returns a KPI (LTV), a chart (portfolio trend for the last 6 months), a datatable (all loans), and a document (credit policy summary). These are four different questions bundled into one response.

### 2.2 Combination Rules

The following table defines which artifact combinations are permitted. “Anchor” is the primary artifact; additional artifacts are secondary.

| Anchor Artifact | Permitted Combinations | Forbidden Combinations |
| --- | --- | --- |
| `ArtifactKPI` (single) | + `ArtifactChart` (sparkline or single-trend only) | + `ArtifactDatatable` (excess detail defeats KPI purpose) |
| `ArtifactKPI` (group, 2–4 KPIs) | + `Text` (context sentence) | + `ArtifactChart` (visual competition; use separately) |
| `ArtifactChart` | + `Text` (interpretation, mandatory — see 2.4) | + Another `ArtifactChart` of the same metric type |
| `ArtifactDatatable` | + `ArtifactChart` (visual summary of the table data only) | + `ArtifactKPI` (redundant; KPI values visible in table) |
| `ArtifactDocument` | standalone, or + `ArtifactDatatable` (as appendix) | + `ArtifactChart` (distraction from narrative flow) |
| `ArtifactList` | + `Text` (brief framing) | + `ArtifactDatatable` (pick one structured format) |

### 2.3 Artifact Count — Intent-Driven

The number of artifacts in a response is governed by **user intent**, not a hard cap.

| User intent | Expected scope | Artifact count |
| --- | --- | --- |
| Conversational question (“what’s the LTV today?”) | Single metric | 1–2 |
| Focused analysis (“analyze credit for CPF X”) | Canonical sequence | 3–5 |
| Full dashboard request (“build a portfolio dashboard”) | Complete panel | Unlimited — follow Section 4.7 composition rules |
| Unprompted proactive response | Minimal | 1–2 max |

**RULE:** Artifact count must match intent. The failure mode is not “too many artifacts” — it is **artifacts without a unifying question**. Every artifact in a response must contribute to answering the same central question.

**ANTI-PATTERN:** A “comprehensive dashboard” response generated without a user request — or a response where artifact 4 answers a different question than artifact 1. That is the canonical agglomeration failure.

**RULE:** For full dashboard requests, follow the `DASHBOARD_GENERATION_PROTOCOL` in `4.7-dashboard-context.md §7`. For bounded responses (analysis, question), use the canonical sequences in §2.5.

### 2.4 Autonomous Chart Rule

**RULE:** A `ArtifactChart` MUST NEVER appear in a response without an accompanying `Text` block (role=agent) that contains at minimum:
- One sentence describing what the chart shows.
- One sentence indicating what the user should pay attention to (the signal, not just the label).

**ANTI-PATTERN:**

```
[ArtifactChart: "LTV Trend — Last 12 Months"]
(no text)
```

**Correct:**

```
[ArtifactChart: "LTV Trend — Last 12 Months"]

Text (role=agent): "The chart shows average LTV across active operations over the
last 12 months. Note the uptick in Q1 2026 — this correlates with the new
residential tranche onboarded in February and has not yet normalized."
```

### 2.5 Canonical Sequences for Complex Analyses

When a task type is known, the agent must follow the canonical sequence. Deviating requires an explicit reason in the agent’s reasoning trace.

**Credit Analysis**

```
1. ArtifactKPI       → credit score + primary risk signal
2. ArtifactDocument  → narrative analysis (income, exposure, bureau summary)
3. ArtifactDatatable → raw bureau data (as appendix)

FORBIDDEN: ArtifactChart in credit analysis responses.
Reason: Credit decisions are point-in-time; trend charts imply
false precision and distract from the narrative judgment.
```

**Portfolio Overview**

```
1. ArtifactKPI (group, max 4) → primary portfolio metrics
2. ArtifactChart              → single trend (e.g., delinquency rate over time)
3. ArtifactDatatable          → operation-level detail

For bounded responses (not full dashboard requests).
```

**Market Intelligence**

```
1. ArtifactChart   → primary market trend (single, well-labeled)
2. ArtifactKPI     → 2–3 standout numbers from the data
3. ArtifactDocument → interpretation and recommended actions

Text (interpretation) accompanies the chart per rule 2.4.
```

**Repasse Status Update**

```
1. ArtifactKPI    → current stage / stage name
2. ArtifactDocument → next steps and blockers

FORBIDDEN: ArtifactChart in repasse status responses.
Reason: Repasse is a process state, not a data analysis.
Charts imply quantitative comparison where none is meaningful.
```

---

## 3. Agent Conversation States

The agent exists in one of the following states at all times. The UI must reflect the current state visually and behaviorally.

| State | Trigger | UI Representation | inputArea |
| --- | --- | --- | --- |
| `idle` | No active task | Normal chat view | Enabled |
| `thinking` | LLM processing user input | Typing indicator (3 animated dots, `ds/LoadingDots`) | Disabled |
| `planning` | Agent decomposing task into steps | “Analyzing…” label + agent icon animation | Disabled |
| `executing_tool` | Agent calling a skill or connector | “Consulting [connector name]…” + inline spinner (`ds/Spinner`, size=sm) | Disabled |
| `waiting_approval` | Policy triggered human-in-loop checkpoint | `ApprovalCard` rendered inline; full input block | **Blocked** |
| `streaming` | Agent generating and streaming response | Text renders incrementally (token stream) | Disabled |
| `error` | Agent or tool call failed | Inline error chip (`ds/Chip`, variant=error) + retry CTA | Enabled |
| `done` | Response complete | Normal state restored | Enabled |

### 3.1 State Rules

**RULE:** The user must NEVER be left without a visible indicator that the agent is working. Transitions from `thinking` → `planning` → `executing_tool` must each update the UI label. Silence is not an acceptable loading state.

**RULE:** States `planning` and `executing_tool` must display the specific action being taken, not a generic spinner. Transparency into what the agent is doing at each step is required by Product Constitution C7.

**RULE:** State `waiting_approval` MUST block the inputArea entirely. The user cannot send a new message until they explicitly Approve, Reject, or request more information via the `ApprovalCard`. This is not a UI preference — it is an integrity constraint. Allowing new input while an approval is pending creates a race condition in the agent execution graph.

**ANTI-PATTERN:** Showing `thinking` for the entire duration of a multi-step task. Each sub-step should transition the label so the user understands progress.

### 3.2 State Transition Diagram

```
idle ──[user sends message]──► thinking
                                  │
                         ┌────────▼────────┐
                         │    planning      │
                         └────────┬────────┘
                                  │
                    ┌─────────────▼──────────────┐
                    │      executing_tool         │◄─── (loops per tool call)
                    └─────────────┬──────────────┘
                                  │
                    ┌─────────────▼──────────────┐
              ┌─YES─┤   policy approval needed?   │
              │     └─────────────┬──────────────┘
              │                   │ NO
              ▼                   ▼
    waiting_approval          streaming
              │                   │
    [user acts]                   ▼
              │                  done ──► idle
              ▼
    [approved] ──► executing_tool (continues)
    [rejected] ──► done (with rejection message) ──► idle
              │
    [error at any point] ──► error ──► idle (retry available)
```

---

## 4. Human-in-the-Loop: Approval Specification

### 4.1 Mandatory Approval Triggers

The following conditions require the agent to pause and issue an `ApprovalCard` before proceeding. These are non-negotiable; no policy configuration can bypass them at the product level.

| Trigger | Condition | Notes |
| --- | --- | --- |
| Credit decision | Decision amount or exposure exceeds policy threshold | Threshold is channel-specific (see Policy Builder) |
| Repasse submission | Any submission of a package to a banking partner | Irreversible once received by bank system |
| Irreversible action | DELETE, SUBMIT, PUBLISH operations on any entity | Includes document submission and status finalization |
| Cost threshold | Estimated working units for action > configured limit | Default limit defined in Org Settings |
| Orbital Agent — CPF/CNPJ action | Any action involving retrieval, submission, or processing of a third-party CPF or CNPJ via Orbital Agent | Applies even if the requester is an internal analyst |
| Bulk operation | Any operation affecting > N records simultaneously | N defined per role in Policy Builder |

### 4.2 ApprovalCard Format

The `ApprovalCard` is rendered inline in the chat channel. Its structure is fixed and must not be abbreviated, even for low-stakes approvals.

```
┌─ ApprovalCard ───────────────────────────────────────────────┐
│                                                               │
│  CONTEXT                                                      │
│  2–3 sentences summarizing what happened up to this point.   │
│  Written in plain language. No jargon. Assumes the approver  │
│  may not have read the full conversation.                     │
│                                                               │
│  ACTION REQUESTED                                             │
│  Exactly one specific action. No compound statements.        │
│  Example: "Submit repasse package #RP-2847 to Banco          │
│  Bradesco via the CEF API connector."                         │
│                                                               │
│  CONSEQUENCE                                                  │
│  If approved:  [what happens, concrete and specific]         │
│  If rejected:  [what the agent will do instead or stop]      │
│                                                               │
│  POLICY REFERENCE                                             │
│  Policy name + link to Policy Builder entry.                 │
│  Example: "Repasse Submission Gate — Policy #P-014"          │
│                                                               │
│  DATA                                                         │
│  Minimum data required for the decision.                     │
│  Max 5 fields. If more data is needed, use [Request info].   │
│  Example:                                                     │
│    Operation ID:   OP-9923                                    │
│    Amount:         R$ 1.240.000                               │
│    Bank:           Banco Bradesco                             │
│    Prepared by:    Credit Agent v2.1                         │
│    Ready since:    2026-04-09 14:32                           │
│                                                               │
│  ┌──────────┐  ┌──────────────┐  ┌──────────────────────┐   │
│  │ Approve  │  │    Reject    │  │   Request more info   │   │
│  │(primary) │  │(destructive  │  │     (secondary)       │   │
│  │          │  │   ghost)     │  │                       │   │
│  └──────────┘  └──────────────┘  └──────────────────────┘   │
└───────────────────────────────────────────────────────────────┘
```

**RULE:** The `CONSEQUENCE / If rejected` field must always be populated. The approver must understand what happens if they say no — not just what happens if they say yes.

**RULE:** The `DATA` section must contain only decision-relevant fields. Listing all available metadata is an anti-pattern that buries the signal.

**ANTI-PATTERN:** An ApprovalCard with `Action requested: "Process the operation"`. This is too vague. The action must name the specific entity, the specific operation, and the specific external system affected.

### 4.3 Post-Approval Behavior

After the user acts on an ApprovalCard:

- **Approved:** Agent transitions back to `executing_tool` and continues from the paused step. A brief confirmation message (role=agent) is emitted: “Approved. Continuing with [action].”
- **Rejected:** Agent emits a status message: “Understood. [Action] has been cancelled. [What the agent will do instead, if applicable].” Agent transitions to `done`.
- **Request info:** Agent surfaces a text input scoped to the approval. User input is processed in context of the pending decision only. inputArea remains blocked for general messages.

---

## 5. Orbital Agent: External Interaction Representation

The Orbital Agent communicates with external contacts (buyers, clients, guarantors) via WhatsApp. Internal users monitor these interactions through Liquid UI without participating directly.

### 5.1 Representing an Active Orbital Conversation

When the Orbital Agent has an active outbound conversation, the originating Task or Channel must display:

```
┌─ OrbitalThread (inline in Task timeline) ──────────────────┐
│  Contact: [name or masked phone if PII rules apply]        │
│  Channel: WhatsApp                                         │
│  Status: Active / Waiting for reply / Timed out           │
│  Started: [timestamp]                                      │
│  Last message: [timestamp]                                 │
│  Objective: [what data/action the agent is collecting]     │
│  [View transcript] (opens read-only side panel)           │
└────────────────────────────────────────────────────────────┘
```

**RULE:** The transcript side panel is read-only for all internal roles. Internal users CANNOT inject messages into an active Orbital conversation. Any intervention requires the Orbital Agent to be explicitly paused via a dedicated control, which triggers an ApprovalCard.

### 5.2 Returning Data to the Originating Context

When the Orbital Agent completes a collection task, results are pushed back to the originating Task or Channel as a structured artifact:

- Collected documents → `ArtifactList` with download links, filed under the Task’s Documents tab.
- Collected data fields (e.g., income declaration, marital status) → `ArtifactDatatable` with field/value/source/timestamp columns.
- Orbital summary → `Text` (role=agent) stating what was collected, what is still missing, and recommended next step.

The OrbitalThread status updates to `Completed` and the timeline entry collapses to a summary row.

### 5.3 Internal Monitoring Without Interference

**RULE:** Internal users (Analyst, Coordinator) may open the read-only transcript at any time. Viewing the transcript does not interrupt the Orbital Agent’s execution or send any signal to the external contact.

The Task timeline shows Orbital activity as a distinct event type (`event_type: orbital_activity`) using a visual treatment that differentiates it from internal agent actions (see `ds/TimelineEvent`, variant=orbital).

### 5.4 Timeout Behavior

If the external contact does not respond within the configured timeout window:

1. OrbitalThread status updates to `Timed out`.
2. Agent emits an inline alert (role=agent) to the Task/Channel: “No response from [contact] after [duration]. Objective: [what was sought]. Recommended action: [retry / escalate / proceed without data].”
3. The agent does NOT automatically retry. Retry requires explicit instruction from the internal user or a policy rule configured in the Policy Builder.
4. The timeout event is logged in the Task audit trail with timestamp and contact identifier.

**ANTI-PATTERN:** Silently marking a contact as unresponsive and continuing the workflow as if data was collected. Missing data must surface explicitly before any downstream decision is made.

---

## 6. Cost Transparency Rules

Governed by Product Constitution C9. Cost visibility is not optional.

### 6.1 When to Show Cost

| Moment | Condition | Display |
| --- | --- | --- |
| Before action execution | Estimated cost > 0 working units | Inline cost estimate in agent message: “This action will use ~X working units” |
| Before action execution | Estimated cost > approval threshold | ApprovalCard must include cost in DATA section |
| After action completion | Always, for actions that consumed units | Cost logged in Task audit trail |
| In ApprovalCard | Action involves any tool call or external connector | Cost field mandatory in DATA section |

### 6.2 Cost Display Format

Inline (pre-execution):

```
Text (role=agent):
"Analyzing 47 operations against current bureau data.
Estimated cost: ~12 working units. Proceed?"
[Confirm] [Cancel]
```

For low-cost actions below the silent execution threshold, the agent executes without confirmation but still logs the cost in the audit trail.

**RULE:** The cost estimate must always use the `~` prefix to communicate it is an estimate, not a guaranteed figure. Displaying a precise number implies a precision the system does not guarantee.

### 6.3 Approval Thresholds

| Cost Range | Behavior |
| --- | --- |
| 0–5 working units | Silent execution. Cost logged in audit. No user prompt. |
| 6–20 working units | Inline cost notice before execution. Single [Confirm] / [Cancel]. |
| > 20 working units | Full ApprovalCard required. Cost appears in DATA section. |
| Any irreversible action | Full ApprovalCard regardless of cost. |

Thresholds above are defaults. Org admins may lower (but not raise) these thresholds via Org Settings. Per-role overrides are configurable in the Policy Builder.

### 6.4 Cost in Task Audit Trail

Every action that consumed working units must appear in the Task’s audit trail with the following fields:

| Field | Description |
| --- | --- |
| `timestamp` | When the action completed |
| `actor` | Agent name and version (e.g., `credit-agent@2.1`) |
| `action` | Human-readable description of what was done |
| `units_estimated` | Estimate shown to user before execution |
| `units_actual` | Actual consumption after completion |
| `approved_by` | User ID if ApprovalCard was triggered; `auto` if silent |

**RULE:** If `units_actual` exceeds `units_estimated` by more than 20%, the agent must emit a post-hoc notification to the Task channel: “Note: this action used X working units, above the estimated Y. Reason: [brief explanation].”

**ANTI-PATTERN:** Aggregating cost entries (e.g., “Batch: 15 actions, 47 units total”). Each action must have its own audit entry to support individual review and dispute resolution.

---

## Appendix: Quick-Reference Anti-Pattern Index

| # | Anti-Pattern | Rule Reference |
| --- | --- | --- |
| AP-01 | Chart rendered without interpretation text | Section 2.4 |
| AP-02 | Single response answering two unrelated questions | Section 2.1 |
| AP-03 | Artifacts without a unifying question (agglomeration) | Section 2.3 |
| AP-04 | KPI + Datatable in same response (redundancy) | Section 2.2 |
| AP-05 | Two charts of the same metric type in one response | Section 2.2 |
| AP-06 | ApprovalCard with vague “Action requested” | Section 4.2 |
| AP-07 | ApprovalCard missing “If rejected” consequence | Section 4.2 |
| AP-08 | inputArea enabled during `waiting_approval` state | Section 3.1 |
| AP-09 | Orbital Agent retry triggered without explicit instruction | Section 5.4 |
| AP-10 | Silent execution of action above cost threshold | Section 6.3 |
| AP-11 | Aggregated cost entries in audit trail | Section 6.4 |
| AP-12 | Chart in credit analysis response | Section 2.5 |
| AP-13 | Chart in repasse status response | Section 2.5 |
| AP-14 | User left without progress indicator during multi-step task | Section 3.1 |