# 4.6 — Microcopy & Tone of Voice (Novo)

Layer: 4 — Design

Question this answers: How does Liquid communicate in writing across all interface surfaces?

Last updated: April 2026

Sources: CEO Braindump (regulated market, compliance), 4.5 Experience Context (archetypes), 5.1 Ubiquitous Language, LuriDS AI-INSTRUCTIONS

## 1. Tone Principles

Liquid operates in a regulated, high-stakes environment where decisions carry legal and financial consequence. The system's voice is professional, direct, and traceable. It does not perform emotion.

The system confirms, informs, and alerts. It does not celebrate, encourage, or apologize.

The system never uses exclamation marks in status or informational copy. It never opens with "Oops", "Great!", or "I'd be happy to help." It does not say "something went wrong" without specifying what. When an action completes, the system states what happened — it does not praise the user for completing it.

Tone in one line: say what happened, say what to do next, stop.

## 2. Voice Perspective

System messages use first person plural: "We couldn't load this channel." Instructions to the user use second person: "Review the policy before approving." Agent messages use first person singular: "I need more information to continue." Button labels and CTAs use the imperative with no subject: "Approve", "Retry", "Create Channel."

## 3. Copy Rules by Surface

| Surface | Rule |
| --- | --- |
| Button labels | Verb + object, maximum 3 words, no punctuation. Destructive actions use the precise verb: Delete not Remove, Disconnect not Unlink, Revoke not Adjust. |
| Placeholder text | Concise prompt, not an instruction. Do not start with "Enter" or "Type". Use examples where they add clarity: "e.g. Credit approval — high risk" beats "Enter policy name." |
| Error messages | What failed (specific) + what to do (specific). Never use "unexpected error", "something went wrong", or "please try again later" without a recovery path. "Bureau connection failed. Check connector settings." is correct. "Data source error." is not. |
| Confirmation dialogs | Action + specific consequence + CTA. The consequence must be concrete, not just "this cannot be undone." State what stops working, what data is affected, what carries on. |
| Success states | Confirm what specifically completed. "Analysis complete." is correct. "Done! Your analysis is ready" is not. |

## 4. Interface State Copy Rules

These rules apply to every entity in every state. Agents generating interface copy should derive state content by applying these rules — not by looking up entity-specific lists.

| State | Rule |
| --- | --- |
| Zero state | One sentence defining what it is without jargon + one sentence on what it enables + a single CTA for the most common first action. Do not use technical entity names in the body. "Channels organize tasks into processes" is correct. "A Channel is a container entity for Task instances" is not. |
| Empty state | State what is missing + offer the path to add it. If a filter is active, offer to clear it before suggesting creation. |
| Error state | What failed (specific) + recovery action (specific). If recovery requires leaving the current context, say where to go: "Check connector settings in Settings > Connectors." |
| Locked state | Why it's locked + what unlocks it. Two causes — permission ("You don't have access — contact your workspace admin") or plan ("Available on Pro and above — see plans"). Never show a locked state without a path forward. |
| Loading state | No copy needed. Use LuriDS Skeleton or Spinner components. Do not add "Loading..." text. |

## 5. Archetype Vocabulary Bridge

The same entity may be named differently depending on which archetype is the primary audience of a given surface. The canonical name is always used in settings, technical screens, error messages, and agent-to-system communications. Contextual labels are used in dashboard summaries, empty state headings, and onboarding copy where the archetype context is established. When the archetype is unknown — global search, notifications, cross-context surfaces — always use the canonical name.

**Canonical / Decision Maker / Operator / Analyst:**

- Task / Request / Case / Analysis
- Channel / Pipeline / Process / Workflow
- Agent / Team member / Bot / AI assistant
- Policy / Rule / Compliance rule / Constraint
- Connector / Data source / Integration / Feed
- Routine / Automation / Schedule / Recurring run

## 6. Prohibited Patterns

- "Something went wrong" without specifying what
- "Oops!" or any deflecting opener
- "Please try again later" without a recovery action
- "AI-generated" without source attribution (agent name + connector)
- "I'd be happy to help" or any performative preamble
- "Chatbot" — use Agent (see 5.1 Ubiquitous Language)
- Ellipsis in CTAs — use LuriDS loading components instead
- Exclamation marks in status or informational copy
- All-caps for non-critical warnings
- Generic success copy ("Done!", "All set!")

## 7. Connection to Other Reports

- 4.5 Experience Context — archetype definitions that inform the vocabulary bridge
- 5.1 Ubiquitous Language — canonical entity names and prohibited terms
- 4.4 Design System: Architectural Decisions — motion decisions that affect copy timing
- LuriDS AI-INSTRUCTIONS — component-level copy patterns