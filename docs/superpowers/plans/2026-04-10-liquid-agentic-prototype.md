# Liquid Agentic Prototype Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a functional Next.js prototype demonstrating the "Real Estate Asset Liquidity Analysis" mission with double sidebar, Kanban pipeline, and adaptive AIWorkspace.

**Architecture:** The app shell is a three-column layout (WorkspaceSwitcher → ContextualSidebar → MainStage). MainStage switches between KanbanPipeline and AIWorkspace. AIWorkspace is a stateful container that manages layout proportions (100% chat / 50-50 split / 30-70 deep / orbital) based on a `flowStep` driven by scripted agent actions.

**Tech Stack:** Next.js 16, React 19, Tailwind CSS 4, ShadCN UI, Geist fonts, lucide-react. Dark mode is always on (forced via `className="dark"` on `<html>`).

---

## File Map

**Create:**
- `showcase/src/data/liquify-mock.ts` — Workspace, project, issue, and pipeline mock data
- `showcase/src/components/layout/AppLayout.tsx` — Root shell (WorkspaceSwitcher + ContextualSidebar + MainStage)
- `showcase/src/components/layout/WorkspaceSwitcher.tsx` — Discord-style vertical icon bar
- `showcase/src/components/layout/ContextualSidebar.tsx` — Nav: Inbox, Channels, Projects tree
- `showcase/src/components/pipeline/KanbanPipeline.tsx` — Kanban board with stage columns
- `showcase/src/components/pipeline/IssueCard.tsx` — Issue card (LIQ-n format)
- `showcase/src/components/workspace/AIWorkspace.tsx` — Adaptive layout controller
- `showcase/src/components/workspace/ApprovalCard.tsx` — Approval request panel
- `showcase/src/components/workspace/OrbitalThread.tsx` — Simulated WhatsApp transcript

**Modify:**
- `showcase/src/app/layout.tsx` — Add `className="dark"` to `<html>` element
- `showcase/src/app/page.tsx` — Replace `<Chat />` with `<AppLayout />`

**Reuse (no changes):**
- `showcase/src/components/chat/` — Chat, ChatInput, MessageList, Suggestions, TypingIndicator
- `showcase/src/components/artifacts/` — ArtifactCard, ArtifactChart, ArtifactDatatable, ArtifactKPI, ArtifactList
- `showcase/src/data/scenarios.ts` — Existing chat scenarios

---

## Task 1: Mock Data

**Files:**
- Create: `showcase/src/data/liquify-mock.ts`

- [ ] **Step 1: Create mock data file**

