# 4 2 - UX Layouts and Pages

# 4.2 - UX Layouts and Pages

## 1. AIWorkflow Layout (Tasks / Chat)

- **Proportions:** Sidebar (220px fixed) | MessageList (flex, min 480px) | ContextPanel (320px collapsible).
- **Persistent Elements:** Chat Input Bar is pinned to the bottom. Never scrolls out of view.
- **Context Panel:** Consists of 3 tabs: Info, Related, Timeline (Audit). Auto-opens when an entity is selected, policy fails, or approval represents.

## 2. Layout Transitions (Chat)

Agents trigger UI layout changes based on the artifacts returned. Transitions MUST be animated (`--duration-moderate`).
* **Chat-Only (100%):** Pure conversational text. No artifacts.
* **Split 50/50:** Triggered by 1-2 Artifacts (`ArtifactKPI`, `ArtifactChart`, `ArtifactList`).
* **Split 30/70:** Triggered by dense artifacts (`ArtifactDatatable`, `ArtifactDocument`), 3+ artifacts, or full dashboards.
* **Artifact-Max:** Triggered by explicit user manual expansion. Chat becomes floating action button.

## 3. Other Core Layouts

- **Dashboard Hub:** KPI Row (top, renders in 1 viewport across breakpoints) > Primary Chart (60%) > Alerts Panel (40%).
- **Channel (Kanban):** Kanban board scrolls horizontally. Fixed column widths (280px). Timeline expands upwards from the bottom. Chat panel is fixed to the right (min 48px).
- **Detail Page:** Entity header is fixed. Primary action bar holds max 3 primary actions. “Audit” is a mandatory tab.

## 4. Progressive Disclosure

- **Onboarding:** Max 3 visible actions. Sidebar shows only: Home, New Channel, Help.
- **Active:** Unlocks full sidebar + Inbox. Triggered by having ≥1 active channel.
- **Power:** Unlocks Policies, Routines, Advanced Dashboards. Triggered by ≥1 configured Agent + ≥1 active Policy. Do not show “Upgrade to unlock” friction.