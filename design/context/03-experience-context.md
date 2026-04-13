# 4.5 — Experience Context (Novo)

Layer: 4 — Design

> Question this answers: Who uses Liquid, in what operational mode, and what does that require from the interface?
> 

> Last updated: April 2026
> 

> Sources: CEO Braindump (global by design, generalist at entry), Paperclip — The New Dominant UI Design for AI Agents (Emerge Haus), NN/G — The UX of AI: Lessons from Perplexity, EY Studio — How Agentic AI Enables a New Approach to UX, HackerNoon — Agentic UX Over Chat
> 

## 1. Design Philosophy: Archetypes, Not Personas

Liquid is designed to be market-agnostic at entry. The CEO Braindump is explicit: "the platform must be generalist enough to embrace transactions without bringing business rules into the application layer." This extends to UX — the interface cannot be designed for a specific vertical's personas without becoming a liability when the platform enters a different market.

Archetype-based design optimizes for operational modes — what the user is trying to do at this moment, regardless of who they are or what market they operate in. Research on leading agentic platforms confirms this shift. Perplexity designs for intent ("what do you want?"), not identity. Paperclip frames the UX challenge as: "How do I give any human enough context to make a governance decision in under 30 seconds?" Manus AI builds for a universal observer — any human watching any agent work.

Vertical-specific personas (e.g., real estate CFO, credit analyst) belong in go-to-market documentation, agent configuration, and onboarding templates — not in the interface foundation. The interface is the same console regardless of what game is loaded.

## 2. Role Archetypes

Four archetypes cover the full surface of Liquid users across any vertical. Every person using the platform occupies at least one of these modes at any moment — and may shift between them within a single session. Archetypes are modes, not identities.

| Archetype | What they do | They need | They avoid |
| --- | --- | --- | --- |
| **Decision Maker** | Approves, rejects, and sets direction. | Status at a glance, a clear required action, and an audit trail visible without digging. | Cognitive load and ambiguity about what is expected. |
| **Operator** | Runs processes, manages agents, and maintains workflows. | Queue visibility, process stage indicators, and agent health. | Dead ends and loss of process context. |
| **Analyst** | Investigates data, generates reports, and builds knowledge. | Data density, drill-down access, and artifact generation. | Shallow outputs, missing sources, and forced simplification. |
| **Viewer** | Monitors outcomes and consumes reports. | Summary views and exportable content. | Noise and interfaces that demand action when they only want to observe. |

## 3. Interaction Intents

Four intents cover every meaningful interaction a user has with an agentic system. These are independent of vertical, role, or session length.

| Intent | When | User question | Interface prioritizes |
| --- | --- | --- | --- |
| **Orient** | Session start or return after absence | "What happened? Where am I?" | Status overview, recent activity, and pending actions |
| **Delegate** | The user wants something done | "Do this for me." | Fast task input, agent selection, and confirmation of delegation |
| **Monitor** | A process is running and the user checks in | "Is this going as expected?" | Progress indicators, agent activity, and status changes |
| **Act** | An agent requires a human decision | "I need to do something now." | A single clear action with full context and explicit consequence. Act is the highest-priority intent and must never share visual weight with Monitor. |

## 4. Governance Moments

Governance moments are when the interface must shift from ambient to active — where an agent's autonomous work stops and human judgment is required. These are the highest-stakes interactions on the platform.

| Governance moment | Trigger | Interface response |
| --- | --- | --- |
| **Policy checkpoint** | Agent reached a rule requiring human approval. | Inbox item + active state shift in the relevant task or channel. |
| **Risk threshold** | Agent flagged output above defined tolerance. | Alert in Inbox, visible in channel status. |
| **Ambiguous instruction** | Agent cannot proceed without clarification. | In-task input request, not a background notification. |
| **Irreversible action** | Agent about to do something that cannot be undone. | Confirmation modal with full consequence stated. |
| **Orbital agent result** | External agent returned with data requiring review before proceeding. | Inbox item + resume prompt in the originating task. |

Every governance moment must surface a single, unambiguous action. The user should never need to figure out what to do — the interface tells them, with the context required to decide.

## 5. Experience Principles

These are deterministic rules for UI generation. They apply across all archetypes, intents, and trust states.

1. **Status always visible.** Every entity shows its current state without requiring a click.
2. **Single primary action per viewport.** If two actions compete for equal weight, the information hierarchy is broken — not the styling.
3. **Traceability without friction.** Every agent output has a visible source. The user can verify without leaving the current context.
4. **No dead ends.** Every state — empty, error, locked — has a defined next step.
5. **Hierarchy before aesthetics.** Attention is earned through typography, spacing, and position — not color inflation or animation. Color communicates state, not importance.
6. **Consistency over perfection.** Identical situations produce identical outputs. "It always looks the same" is the quality standard.

## 6. Connection to Other Reports

- 4.1 Design System: Cognitive Architecture — mental models that underpin these archetypes
- 4.2 UX Functional Architecture — where archetypes and intents manifest in the screen map
- 4.3 Interaction Model — governance moments in technical terms
- 4.6 Microcopy & Tone of Voice — how the system speaks to each archetype
- 2.2 Human System Map — vertical-specific personas that map onto these archetypes for go-to-market