```typescript
// showcase/src/data/liquify-mock.ts

export interface Workspace {
  id: string
  name: string
  initials: string
  active: boolean
}

export interface Issue {
  id: string
  code: string // "LIQ-104"
  title: string
  status: "backlog" | "in-progress" | "review" | "closed"
  assignee: string
  value: string
  priority: "high" | "medium" | "low"
}

export interface PipelineStage {
  id: string
  label: string
  issues: Issue[]
}

export const workspaces: Workspace[] = [
  { id: "re", name: "Real Estate", initials: "RE", active: true },
  { id: "fi", name: "Finance", initials: "FI", active: false },
  { id: "lq", name: "Liquidity Ops", initials: "LQ", active: false },
]

export const pipelineStages: PipelineStage[] = [
  {
    id: "backlog",
    label: "Backlog",
    issues: [
      { id: "101", code: "LIQ-101", title: "Liquidation of Commercial Property D", status: "backlog", assignee: "ML", value: "R$ 1.2M", priority: "low" },
      { id: "102", code: "LIQ-102", title: "Portfolio Asset Review — Block B", status: "backlog", assignee: "VT", value: "R$ 890K", priority: "medium" },
    ],
  },
  {
    id: "in-progress",
    label: "In Progress",
    issues: [
      { id: "103", code: "LIQ-103", title: "Liquidation of Residential Unit C", status: "in-progress", assignee: "ML", value: "R$ 650K", priority: "medium" },
      { id: "104", code: "LIQ-104", title: "Liquidation of Property A — São Paulo", status: "in-progress", assignee: "MO", value: "R$ 450K", priority: "high" },
    ],
  },
  {
    id: "review",
    label: "In Review",
    issues: [
      { id: "105", code: "LIQ-105", title: "Asset Valuation — Block A Units", status: "review", assignee: "VT", value: "R$ 2.1M", priority: "high" },
    ],
  },
  {
    id: "closed",
    label: "Closed",
    issues: [
      { id: "100", code: "LIQ-100", title: "Liquidation of Office Space E", status: "closed", assignee: "ML", value: "R$ 320K", priority: "low" },
    ],
  },
]

export const activeIssue: Issue = pipelineStages[1].issues[1] // LIQ-104

// Orbital thread: simulated WhatsApp negotiation transcript
export interface OrbitalMessage {
  id: string
  sender: "agent" | "buyer" | "system"
  senderName: string
  text: string
  time: string
}

export const orbitalMessages: OrbitalMessage[] = [
  { id: "o1", sender: "system",  senderName: "System",        text: "OrbitalAgent initiated outbound contact with prospective buyer.",            time: "14:02" },
  { id: "o2", sender: "agent",   senderName: "Liquid Agent",  text: "Olá, temos um ativo residencial disponível em São Paulo — R$ 450K. Interesse?", time: "14:03" },
  { id: "o3", sender: "buyer",   senderName: "Carlos M.",     text: "Sim, pode enviar mais detalhes sobre o imóvel e a condição de pagamento.",    time: "14:05" },
  { id: "o4", sender: "agent",   senderName: "Liquid Agent",  text: "Imóvel 3 quartos, 98m², Pinheiros. Financiamento em 180 meses disponível.",    time: "14:06" },
  { id: "o5", sender: "buyer",   senderName: "Carlos M.",     text: "Qual o valor da entrada mínima?",                                              time: "14:08" },
  { id: "o6", sender: "agent",   senderName: "Liquid Agent",  text: "Entrada mínima de 20% (R$ 90K). Posso agendar uma análise de crédito agora?",  time: "14:09" },
  { id: "o7", sender: "buyer",   senderName: "Carlos M.",     text: "Pode sim. Como funciona o processo?",                                          time: "14:11" },
  { id: "o8", sender: "agent",   senderName: "Liquid Agent",  text: "Envio um link seguro para você preencher os dados. Leva menos de 5 minutos.",  time: "14:12" },
  { id: "o9", sender: "system",  senderName: "System",        text: "Buyer engaged. Negotiation flagged as high-intent. Returning to workspace.",   time: "14:13" },
]
```

- [ ] **Step 2: Verify file saved correctly**

Run: `cat showcase/src/data/liquify-mock.ts | head -20`
Expected: First lines of the file output with no errors.

---

## Task 2: Force Dark Mode

**Files:**
- Modify: `showcase/src/app/layout.tsx`

- [ ] **Step 1: Read current layout.tsx**

```bash
cat showcase/src/app/layout.tsx
```

- [ ] **Step 2: Add `dark` class to `<html>`**

Find the `<html` tag and change it to:

```tsx
<html lang="en" className="dark">
```

- [ ] **Step 3: Verify**

Run: `grep -n 'className="dark"' showcase/src/app/layout.tsx`
Expected: One match on the html line.

- [ ] **Step 4: Commit**

```bash
cd showcase && git add src/app/layout.tsx && git commit -m "feat: force dark mode on html element"
```

---

## Task 3: WorkspaceSwitcher

**Files:**
- Create: `showcase/src/components/layout/WorkspaceSwitcher.tsx`

- [ ] **Step 1: Create component**

