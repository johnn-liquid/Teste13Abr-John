# 4 2 - Design System Architectural Decisions

# 4.2 - Design System: Architectural Decisions

> Layer: 4 — Design
Question this answers: What architectural decisions anchor Liquid’s visual system, and why were they made?
Last updated: April 2026
Sources: CEO Braindump (interface vision, ShadCN rationale), LuriDS (Murilo Oliveira), Carbon Design System, HackerNoon — Agentic UX Over Chat, Bencium — The Great UX Evolution, Jitter — How Perplexity builds its brand with motion, Nielsen Norman Group — The Role of Animation in UX
> 

## **1. Component Foundation: ShadCN Vanilla**

Liquid’s UI is built on ShadCN/ui (Radix UI primitives + Tailwind CSS) without significant divergence from the vanilla component set. The decision is deliberate: significant customization shifts consistency responsibility from the agent to the designer. An agent understands ShadCN deeply by default — it does not understand custom components until each is explicitly documented. A small contextual layer on top of vanilla is preferable to a custom fork.

New components require explicit documentation before agents can use them. Undocumented components do not exist in generation context.

## **2. Dark Mode Default**

Liquid uses dark mode by default, but it is not the only option. A light mode version is available, and users can switch to it if they prefer. All generated screens should use appropriate theme tokens to support both modes.

Financial operators work in extended sessions — reduced screen luminance lowers cumulative eye strain. Risk indicators, timelines, and multi-column tables achieve higher contrast separation on dark surfaces. Semantic colors (red/yellow/green) retain full perceptual weight on dark backgrounds and wash out on white. Dark mode also places Liquid in the category of professional operator tools — Linear, Perplexity, Bloomberg Terminal — rather than consumer SaaS.

## **3. Motion as Functional Communication**

Motion in Liquid communicates system state. It does not decorate. Every transition must answer: what does the user now understand that they didn’t before this animation played?

What does not animate, and why: risk color changes are facts — red appears immediately, a transition implies uncertainty. Policy evaluation results are deterministic — delay suggests the system is still deciding. Error messages must be instant — latency signals the system is processing something it is not. Nothing animates for visual interest alone.

When in doubt, do not animate. The default is stillness.

## **4. Token Architecture: Concept**

All visual values are expressed through a three-tier semantic token system. No value is hardcoded in any component. This enables AI generation fidelity — an agent that only emits token references can never produce a visually inconsistent output.

The three tiers: Global (raw values, never referenced directly), Semantic (purpose-mapped aliases, used in all components), Component (scoped overrides for specific contexts).

Colors use OKLCH color space for perceptual uniformity — two colors at the same OKLCH lightness value appear equally light to the human eye. This matters for risk indicators, where red, yellow, and green must carry equal visual weight regardless of hue. Token reference via var(–token) is mandatory. Any hardcoded value in generated output is a violation.

## **5. Agentic Motion Patterns**

These patterns are specific to agentic systems — they emerge only when an autonomous agent is acting on behalf of the user and are absent from traditional UI motion documentation.

Research shows that users lose confidence in agentic systems not when the system makes errors, but when they cannot tell what the system is doing. Motion serves a governance function: it makes invisible agent activity visible. Trust is graduated, and motion adapts: new users need every agent action to have a visible signal; established users need only decision-relevant states; power users are comfortable with peripheral indicators.

### **Thinking State vs. Loading State**

A spinner communicates waiting. A thinking state communicates active, directed processing. These are not the same — conflating them erodes trust over time.

The pattern: a persistent, low-intensity indicator paired with a label describing the current agent action. The label changes as the agent progresses — “Consulting bureau → Applying policy → Generating report.” The visual indicator stays consistent; the label is what moves. This is the pattern Claude uses (streaming cursor + generation state) and is adapted from Perplexity’s layered, sequenced animation approach.

### **Handoff Motion**

The transition from agent control to human control requires an explicit visual signal. Research identifies agent-to-human handoffs as the highest abandonment point in agentic systems. When an agent reaches a Human-in-the-Loop checkpoint, the interface makes the transition unambiguous — the conversation shifts register, the primary action becomes the human’s, and the agent visually recedes.

Any screen containing a Human-in-the-Loop moment must have a distinct visual treatment for “waiting for human” — separate from the agent thinking state and from a generic empty state.

### **Streaming with Intent**

AI-generated content is delivered in meaningful chunks — not token-by-token, not all at once. Token-by-token creates cognitive noise; all-at-once creates a jarring jump. The right unit is semantic: paragraph, section, or complete artifact depending on content type.

LuriDS defines –duration-deliberate (700ms) for AI streaming. This duration governs the reveal of a complete semantic unit, not individual characters. Perplexity implements this as sequenced, layered animations — each layer appears after the previous has stabilized.

## **6. Connection to Other Reports**

- 4.1 Design System: Cognitive Architecture — principles of how users navigate and organize
- 4.6 Interaction Model — Human-in-the-Loop patterns that require handoff motion
- 4.4 Experience Context — trust states that determine motion intensity
- 4.8 Microcopy & Tone of Voice — copy decisions that follow the same rationale pattern
- 3.5 Strategic & Architectural Decisions (ADRs) — broader product-level decisions
- LuriDS / AI-INSTRUCTIONS — implementation rules that follow from these decisions