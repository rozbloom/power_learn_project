# AgriPride — Autonomous Agent Pride for Smallholder Coffee & Maize Farmers (Uganda / Rwanda)

## Introduction

AgriPride's original all-in-one agent failed because it tried to act as pricing analyst, dispatcher, and advisor at the same time. This led to contradictory advice and truck bookings with no verification.

A better approach is to orchestrate a specialized agent pride:
- **Scout** for market intelligence
- **Guardian** for logistics coordination
- **Hunter** for human-in-loop liaison

This structure replaces guesswork with calibrated autonomy, embedded safety, and human sovereignty at every critical decision.

---

## Agent 1: Scout Agent — Market Intelligence · RANK Framework

### Role
- Price advisor only.
- Recommends sell/hold timing using Rwanda Agricultural Board API and Uganda commodity exchange feeds.
- Never executes a sale or contract itself.

### Authority
- Maximum **3 SMS/day per farmer**.
- May never override the farmer's final decision.
- Advice is always framed as a recommendation, not an instruction.

### Notification
- Auto-escalates to **Guardian Agent** and flags a human market analyst if price volatility exceeds **20% within 24 hours**.

### Kill Switch
- **USSD `*#700#`** immediately pauses Scout.
- Returns the farmer to manual, unassisted decision-making.
- Works without a smartphone.

### RANK Elementsa
- **GUARD**: Guardrail — block any price recommendation that uses ethnic, tribal, or regional proxies as inputs. (Dignity Preservation filter)
- **CYCLE**: Capture — log SMS delivery/response rate and the percentage of farmers who followed vs. ignored each price recommendation.
- **TOOL**: `n8n` — low-code automation linking commodity-price APIs to the SMS/USSD gateway across shaky rural connectivity.

---

## Agent 2: Guardian Agent — Logistics Coordinator · TRAIL Framework

### Data Handling
- **Transient**: Holds only the active booking's truck ID, route, and ETA; discarded once delivery closes.
- **Relational**: Farm GPS location is opt-in, shown to the farmer at signup, and revocable at any time without losing the SMS price service.
- **Archival**: Stores anonymized, district-level route success and delay rates. No farmer names, phone numbers, or plot IDs are retained.
- **Inheritance**: Passes verified road-condition and truck-availability data to Hunter only after a contract is confirmed.

### Land Rights
- All records are held in **AWS Africa (Cape Town)**.
- Satisfies Uganda's **Data Protection Act 2022** localization requirement.

### RANK Elements
- **GUARD**: Guardrail — a truck may never be dispatched until a local CHW-style field contact confirms current road conditions; this closes the prototype's unverified-booking failure.
- **CYCLE**: Yield Insights — Sunday 2AM EAT review of delayed-delivery percentage, split by road condition vs. truck-availability cause.
- **TOOL**: `CrewAI` — role-based coordination so Guardian and Hunter negotiate truck confirmation without conflicting instructions.

---

## Agent 3: Hunter Agent — Human-in-Loop Liaison · HUNT Protocol

### Workflow
- **Handoff Trigger**: When Scout secures a buyer contract over **500kg**, Hunter auto-alerts Guardian with destination, volume, and deadline.
- **Unified Context**: A single shared real-time dashboard is visible to Scout, Guardian, Hunter, and the human logistics manager.
- No agent works from a stale or private copy of the data.

### Rules
- **Negotiation Rules**: If freight cost exceeds **30% of the contract price**, Hunter halts and escalates to the human logistics manager before any booking proceeds.
- **Termination**: The workflow closes only once the human logistics manager signs off on the delivery confirmation, not on truck departure alone.

### RANK Elements
- **GUARD**: Audit Trail — an immutable log of every handoff, including the counterfactual of what would have happened absent the human check-in.
- **CYCLE**: Explain — a weekly plain-language report to the logistics manager and farmers summarizing what changed in the workflow and why.
- **TOOL**: `LangGraph` — visual mapping of the Scout → Guardian → Hunter handoff so managers can audit the workflow at a glance.

---

## Reflection: Preserving Human Sovereignty

Full autonomy never means unchecked autonomy.

- Each agent's **RANK boundaries** stop it at the edge of its role.
- Every farmer keeps a **USSD kill switch** independent of literacy or smartphone access.
- Guardian cannot dispatch a truck without human-sourced road verification.
- Hunter cannot close a workflow without the logistics manager's sign-off.

Autonomy speeds up the pride's work, but final authority on money, transport, and land rights stays with people.

Opt-in memory and immutable audit trails make every decision reversible and visible.