```tsx
// showcase/src/components/layout/WorkspaceSwitcher.tsx
"use client"

import { workspaces } from "@/data/liquify-mock"
import { Building2 } from "lucide-react"
import { cn } from "@/lib/utils"

interface Props {
  activeId: string
  onSelect: (id: string) => void
}

export function WorkspaceSwitcher({ activeId, onSelect }: Props) {
  return (
    <div className="w-[52px] flex flex-col items-center py-3 gap-2 bg-stone-950 border-r border-stone-800 h-full shrink-0">
      {/* App icon */}
      <div className="w-8 h-8 rounded-lg bg-stone-700 flex items-center justify-center mb-2">
        <Building2 size={16} className="text-stone-300" />
      </div>

      <div className="w-full px-2 border-b border-stone-800 pb-2" />

      {/* Workspace icons */}
      {workspaces.map((ws) => (
        <button
          key={ws.id}
          onClick={() => onSelect(ws.id)}
          title={ws.name}
          className={cn(
            "w-8 h-8 rounded-lg text-[11px] font-semibold flex items-center justify-center transition-colors",
            ws.id === activeId
              ? "bg-blue-600 text-white"
              : "bg-stone-800 text-stone-400 hover:bg-stone-700 hover:text-stone-200"
          )}
        >
          {ws.initials}
        </button>
      ))}
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
git add src/components/layout/WorkspaceSwitcher.tsx && git commit -m "feat: add WorkspaceSwitcher component"
```

---

## Task 4: ContextualSidebar

**Files:**
- Create: `showcase/src/components/layout/ContextualSidebar.tsx`

- [ ] **Step 1: Create component**

```tsx
// showcase/src/components/layout/ContextualSidebar.tsx
"use client"

import { Inbox, Layers, FolderKanban, ChevronDown } from "lucide-react"
import { cn } from "@/lib/utils"

type ActiveView = "kanban" | "inbox"

interface Props {
  activeView: ActiveView
  onNavigate: (view: ActiveView) => void
}

export function ContextualSidebar({ activeView, onNavigate }: Props) {
  return (
    <div className="w-[220px] flex flex-col bg-stone-900 border-r border-stone-800 h-full shrink-0">
      {/* Workspace header */}
      <div className="flex items-center justify-between px-4 py-3 border-b border-stone-800">
        <span className="text-sm font-semibold text-stone-100">Real Estate</span>
        <ChevronDown size={14} className="text-stone-500" />
      </div>

      {/* Nav items */}
      <nav className="flex flex-col gap-0.5 px-2 pt-3">
        <NavItem
          icon={<Inbox size={14} />}
          label="Inbox"
          active={activeView === "inbox"}
          onClick={() => onNavigate("inbox")}
          badge="3"
        />
        <NavItem
          icon={<FolderKanban size={14} />}
          label="Liquidation Pipeline"
          active={activeView === "kanban"}
          onClick={() => onNavigate("kanban")}
        />
      </nav>

      {/* Projects section */}
      <div className="mt-4 px-4">
        <p className="text-[10px] uppercase tracking-wider text-stone-500 mb-2 font-medium">
          Projects
        </p>
        <div className="flex flex-col gap-0.5">
          <ProjectItem label="Alpha Portfolio" active />
          <ProjectItem label="Beta Assets" />
        </div>
      </div>

      {/* Bottom: user */}
      <div className="mt-auto px-4 py-3 border-t border-stone-800 flex items-center gap-2">
        <div className="w-6 h-6 rounded-full bg-blue-600 text-white text-[10px] flex items-center justify-center font-semibold">
          MO
        </div>
        <span className="text-xs text-stone-400">Murilo Oliveira</span>
      </div>
    </div>
  )
}

function NavItem({
  icon, label, active, onClick, badge,
}: {
  icon: React.ReactNode
  label: string
  active: boolean
  onClick: () => void
  badge?: string
}) {
  return (
    <button
      onClick={onClick}
      className={cn(
        "flex items-center gap-2 px-2 py-1.5 rounded-md text-sm w-full text-left transition-colors",
        active
          ? "bg-stone-800 text-stone-100"
          : "text-stone-400 hover:bg-stone-800/50 hover:text-stone-200"
      )}
    >
      <span className="text-stone-500">{icon}</span>
      <span className="flex-1">{label}</span>
      {badge && (
        <span className="text-[10px] bg-blue-600 text-white px-1.5 py-0.5 rounded-full font-medium">
          {badge}
        </span>
      )}
    </button>
  )
}

function ProjectItem({ label, active }: { label: string; active?: boolean }) {
  return (
    <button
      className={cn(
        "flex items-center gap-2 px-2 py-1 rounded-md text-xs w-full text-left",
        active ? "text-stone-200" : "text-stone-500 hover:text-stone-300"
      )}
    >
      <Layers size={12} className="text-stone-600" />
      {label}
    </button>
  )
}
```

