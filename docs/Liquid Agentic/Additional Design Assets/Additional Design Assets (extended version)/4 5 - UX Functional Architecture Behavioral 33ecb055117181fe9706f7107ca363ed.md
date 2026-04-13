# 4 5 - UX Functional Architecture Behavioral

# Liquid OS — UX Functional Architecture: Behavioral Specification

This document instructs AI agents and developers on how each view in Liquid OS behaves, which components to use in each context, and what the interaction rules are per view. It does not duplicate the design system — refer to `ds/rulebook.md` and `ds/components/` for component definitions, patterns, and visual tokens.

---

## 1. Behavioral Specification by View

### 1.1 Tasks / Chat

> This is the most critical view in the product. See Section 7 for the full detailed specification.
> 

**Purpose:** The user sends natural language requests to agents, reviews AI-generated artifacts, provides approvals, and manages active tasks end-to-end.

**Primary layout pattern:** `AIWorkflow` (see `ds/rulebook.md` — tree #03, branch: agent-initiated + human-in-the-loop)

**Default state:** Opens to the last active task. If no prior task, shows the empty state.

**Empty state:**
- Headline: “What would you like to work on?”
- Subtext: “Start a credit analysis, open a repasse, or ask your agent anything.”
- CTA: “New Task” button + 3 suggested prompts contextual to user role (Analyst → credit; Coordinator → repasse; Executive → market intel)

**Loaded state — visual hierarchy:**

```
1. MessageList (primary content — occupies most vertical space)
   └─ grouped by date
   └─ agent messages carry ArtifactContainers when applicable
   └─ user messages right-aligned, agent messages left-aligned
2. InputArea (pinned bottom)
3. ContextPanel (right rail — collapsible)
   └─ TaskInfo header (title, status, assignee, due date)
   └─ RelatedEntities list (linked Channel, Documents, Policies)
   └─ Timeline (audit trail of events on this task)
```

**Agent behavior:** Fully active. The agent is the primary actor in this view. Proactively:
- Notifies when a policy evaluation is triggered
- Surfaces missing data requirements before analysis starts
- Proposes next steps after delivering an artifact
- Sends an approval request when human sign-off is required

**Key interactions:**
1. Send a message or instruction to the agent
2. Review and approve/reject an artifact
3. Invoke a skill (`/skill-name`) or mention an agent (`@agent-name`)
4. Attach a file for analysis
5. Navigate to a related entity (Channel, Policy, Document) from the ContextPanel

**Forbidden interactions:**
- No direct Billing or Connector configuration from this view
- No bulk task management operations (use Inbox or Channels for that)
- No agent configuration or policy editing inline (link out to Agent Config / Policy Builder)
- No global search bar replacement — this is a single-task context

---

### 1.2 Channels

**Purpose:** The user manages multi-step workflows that span multiple tasks and participants. Each channel is a workspace for a specific operation (e.g., a repasse deal, a liquidity operation).

**Primary layout pattern:** `ListPage` (kanban) + `DetailPage` (channel detail, when opened)

**Default state:** Kanban board showing all active channels the user has access to, grouped by status column (e.g., Pending, In Progress, Awaiting Approval, Closed).

**Empty state:**
- Headline: “No active channels”
- Subtext: “Channels are created automatically when an operation is initiated, or manually for coordinated workflows.”
- CTA: “Create Channel” (visible only to roles with create permission)

**Loaded state — visual hierarchy (Kanban):**

```
1. Column headers (status labels + count badge)
2. ChannelCards (title, type, assignee avatars, last activity timestamp, open task count)
3. Quick filters bar (by type, by assignee, by date)
```

**Loaded state — visual hierarchy (Channel Detail):**

```
1. Channel header (name, type badge, status, participants)
2. MessageFeed (threaded activity log — not a full chat; use Tasks for that)
3. TaskList sidebar (tasks linked to this channel, with status)
4. ArtifactList (documents and outputs produced in this channel)
5. ContextPanel (channel metadata, linked entities, policy status)
```

**Agent behavior:** Passive-to-reactive. The agent monitors channel status and:
- Posts automated status updates when linked tasks change state
- Flags blockers (e.g., missing document, expired policy rule)
- Triggers notifications when SLA thresholds are approaching

**Key interactions:**
1. Move a channel card between status columns (drag or status dropdown)
2. Open a channel to see its detail view
3. Create a new task within a channel context
4. View an artifact produced inside the channel
5. Add a participant to the channel

**Forbidden interactions:**
- No direct message input to agents in the channel feed (link to Tasks for that)
- No global dashboard editing from this view
- No policy creation inline (link to Policy Builder)

---

### 1.3 Dashboard Hub

**Purpose:** The user gets a high-level overview of the platform’s key metrics and navigates to specific dashboard views.

**Primary layout pattern:** `ListPage` (grid of dashboard cards) — see `ds/rulebook.md` tree #03

**Default state:** Grid of all dashboards the user has access to, ordered by last viewed. Featured dashboards (pinned by admin) appear first.

**Empty state:**
- Headline: “No dashboards available”
- CTA: “Request a Dashboard” (sends a message to the Admin agent) — visible to Viewer and Analyst roles
- CTA: “Create Dashboard” — visible to Admin only

**Loaded state — visual hierarchy:**

```
1. Pinned / Featured dashboards (top row, larger cards)
2. All dashboards grid (filterable by domain: Credit, Repasse, Liquidity, Market)
3. Quick access: "Last viewed" row
```

**Agent behavior:** Passive. No proactive agent in this view. Agent access available via global input if user navigates to a specific dashboard.

**Key interactions:**
1. Open a dashboard by clicking its card
2. Pin/unpin a dashboard (Admin only)
3. Filter dashboards by domain
4. Share a dashboard link
5. Navigate to the underlying data source (linked Channel or Report)

**Forbidden interactions:**
- No data editing or form submission in this view
- No task creation from Dashboard Hub (use Tasks for that)

---

### 1.4 Dashboard View (individual dashboard)

**Purpose:** The user analyzes metrics and trends for a specific operational domain.

**Primary layout pattern:** `DetailPage` (read-only data layout) — see `ds/rulebook.md`

**Default state:** Last saved view configuration (filters, date range, selected segments). If no prior configuration, loads the dashboard default.

**Empty state:**
- Displayed per widget: “No data available for the selected period.”
- CTA at widget level: “Adjust filters”

**Loaded state — visual hierarchy:**

```
1. Dashboard header (title, last updated timestamp, filter bar)
2. KPI summary row (top-line metrics — always visible)
3. Chart grid (responsive, defined by dashboard template)
4. Drill-down panel (opens on chart click — shows underlying records)
```

**Agent behavior:** Available on demand. The user can open a Chat panel attached to the current dashboard context. The agent:
- Answers questions about the data displayed
- Can generate a narrative summary of the current metrics
- Can trigger a new Task from an insight (e.g., “Create credit review for this borrower”)

**Key interactions:**
1. Change date range or filter
2. Click a chart element to drill down
3. Export data or chart as PDF/CSV
4. Ask the agent a question about the data
5. Navigate to a linked Channel or Task from a data record

**Forbidden interactions:**
- No data mutation from any dashboard view
- No policy editing or agent configuration

---

### 1.5 Agents List

**Purpose:** The user browses available agents, understands their capabilities, and navigates to configure them.

**Primary layout pattern:** `ListPage` — see `ds/rulebook.md`

**Default state:** Full list of agents provisioned for the organization, grouped by domain (Credit, Repasse, Liquidity, Market, System).

**Empty state:**
- Headline: “No agents configured”
- CTA: “Add Agent” (Admin only)

**Loaded state — visual hierarchy:**

```
1. Agent cards (name, domain badge, status indicator: active/inactive, last used timestamp)
2. Filter bar (by domain, by status)
3. Search field (by agent name or capability keyword)
```

**Agent behavior:** None in list view. Agents are subjects here, not actors.

**Key interactions:**
1. Open an agent’s detail/config page
2. Enable or disable an agent (Admin only)
3. Filter by domain or status
4. View agent usage stats (last active, task count)
5. Trigger a test task from the agent card (Admin only)

**Forbidden interactions:**
- No inline editing of agent configuration in list view (navigate to Agent Config)
- No policy editing from this view

---

### 1.6 Agent Config

**Purpose:** The Admin or Operating user configures an agent’s behavior, linked policies, available skills, and permissions.

**Primary layout pattern:** `FormFlow` with tabbed sections — see `ds/rulebook.md`

**Default state:** Opens to the last edited tab. Default first-open: “Overview” tab.

**Empty state:** N/A — Agent Config always has a loaded agent as context.

**Loaded state — visual hierarchy:**

```
Tabs:
1. Overview (name, description, domain, status toggle)
2. Skills (list of enabled skills, skill parameters, test invocation)
3. Policies (linked policy rules — read-only; link to Policy Builder to edit)
4. Permissions (which roles can invoke this agent, in which views)
5. Logs (recent execution history, error log)
```

**Agent behavior:** None — this is a configuration surface.

**Key interactions:**
1. Edit agent name, description, or domain
2. Enable/disable individual skills
3. View linked policies (navigate to Policy Builder to change)
4. Update permission settings
5. View execution logs and errors

**Forbidden interactions:**
- No direct policy editing inline (always links to Policy Builder)
- No billing actions from this view
- Limited role: Analysts can view Overview and Skills tabs only; all others hidden

---

### 1.7 Policy Builder

**Purpose:** The Admin or Operating user defines rules that govern agent behavior — thresholds, approvals, restrictions, and compliance constraints.

**Primary layout pattern:** `FormFlow` (structured rule builder) — see `ds/rulebook.md`

**Default state:** List of existing policies. Click to open detail. “New Policy” button top-right.

**Empty state:**
- Headline: “No policies defined”
- Subtext: “Policies control how agents behave and when they require human approval.”
- CTA: “Create First Policy” (Admin only)

**Loaded state — visual hierarchy:**

```
Policy List (left panel):
  - Policy name, domain badge, status (active/draft/archived), last modified

Policy Detail (right panel / modal):
  1. Policy header (name, description, domain, status toggle)
  2. Trigger conditions (when this policy fires)
  3. Rule definition (conditions + actions — structured form, not freeform text)
  4. Approval requirements (who must approve, timeout behavior)
  5. Affected agents (which agents this policy applies to)
  6. Audit log (when this policy fired and with what outcome)
```

**Agent behavior:** None in this view. Policies are authored by humans; agents are subjects.

**Key interactions:**
1. Create a new policy
2. Edit an existing policy’s rules or approval requirements
3. Activate, deactivate, or archive a policy
4. View the audit log for a policy
5. Link/unlink a policy to a specific agent

**Forbidden interactions:**
- Analyst role: view only — no create or edit actions
- Viewer role: hidden entirely
- No task creation from Policy Builder
- No agent configuration inline

---

### 1.8 Inbox

**Purpose:** The user reviews all notifications, approval requests, and flagged items that require their attention across all views.

**Primary layout pattern:** `ListPage` (unified feed) — see `ds/rulebook.md`

**Default state:** Sorted by urgency + recency. Unread items appear first.

**Empty state:**
- Headline: “You’re all caught up”
- No CTA needed.

**Loaded state — visual hierarchy:**

```
1. Filter tabs: All | Approvals | Mentions | Updates | System
2. Item list:
   - Icon (item type)
   - Source context (Task name / Channel name / Agent name)
   - Summary text
   - Timestamp
   - Action buttons inline (Approve / Reject / View / Dismiss)
3. Bulk actions bar (appears when items selected): Mark read, Dismiss
```

**Agent behavior:** None directly. Inbox is a passive notification surface. Agent-generated items arrive here (approval requests, policy evaluations, alerts).

**Key interactions:**
1. Approve or reject an agent’s request inline
2. Navigate to the source Task or Channel from an item
3. Filter by type (Approvals, Mentions, etc.)
4. Mark items as read or dismiss
5. Bulk-dismiss a category

**Forbidden interactions:**
- No task creation from Inbox
- No policy editing or agent configuration inline
- No dashboard access from Inbox

---

## 2. Entity → View Mapping

| Entity | Primary View | Also Appears In |
| --- | --- | --- |
| Task | Tasks / Chat | Channels (task list rail), Inbox (approval items), Dashboard (drill-down records) |
| Channel | Channels | Inbox (status updates), Dashboard Hub (linked metric cards), Home |
| Agent | Agents List / Agent Config | Tasks (via `@mention` invocation), Settings (system agents) |
| Policy | Policy Builder | Tasks (evaluation result inline), Channels (compliance status badge), Agent Config (linked policies tab) |
| Dashboard | Dashboard Hub | Home / Dashboard Hub (card grid) |
| Routine | Routines | Agents (triggered by agent schedule), Channels (automated updates) |
| Artifact | Produced in Tasks | Channels (ArtifactList rail), Files / KB (stored outputs) |
| Notification | Inbox | Sidebar badge (all views) |

---

## 3. Navigation and Deep Linking Rules

### 3.1 Task → Channel

- **When:** A task is explicitly linked to a channel operation (e.g., a credit task linked to a repasse channel), or when the user selects “Add to Channel” from the task’s ContextPanel.
- **How:** Selecting “Add to Channel” opens a channel picker modal. Once linked, a backlink appears in both the task ContextPanel and the channel’s TaskList rail.
- **Deep link format:** `/tasks/{taskId}?channel={channelId}` — loads the task with the channel pre-selected in ContextPanel.

### 3.2 Channel → Task

- **When:** The user clicks a task item in the channel’s TaskList rail.
- **How:** Opens the task in the Tasks / Chat view, with the originating channel surfaced in ContextPanel. The back navigation returns to the channel detail.
- **Deep link format:** `/channels/{channelId}/tasks/{taskId}`

### 3.3 Credit Result Artifact → Audit Trail

- **When:** The user clicks the audit icon on a credit analysis artifact.
- **How:** Opens an overlay panel (not a navigation) showing the full policy evaluation log, data inputs, agent decision steps, and human approval timestamps. Sourced from `ds/components/ia-views/` — ArtifactContainer audit mode.
- **Deep link format:** `/tasks/{taskId}/artifacts/{artifactId}/audit`

### 3.4 Liquidity Trigger → Liquidity Channel

- **When:** A routine or market event triggers a liquidity operation automatically.
- **How:** The system creates a Liquidity Channel automatically and routes the trigger event as the first message in that channel. The user receives an Inbox notification with a direct link to the new channel.
- **Transition:** Automatic — no user action required to initiate. User enters the channel from Inbox or from Channels list (Liquidity type filter).
- **Deep link format:** `/channels/{channelId}?trigger={eventId}`

### 3.5 Agent `@mention` → Agent Detail

- **When:** The user clicks on an `@agent-name` mention in a task message.
- **How:** Opens a lightweight popover with agent name, description, status, and a “View Config” link. “View Config” navigates to Agent Config (`/agents/{agentId}/config`). The Tasks view remains in place — this is not a full navigation.

---

## 4. Persistent Context Rules

### 4.1 Chat State (Tasks view)

- **Scroll position:** Persists per task. Returning to a task restores the last scroll position.
- **Draft input:** Text in the InputArea is saved as a draft per task. Navigating away and returning restores the draft. Draft is discarded only upon send or explicit clear.
- **ContextPanel open/closed:** Persists per task (user’s last state for that task is remembered).

### 4.2 List Filters

| View | Filter persistence rule |
| --- | --- |
| Channels | Persists within the session. Cleared on logout. |
| Inbox | Persists within the session. Tab selection (All/Approvals/etc.) is remembered. |
| Agents List | Does not persist. Resets to default on each visit. |
| Dashboard Hub | Persists within the session (last domain filter remembered). |
| Policy Builder | Does not persist. Always opens to full list. |

### 4.3 Last Visited Channel

- Clicking “Channels” in the sidebar navigates to the Channels list (not the last visited channel).
- The last visited channel is surfaced as the first card in the “Recent” row within the Channels list.
- There is no auto-open behavior — users must explicitly select a channel.

### 4.4 Context Panel

| Trigger | Behavior |
| --- | --- |
| Opening a task for the first time | ContextPanel opens by default |
| Returning to a task | Restores last open/closed state |
| Mobile / narrow viewport | ContextPanel is closed by default; accessible via toggle button |
| Artifact approval pending | ContextPanel opens automatically and scrolls to approval item |
| Policy evaluation result arrives | ContextPanel opens automatically if closed |

---

## 5. Sidebar / Navbar Behavior

### 5.1 Always Visible Items (Client Side)

```
- Home / Dashboard Hub
- Tasks (Chat)
- Channels
- Inbox  (with notification badge)
- Agents
- Routines
- Files / KB
- Settings
```

### 5.2 Conditionally Visible Items

| Item | Condition |
| --- | --- |
| Dashboards | Visible only when at least one dashboard is provisioned for the user |
| Policy Builder | Visible to Admin and Operating roles only |
| Connectors | Visible to Admin and Operating roles only |
| Internal Channels (Operating Side) | Visible only when the user has an Operating role |
| Liquidity Board (Operating Side) | Visible only when the user has an Operating role |
| Agent Admin (Operating Side) | Visible only when the user has an Operating role |
| Billing Admin (Operating Side) | Visible only when the user has an Operating role |

### 5.3 Notification Badge

- Appears on: **Inbox** (primary), and on **Channels** when a channel has an unread update.
- Badge shows count of unread items capped at `99+`.
- Badge on Channels shows only when there are unread channel updates (not task updates — those route to Inbox).
- Badge clears when the user visits the view and the item is marked read (automatically on view, or manually).

### 5.4 Active State

- The active sidebar item is determined by the current URL path prefix.
- `/tasks/*` → Tasks active
- `/channels/*` → Channels active
- `/agents/*` → Agents active
- `/policy-builder/*` → Policy Builder active
- `/inbox` → Inbox active
- Active state uses the visual indicator defined in `ds/components/ui/` — sidebar nav token.

---

## 6. Permission Matrix by View and Role

| View | Admin | Analyst | Viewer | Operating |
| --- | --- | --- | --- | --- |
| Home / Dashboard Hub | full access | full access | full access | full access |
| Tasks / Chat | full access | full access | view only (no send) | full access |
| Channels | create / edit / close | create / edit | view only | create / edit / close |
| Inbox | full access | full access | full access | full access |
| Agents List | full access | view only | hidden | full access |
| Agent Config | full access | view Overview + Skills tabs only | hidden | full access |
| Policy Builder | create / edit / archive | view only | hidden | create / edit / archive |
| Connectors | full access | hidden | hidden | full access |
| Dashboards | create / edit | view only | view only | create / edit |
| Routines | create / edit | view / trigger | hidden | create / edit |
| Files / KB | full access | full access | view / download only | full access |
| Settings | full access | profile only | profile only | full access |
| Clients (Operating) | hidden | hidden | hidden | full access |
| Liquidity Board (Operating) | hidden | hidden | hidden | full access |
| Agent Admin (Operating) | hidden | hidden | hidden | full access |
| Billing Admin (Operating) | full access | hidden | hidden | full access |
| Internal Channels (Operating) | hidden | hidden | hidden | full access |

**Permission enforcement rules:**
- Hidden views must not appear in the sidebar or be reachable via direct URL (redirect to 403 page).
- “View only” means the user can read content but all write actions (buttons, forms, drag) are either hidden or disabled with a tooltip explaining the restriction.
- Permission checks happen server-side; UI state is a reflection, not the enforcement layer.

---

## 7. Full Specification: Tasks / Chat View

### 7.1 Layout and Proportions

```
[ Sidebar ]  [ MessageList (flex-grow) ]  [ ContextPanel (320px fixed, collapsible) ]
```

- MessageList occupies all remaining horizontal space after the sidebar and ContextPanel.
- ContextPanel is 320px wide on desktop. On viewports under 1280px wide, it collapses to a slide-over panel accessible via toggle.
- InputArea is pinned to the bottom of the MessageList column, not the full page width.
- Minimum usable MessageList width: 480px. Below this, ContextPanel is always collapsed.

### 7.2 MessageList Behavior

- **Scroll:** Infinite scroll upward (loads older messages in batches of 30). New messages appear at the bottom and auto-scroll only if the user is already at the bottom. If the user has scrolled up, a “New messages ↓” anchor badge appears.
- **Date grouping:** Messages are grouped by calendar date. Date separator labels (e.g., “Today”, “Yesterday”, “April 7”) appear as sticky headers while scrolling within that day’s group.
- **Message types rendered in MessageList:**
    - User messages (text, file references)
    - Agent messages (text, ArtifactContainers, status updates)
    - System events (policy triggered, approval requested, channel linked) — displayed as inline event rows, visually distinct from messages
- **Anchors:** Approval requests, policy evaluation results, and artifact deliveries generate anchor IDs. Deep links and Inbox notifications can scroll directly to these anchors.

### 7.3 ArtifactContainer (inline in agent messages)

Refer to `ds/components/ia-views/` for the component definition. Behavioral rules:

- Artifacts are rendered inline within the agent message thread, immediately after the agent’s explanatory text.
- Each artifact has: title, type badge (e.g., “Credit Report”, “Repasse Summary”), preview content, and an action bar.
- Action bar options vary by artifact type and status:
    - Pending approval: “Approve” / “Reject” / “Request Changes” buttons
    - Delivered: “View Full” / “Export” / “View Audit Trail”
    - Processing: shows agent thinking state (see 7.5)
- Clicking “View Full” opens the artifact in a full-width overlay (not a new page). The overlay has its own audit trail tab.
- Artifacts are never editable inline. Users can only approve, reject, or export.

### 7.4 InputArea Components

The InputArea renders at the bottom of the MessageList column and includes the following components (see `ds/components/ui/` for each):

| Component | Trigger | Behavior |
| --- | --- | --- |
| Text input | Default | Multi-line, grows to max 5 lines before scrolling internally |
| File attach | Paperclip icon / drag-drop | Accepts PDF, XLSX, DOCX, PNG, JPG. Max 50MB per file. Shows upload progress inline. |
| Skill invoke | `/` prefix in text input | Opens a slash-command picker filtered by typed characters. Shows skill name + description. |
| Agent mention | `@` prefix in text input | Opens an agent picker filtered by typed characters. Mentioning an agent routes the message to that agent specifically. |
| Send button | Always visible | Sends on click or `Cmd/Ctrl + Enter`. Disabled when input is empty and no files attached. |
| Voice input | Microphone icon | Optional — available when browser grants microphone permission. Transcribes to text input. |

**InputArea disabled state:** The InputArea is disabled (with explanatory tooltip) when:
- The task status is “Closed” or “Archived”
- The user’s role is Viewer

### 7.5 Agent “Thinking” and “Executing” States

- **Thinking:** A typing indicator (three animated dots) appears as an agent message bubble. Text label: “Agent is thinking…” Shown after the user sends a message and before the agent begins streaming a response.
- **Streaming response:** Agent text streams in word-by-word. The message bubble grows as text arrives.
- **Executing a skill or tool:** When the agent is performing a background action (e.g., fetching credit bureau data, running an analysis), a distinct execution status chip appears below the streaming text. Format:
    
    ```
    ⚙ Running: [skill name] — [elapsed time]s
    ```
    
    This chip updates in real time. On completion, it collapses into a small “View execution details” link.
    
- **Error state:** If the agent fails, the message ends with an error card (red border, error summary, “Retry” button).

### 7.6 Policy Evaluation — Inline Display

When an agent action triggers a policy evaluation, a system event row appears in the MessageList:

```
[Policy icon]  Policy evaluated: [Policy Name]
               Result: [Passed / Failed / Requires Approval]
               [View Policy Details →]
```

- “Passed” — green indicator, no further action required.
- “Failed” — red indicator. The agent stops its current action and posts an explanation message. The ContextPanel opens automatically showing the policy detail.
- “Requires Approval” — triggers the Human Approval Request flow (see 7.7).

### 7.7 Human Approval Requests

When an agent requires human approval, a structured approval card appears in the MessageList as a system event:

```
┌─────────────────────────────────────────────────────────┐
│  Approval Required                           [Pending]  │
│  Agent: [Agent Name]                                    │
│  Action: [Plain-language description of what the agent  │
│           wants to do]                                  │
│  Policy: [Policy Name triggering this requirement]      │
│  Context: [Link to relevant artifact or data]           │
│                                                         │
│  [Approve]   [Reject]   [Request Changes]               │
│                                                         │
│  Timeout: Approval required by [date/time]              │
└─────────────────────────────────────────────────────────┘
```

- The ContextPanel opens automatically when an approval card appears, scrolling to the TaskInfo header which shows “Awaiting Approval” status.
- The approval card remains visible and actionable in the message thread even after scrolling away. It does not disappear until actioned.
- “Request Changes” opens a text input inline on the card to provide instructions before rejecting.
- Approval actions are logged in the task’s audit trail (accessible via ContextPanel → Timeline tab).
- If timeout expires without action, the card updates to “Timed Out” and the agent receives an automatic rejection. An Inbox notification is sent to all task participants.

### 7.8 ContextPanel — Content Specification

The ContextPanel has three tabs:

**Tab 1: Info**

```
- Task title (editable inline by task owner)
- Status badge (Active / Awaiting Approval / Completed / Closed)
- Assignee (user or agent)
- Due date (if set)
- Created by / created at
- Channel link (if task is linked to a channel)
- Tags
```

**Tab 2: Related**

```
- Linked Channel (with status badge)
- Referenced Documents (files attached in this task)
- Produced Artifacts (list of all artifacts generated)
- Linked Policies (policies that have evaluated in this task)
- Mentioned Agents (with link to Agent Config for each)
```

**Tab 3: Timeline**

```
- Chronological log of all events in this task
- Includes: messages sent, agent executions, policy evaluations,
  approval requests and outcomes, file uploads, status changes
- Each entry: timestamp, actor (user name or agent name), event description
- "Export Audit Trail" button (PDF/CSV) — available to Admin and Operating roles only
```

**ContextPanel open/close rules:** See Section 4.4.

---

## 8. Chat Layout States — Dynamic Split Rules

The Tasks/Chat view does not have a fixed layout. It transitions between four states based on what the agent returns and what the user is doing. AI agents must understand these states to produce responses that trigger the correct layout — and developers must implement the transition logic accordingly.

### 8.1 The Four Layout States

| State | Chat Area | Artifact Area | When |
| --- | --- | --- | --- |
| **Chat-Only** | 100% width | Hidden | No artifact in current response; conversational exchange |
| **Split 50/50** | ~50% | ~50% | Single artifact or moderate complexity response |
| **Split 30/70** | ~30% | ~70% | Rich artifact (dashboard, datatable, document) or user explicitly expands |
| **Artifact-Max** | Collapsed (icon bar) | ~100% | User manually maximizes, or full dashboard generation requested |

### 8.2 Transition Triggers

**Chat-Only → Split 50/50**

Triggered automatically when the agent response contains:
- Any `ArtifactKPI`, `ArtifactChart`, `ArtifactList`, `ArtifactCard`, `ArtifactLink`, or `ArtifactInputRequest`
- A single artifact of any type paired with interpretation text

```
Agent returns ArtifactKPI or ArtifactChart or ArtifactList
└── Layout transitions to Split 50/50
      └── Chat panel: conversation history + new message
      └── Artifact panel: the artifact, full-height, scrollable
```

**Split 50/50 → Split 30/70**

Triggered automatically when the agent response contains:
- `ArtifactDatatable` (data-dense, needs more horizontal space)
- `ArtifactDocument` (long-form, needs reading room)
- `ArtifactGraph` (relational diagram, needs visual space)
- 3 or more artifacts in a single response
- A full dashboard composition (multiple artifact types together)

```
Agent returns ArtifactDatatable or ArtifactDocument or ArtifactGraph
└── Layout transitions to Split 30/70
      └── Chat panel: compressed but still readable (min 320px)
      └── Artifact panel: expanded, dominant
```

Also triggered manually when the user drags the split handle toward the chat side.

**Split 30/70 → Artifact-Max**

Triggered only by explicit user action:
- Clicking the “Expand” action in `ArtifactActions`
- User dragging the split handle fully to the left edge

In Artifact-Max, the chat is accessible via a floating button (bottom-left). Pressing it collapses back to the last split state.

**Any state → Chat-Only**

Triggered when:
- User starts a new task (resets layout)
- Agent response is purely conversational (Text role=agent, no artifacts)
- User manually collapses the artifact panel

**RULE:** Layout transitions must be animated (use `--duration-moderate` + `--ease-in-out`). Never snap between layouts without a transition.

**RULE:** The layout state persists within a task session. If the user is in Split 30/70 and sends a new message, the layout stays at 30/70 until a trigger changes it — it does not reset to Chat-Only on every send.

### 8.3 Layout Decision Tree for the Agent

Before generating a response, the agent should consider what layout it will trigger:

```
What am I returning?

├── Pure text (conversational answer, no data)
│     └── → Chat-Only layout. Keep response concise.

├── One metric or quick result
│     └── → ArtifactKPI → triggers Split 50/50
│           └── Pair with 1-2 sentences of interpretation

├── One visualization or structured list
│     └── → ArtifactChart or ArtifactList → triggers Split 50/50
│           └── Always include interpretation text (see 4.3 §2.4)

├── Data table or long document
│     └── → ArtifactDatatable or ArtifactDocument → triggers Split 30/70
│           └── Artifact dominates — chat provides framing only

├── Multiple artifacts (analysis, dashboard)
│     └── 3+ artifacts → triggers Split 30/70
│           └── Follow composition rules (see 4.3 §2 and 4.4)

└── Full dashboard on user request
      └── → Triggers Split 30/70 or user will expand to Artifact-Max
            └── Follow DASHBOARD_GENERATION_PROTOCOL (4.7 §7)
```

### 8.4 Anti-Patterns

| Anti-pattern | Why it is wrong | Correct behavior |
| --- | --- | --- |
| Returning a datatable in Chat-Only (no artifact wrapper) | Dense data in plain text is unreadable and unscannable | Always use `ArtifactDatatable` — triggers correct layout |
| Resetting layout to Chat-Only on every new message | Disorienting — user loses their visual context | Persist layout state within the task session |
| Forcing Split 30/70 for a single KPI | Artifact area feels empty; chat feels cramped | Single KPI → Split 50/50 |
| Triggering layout transition without animation | Jarring visual jump breaks spatial orientation | Always animate with `--duration-moderate` |
| Showing artifact panel with no artifact loaded | Empty artifact panel signals a broken state | Never open the panel without content |