- [ ] **Step 2: Commit**

```bash
git add src/components/layout/ContextualSidebar.tsx && git commit -m "feat: add ContextualSidebar component"
```

---

## Task 5: IssueCard + KanbanPipeline

**Files:**
- Create: `showcase/src/components/pipeline/IssueCard.tsx`
- Create: `showcase/src/components/pipeline/KanbanPipeline.tsx`

- [ ] **Step 1: Create IssueCard**

```tsx
// showcase/src/components/pipeline/IssueCard.tsx
"use client"

import { cn } from "@/lib/utils"
import type { Issue } from "@/data/liquify-mock"

interface Props {
  issue: Issue
  onOpen: (issue: Issue) => void
}

const priorityColor: Record<Issue["priority"], string> = {
  high:   "bg-red-500",
  medium: "bg-yellow-500",
  low:    "bg-stone-500",
}

export function IssueCard({ issue, onOpen }: Props) {
  return (
    <button
      onClick={() => onOpen(issue)}
      className="w-full text-left bg-stone-800 hover:bg-stone-750 border border-stone-700 hover:border-stone-600 rounded-lg p-3 flex flex-col gap-2 transition-colors group"
    >
      <div className="flex items-start justify-between gap-2">
        <span className="text-[11px] text-stone-500 font-mono">{issue.code}</span>
        <span className={cn("w-1.5 h-1.5 rounded-full mt-1 shrink-0", priorityColor[issue.priority])} />
      </div>
      <p className="text-xs text-stone-200 leading-snug group-hover:text-white transition-colors">
        {issue.title}
      </p>
      <div className="flex items-center justify-between mt-1">
        <span className="text-[11px] text-stone-400 font-medium">{issue.value}</span>
        <span className="text-[10px] bg-stone-700 text-stone-300 px-1.5 py-0.5 rounded-full">
          {issue.assignee}
        </span>
      </div>
    </button>
  )
}
```

- [ ] **Step 2: Create KanbanPipeline**

```tsx
// showcase/src/components/pipeline/KanbanPipeline.tsx
"use client"

import { pipelineStages } from "@/data/liquify-mock"
import { IssueCard } from "./IssueCard"
import type { Issue } from "@/data/liquify-mock"

interface Props {
  onOpenIssue: (issue: Issue) => void
}

export function KanbanPipeline({ onOpenIssue }: Props) {
  return (
    <div className="flex flex-col h-full bg-stone-950">
      {/* Pipeline header */}
      <div className="px-6 py-4 border-b border-stone-800">
        <h1 className="text-base font-semibold text-stone-100">Liquidation Pipeline</h1>
        <p className="text-xs text-stone-500 mt-0.5">Alpha Portfolio — Real Estate Workspace</p>
      </div>

      {/* Columns */}
      <div className="flex flex-1 gap-px overflow-x-auto p-4 bg-stone-900">
        {pipelineStages.map((stage) => (
          <div
            key={stage.id}
            className="flex flex-col min-w-[240px] max-w-[280px] flex-1 bg-stone-900 rounded-xl overflow-hidden"
          >
            {/* Column header */}
            <div className="flex items-center justify-between px-3 py-2.5">
              <span className="text-xs font-medium text-stone-400 uppercase tracking-wide">
                {stage.label}
              </span>
              <span className="text-[10px] text-stone-600 bg-stone-800 px-1.5 py-0.5 rounded-full">
                {stage.issues.length}
              </span>
            </div>

            {/* Issues */}
            <div className="flex flex-col gap-2 px-2 pb-3 overflow-y-auto">
              {stage.issues.map((issue) => (
                <IssueCard key={issue.id} issue={issue} onOpen={onOpenIssue} />
              ))}
            </div>
          </div>
        ))}
      </div>
    </div>
  )
}
```

- [ ] **Step 3: Commit**

```bash
git add src/components/pipeline/ && git commit -m "feat: add KanbanPipeline and IssueCard components"
```

---

## Task 6: ApprovalCard

**Files:**
- Create: `showcase/src/components/workspace/ApprovalCard.tsx`

- [ ] **Step 1: Create component**

```tsx
// showcase/src/components/workspace/ApprovalCard.tsx
"use client"

import { ShieldCheck, Database } from "lucide-react"

interface Props {
  onApprove: () => void
  onDeny: () => void
}

export function ApprovalCard({ onApprove, onDeny }: Props) {
  return (
    <div className="rounded-2xl ring-1 ring-white/10 bg-stone-800 p-5 flex flex-col gap-4">
      {/* Header */}
      <div className="flex items-start gap-3">
        <div className="w-9 h-9 rounded-lg bg-blue-900/60 flex items-center justify-center shrink-0">
          <Database size={16} className="text-blue-400" />
        </div>
        <div>
          <p className="text-sm font-semibold text-stone-100">Market Data Ingestion</p>
          <p className="text-xs text-stone-400 mt-0.5 leading-snug">
            The agent is requesting access to ingest real-time market data for LIQ-104 analysis.
          </p>
        </div>
      </div>

      {/* Data sources */}
      <div className="flex flex-col gap-1.5 bg-stone-900/60 rounded-lg p-3">
        <p className="text-[10px] uppercase tracking-wider text-stone-500 mb-1 font-medium">
          Requested sources
        </p>
        {["CRECI — Property Registry", "Selic Rate API — Bacen", "Neighborhood Valuation Index"].map((src) => (
          <div key={src} className="flex items-center gap-2">
            <ShieldCheck size={12} className="text-green-400 shrink-0" />
            <span className="text-xs text-stone-300">{src}</span>
          </div>
        ))}
      </div>

      {/* Actions */}
      <div className="flex gap-2">
        <button
          onClick={onApprove}
          className="flex-1 h-9 rounded-full bg-stone-200 text-stone-900 text-sm font-medium hover:bg-white transition-colors"
        >
          Approve
        </button>
        <button
          onClick={onDeny}
          className="flex-1 h-9 rounded-full bg-stone-700 text-stone-300 text-sm font-medium hover:bg-stone-600 transition-colors"
        >
          Deny
        </button>
      </div>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
git add src/components/workspace/ApprovalCard.tsx && git commit -m "feat: add ApprovalCard component"
```

---

## Task 7: OrbitalThread

**Files:**
- Create: `showcase/src/components/workspace/OrbitalThread.tsx`

- [ ] **Step 1: Create component**

```tsx
// showcase/src/components/workspace/OrbitalThread.tsx
"use client"

import { orbitalMessages, type OrbitalMessage } from "@/data/liquify-mock"
import { cn } from "@/lib/utils"
import { MessageCircle } from "lucide-react"

interface Props {
  onClose: () => void
}

export function OrbitalThread({ onClose }: Props) {
  return (
    <div className="flex flex-col h-full bg-stone-900 rounded-2xl overflow-hidden ring-1 ring-white/10">
      {/* Header */}
      <div className="flex items-center justify-between px-4 py-3 bg-stone-800 border-b border-stone-700">
        <div className="flex items-center gap-2">
          <MessageCircle size={14} className="text-green-400" />
          <span className="text-sm font-medium text-stone-100">OrbitalAgent — WhatsApp Negotiation</span>
        </div>
        <div className="flex items-center gap-2">
          <span className="text-[10px] bg-green-900/60 text-green-400 px-2 py-0.5 rounded-full font-medium">
            LIVE
          </span>
          <button
            onClick={onClose}
            className="text-xs text-stone-400 hover:text-stone-200 transition-colors px-2 py-1 rounded-md hover:bg-stone-700"
          >
            Return to workspace
          </button>
        </div>
      </div>

      {/* Messages */}
      <div className="flex-1 overflow-y-auto p-4 flex flex-col gap-3">
        {orbitalMessages.map((msg) => (
          <OrbitalBubble key={msg.id} message={msg} />
        ))}
      </div>
    </div>
  )
}

function OrbitalBubble({ message }: { message: OrbitalMessage }) {
  if (message.sender === "system") {
    return (
      <div className="flex justify-center">
        <span className="text-[11px] text-stone-500 bg-stone-800 px-3 py-1 rounded-full">
          {message.text}
        </span>
      </div>
    )
  }

  const isAgent = message.sender === "agent"

  return (
    <div className={cn("flex flex-col gap-0.5 max-w-[75%]", isAgent ? "self-start" : "self-end items-end")}>
      <span className="text-[10px] text-stone-500 px-1">{message.senderName} · {message.time}</span>
      <div
        className={cn(
          "rounded-2xl px-3.5 py-2.5 text-sm leading-snug",
          isAgent
            ? "bg-stone-700 text-stone-100 rounded-tl-sm"
            : "bg-green-700 text-white rounded-tr-sm"
        )}
      >
        {message.text}
      </div>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
git add src/components/workspace/OrbitalThread.tsx && git commit -m "feat: add OrbitalThread component"
```

---

## Task 8: AIWorkspace

**Files:**
- Create: `showcase/src/components/workspace/AIWorkspace.tsx`

AIWorkspace manages the `flowStep` (0–4) and maps it to layout proportions. It orchestrates the scripted interaction for LIQ-104.

| flowStep | Layout | Right panel |
|----------|--------|-------------|
| 0 | Chat 100% | — |
| 1 | Split 50/50 | ApprovalCard (market data request) |
| 2 | Orbital (full) | OrbitalThread |
| 3 | Split 50/50 | Final liquidation proposal (ArtifactCard) |

The chat messages are scripted per step using a `useEffect` that auto-advances when the user sends a trigger message.

- [ ] **Step 1: Create AIWorkspace**

```tsx
// showcase/src/components/workspace/AIWorkspace.tsx
"use client"

import { useState, useCallback, useRef, useEffect } from "react"
import { ScrollArea } from "@/components/ui/scroll-area"
import { Bot, ArrowLeft } from "lucide-react"
import { MessageList, type Message } from "@/components/chat/MessageList"
import { ChatInput } from "@/components/chat/ChatInput"
import { ApprovalCard } from "./ApprovalCard"
import { OrbitalThread } from "./OrbitalThread"
import { ArtifactCard } from "@/components/artifacts/ArtifactCard"
import { activeIssue } from "@/data/liquify-mock"
import { cn } from "@/lib/utils"

interface Props {
  onBack: () => void
}

type FlowStep = 0 | 1 | 2 | 3

// Scripted agent messages per step
const AGENT_SCRIPTS: Record<number, string> = {
  0: `I've loaded issue **${activeIssue.code}** — ${activeIssue.title} (${activeIssue.value}).\n\nTo proceed with the liquidity analysis, I need to ingest current market data. Please approve the data sources in the panel on the right.`,
  1: `Market data approved. Initiating outbound negotiation via OrbitalAgent on WhatsApp. Monitoring live...`,
  2: `Negotiation complete. Buyer Carlos M. shows high intent.\n\nLiquidity analysis finalized. A proposal has been prepared for your approval.`,
}

const PROPOSAL_ARTIFACT = {
  title: activeIssue.title,
  subtitle: `Código: ${activeIssue.code} · Alpha Portfolio`,
  attributes: [
    { key: "Estimated Origination", value: activeIssue.value },
    { key: "Asset Type", value: "Residential — 3BR, 98m²" },
    { key: "Location", value: "Pinheiros, São Paulo" },
    { key: "Buyer Intent", value: "High (Carlos M.)" },
    { key: "Financing", value: "180 months available" },
    { key: "Liquidity Score", value: "92 / Excellent" },
  ],
  footerBadge: "🎯 Opportunity Created",
}

export function AIWorkspace({ onBack }: Props) {
  const [messages, setMessages] = useState<Message[]>([])
  const [isTyping, setIsTyping] = useState(false)
  const [flowStep, setFlowStep] = useState<FlowStep>(0)
  const initialized = useRef(false)

  // Deliver first scripted message on mount
  useEffect(() => {
    if (initialized.current) return
    initialized.current = true
    setIsTyping(true)
    setTimeout(() => {
      setIsTyping(false)
      setMessages([{
        id: "init",
        role: "agent",
        text: AGENT_SCRIPTS[0],
      }])
    }, 900)
  }, [])

  const handleSend = useCallback((input: string) => {
    const userMsg: Message = {
      id: Date.now().toString(),
      role: "user",
      text: input,
    }
    setMessages((prev) => [...prev, userMsg])
    setIsTyping(true)

    setTimeout(() => {
      setIsTyping(false)
      const nextStep = Math.min(flowStep + 1, 3) as FlowStep
      if (AGENT_SCRIPTS[nextStep]) {
        setMessages((prev) => [
          ...prev,
          { id: Date.now().toString(), role: "agent", text: AGENT_SCRIPTS[nextStep] },
        ])
      }
      setFlowStep(nextStep)
    }, 1000)
  }, [flowStep])

  const handleApprove = useCallback(() => {
    setMessages((prev) => [
      ...prev,
      { id: "approval-user", role: "user", text: "Market data approved." },
    ])
    setIsTyping(true)
    setTimeout(() => {
      setIsTyping(false)
      setMessages((prev) => [
        ...prev,
        { id: "approval-agent", role: "agent", text: AGENT_SCRIPTS[1] },
      ])
      setFlowStep(2)
    }, 800)
  }, [])

  const handleOrbitalClose = useCallback(() => {
    setIsTyping(true)
    setTimeout(() => {
      setIsTyping(false)
      setMessages((prev) => [
        ...prev,
        { id: "orbital-done", role: "agent", text: AGENT_SCRIPTS[2] },
      ])
      setFlowStep(3)
    }, 600)
  }, [])

  // Orbital mode: full width
  if (flowStep === 2) {
    return (
      <div className="flex flex-col h-full bg-stone-950 p-4">
        <OrbitalThread onClose={handleOrbitalClose} />
      </div>
    )
  }

  // Split proportions
  const chatWidth = flowStep === 3 ? "w-[30%]" : flowStep === 1 ? "w-1/2" : "w-full"
  const rightWidth = flowStep === 3 ? "w-[70%]" : flowStep === 1 ? "w-1/2" : "w-0"
  const showRight = flowStep >= 1

  return (
    <div className="flex h-full bg-stone-950">
      {/* Chat panel */}
      <div className={cn("flex flex-col h-full transition-all duration-300", chatWidth)}>
        {/* Header */}
        <div className="flex items-center gap-3 px-4 py-3 border-b border-stone-800">
          <button
            onClick={onBack}
            className="text-stone-500 hover:text-stone-300 transition-colors"
          >
            <ArrowLeft size={16} />
          </button>
          <div className="w-6 h-6 rounded-full bg-stone-800 flex items-center justify-center">
            <Bot size={14} className="text-stone-300" />
          </div>
          <div className="flex flex-col">
            <span className="text-sm font-medium text-stone-100">Liquid Agent</span>
            <span className="text-[10px] text-stone-500">{activeIssue.code} — {activeIssue.title}</span>
          </div>
        </div>

        {/* Messages */}
        <ScrollArea className="flex-1">
          <div className="px-4">
            <MessageList messages={messages} isTyping={isTyping} />
          </div>
        </ScrollArea>

        {/* Input */}
        <div className="border-t border-stone-800">
          <ChatInput onSend={handleSend} disabled={isTyping} />
        </div>
      </div>

      {/* Right panel */}
      {showRight && (
        <div className={cn("flex flex-col h-full border-l border-stone-800 overflow-y-auto transition-all duration-300", rightWidth)}>
          <div className="p-4 flex flex-col gap-4">
            {flowStep === 1 && (
              <ApprovalCard onApprove={handleApprove} onDeny={() => {}} />
            )}
            {flowStep === 3 && (
              <>
                <div className="px-1 py-2">
                  <p className="text-[10px] uppercase tracking-wider text-stone-500 font-medium mb-3">
                    Liquidation Proposal
                  </p>
                  <ArtifactCard data={PROPOSAL_ARTIFACT} />
                </div>
                <button className="w-full h-11 rounded-full bg-stone-200 text-stone-900 text-sm font-medium hover:bg-white transition-colors">
                  Approve Proposal
                </button>
                <button className="w-full h-11 rounded-full bg-stone-800 text-stone-300 text-sm font-medium hover:bg-stone-700 transition-colors">
                  Open in Dashboard
                </button>
              </>
            )}
          </div>
        </div>
      )}
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
git add src/components/workspace/AIWorkspace.tsx && git commit -m "feat: add AIWorkspace adaptive container with flow steps"
```

---

## Task 9: AppLayout

**Files:**
- Create: `showcase/src/components/layout/AppLayout.tsx`

AppLayout wires WorkspaceSwitcher + ContextualSidebar + MainStage. MainStage shows either KanbanPipeline or AIWorkspace depending on whether an issue is open.

- [ ] **Step 1: Create AppLayout**

```tsx
// showcase/src/components/layout/AppLayout.tsx
"use client"

import { useState } from "react"
import { WorkspaceSwitcher } from "./WorkspaceSwitcher"
import { ContextualSidebar } from "./ContextualSidebar"
import { KanbanPipeline } from "@/components/pipeline/KanbanPipeline"
import { AIWorkspace } from "@/components/workspace/AIWorkspace"
import type { Issue } from "@/data/liquify-mock"

export default function AppLayout() {
  const [activeWorkspace, setActiveWorkspace] = useState("re")
  const [activeView, setActiveView] = useState<"kanban" | "inbox">("kanban")
  const [openIssue, setOpenIssue] = useState<Issue | null>(null)

  const handleOpenIssue = (issue: Issue) => {
    setOpenIssue(issue)
  }

  const handleBack = () => {
    setOpenIssue(null)
  }

  return (
    <div className="flex h-dvh bg-stone-950 overflow-hidden">
      <WorkspaceSwitcher activeId={activeWorkspace} onSelect={setActiveWorkspace} />
      <ContextualSidebar activeView={activeView} onNavigate={setActiveView} />
      <main className="flex-1 overflow-hidden">
        {openIssue ? (
          <AIWorkspace onBack={handleBack} />
        ) : (
          <KanbanPipeline onOpenIssue={handleOpenIssue} />
        )}
      </main>
    </div>
  )
}
```

- [ ] **Step 2: Commit**

```bash
git add src/components/layout/AppLayout.tsx && git commit -m "feat: add AppLayout root shell"
```

---

## Task 10: Wire page.tsx + Verify

**Files:**
- Modify: `showcase/src/app/page.tsx`

- [ ] **Step 1: Update page.tsx**

Replace the entire file content with:

```tsx
// showcase/src/app/page.tsx
import AppLayout from "@/components/layout/AppLayout"

export default function Page() {
  return <AppLayout />
}
```

- [ ] **Step 2: Commit**

```bash
git add src/app/page.tsx && git commit -m "feat: wire AppLayout as root page"
```

- [ ] **Step 3: Run dev server**

```bash
cd showcase && npm run dev
```

Expected: Server starts on `http://localhost:3000` with no TypeScript errors.

- [ ] **Step 4: Verify all flow states manually**

Open `http://localhost:3000` and verify:

1. **Kanban visible** — Double sidebar shows, pipeline has 4 columns with issues.
2. **Open LIQ-104** — Click issue card → AIWorkspace opens, agent message appears.
3. **Step 1 (Split)** — After approving via ApprovalCard, layout shifts to 50/50.
4. **Step 2 (Orbital)** — OrbitalThread displays full width with WhatsApp messages.
5. **Step 3 (Proposal)** — After closing orbital, proposal card appears 30/70 layout.
6. **Dark mode** — All surfaces use stone color palette, no white glows.

- [ ] **Step 5: Fix any TypeScript errors found during dev**

Run: `cd showcase && npx tsc --noEmit`
Expected: Zero errors.

- [ ] **Step 6: Final commit**

```bash
git add -A && git commit -m "feat: liquid agentic prototype — complete interaction flow"
```
