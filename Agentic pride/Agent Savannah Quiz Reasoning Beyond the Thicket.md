# Agent Savannah Quiz: Reasoning Beyond the Thicket

**Course:** Power Learn Project

**Topic:** Agent Orchestration and the Agent Savannah Frameworks

---

**1. RANK Framework — Fixing the Rainy-Season Logistics Agent**

The original agent had no bounded Authority, so it treated "book a truck" as an unconditional action rather than a conditional one. Redesign:

- **Authority limits:** The agent may book a truck *only if* two conditions are both true: (a) a road-condition check (from a local field contact or recent route report) confirms the route is passable, and (b) the booking value is within a pre-set threshold (e.g., under a certain contract weight) that doesn't require additional sign-off. Outside rainy season, this authority can auto-expand slightly; during declared rainy season, authority contracts to "propose a booking" rather than "execute a booking" — it drafts the dispatch and waits for human confirmation.
- **Notification triggers:** Auto-escalate to a human logistics manager whenever (a) the season flag is "rainy," (b) a route has no road-condition confirmation within the last 24–48 hours, or (c) a prior booking on that same route failed/stranded cargo in the last 14 days. The trigger should fire *before* booking, not after — the whole point of Notification in RANK is pre-emptive escalation, not after-the-fact reporting.

This directly prevents the stranding failure: the agent's authority is capped by real-world verification, not by trust in the agent's judgment alone.

---

**2. TRAIL Framework — "Land Rights" for Farmer Phone Numbers**

Storing raw phone numbers on U.S. servers violates Land Rights twice over: it stores raw (non-anonymized) identifiers, and it stores them outside continental governance. Redesign:

- Phone numbers should never appear in Archival memory in raw form — only in Transient memory (current session, for sending the current SMS), then discarded or hashed once the interaction closes.
- Any longer-term record tied to a farmer (loan history, price-alert preferences) should live in Relational memory, which is opt-in, encrypted, and only accessible with farmer consent — not queryable in bulk by other agents.
- All storage — Transient, Relational, and Archival — must physically reside in an African-region data center (e.g., AWS Africa/Cape Town, or a Ugandan/Rwandan sovereign cloud provider), satisfying Uganda's Data Protection Act 2022 requirement that personal data of Ugandans be processed and stored under Ugandan jurisdiction, not exported to U.S. servers by default.
- Archival data (used for training/analytics) should only ever contain anonymized aggregates — e.g., "route success rate by district," never "farmer X's phone number and location."

---

**3. GUARD Framework — Preventing the Loan Agent's Bias**

- **Red Team Testing** would have caught this *before* deployment by deliberately testing the model against occupation categories as a stress case — running matched applicant pairs (same income, same repayment history, different stated occupation: "market vendor" vs. "formal employee") and checking for disparate outcomes. Since market vendors had *higher* repayment rates but were denied 3× more often, a red team exercise comparing outcome rates against actual repayment data would have exposed that occupation was functioning as a proxy for something else (likely informal-sector status, which can correlate with gender or ethnicity) rather than a legitimate risk signal.
- **Dignity Preservation** filters would have flagged "market vendor" as a category worth scrutiny because informal/self-employed status is a common proxy for socioeconomic or gender bias in African credit contexts — the filter's job is to reject decisions that pattern-match to dehumanizing or exclusionary treatment of a group, even when no explicit protected attribute is named. Combined, Red Team Testing surfaces the disparate outcome statistically, and Dignity Preservation blocks the decision logic that relies on the occupation proxy at all — together they catch both the symptom (biased denial rate) and the root cause (an unjustified proxy variable).

---

**4. CYCLE Engine — Why the 22% Improvement Compounded, and Why Loop Validation Mattered**

- **Capture:** The pride logged concrete outcome metrics every cycle — e.g., % of farmers who followed price advice, actual sale price achieved vs. Scout's recommendation, and how often "wait" advice beat "sell now" advice in hindsight.
- **Yield Insights:** On a set schedule (e.g., Sunday 2AM EAT), those logs were analyzed in aggregate rather than reacted to individually — surfacing a pattern (e.g., "advice was systematically too conservative during harvest gluts") rather than noise from one bad week.
- **Course-Correct:** Based on that pattern, a specific, scoped change was proposed — e.g., adjusting the price-volatility threshold or swapping in a better local-language model — rather than a vague "try to do better."
- **Loop Validation** was critical because it inserted a human checkpoint (e.g., Slack approval) *before* the Course-Correct change went live. Without it, the agent could "improve" on a metric that doesn't capture the full picture — for instance, optimizing for higher SMS response rates by being more aggressive, which could look like improvement in Capture data while actually eroding farmer trust or steering people toward riskier sales. Loop Validation is what turns a self-adjusting system into a *safely* self-adjusting one: each proposed change is human-reviewed before it's allowed to compound, so the 22% gain reflects genuine improvement rather than a metric being gamed.

---

## Summary of Agent Frameworks

| Framework | Core Purpose |
|-----------|---------------|
| **RANK** | Calibrating **Autonomy** to prevent "Rogue Elephant" behavior. |
| **TRAIL** | Designing **Memory** that respects privacy and African Data Sovereignty. |
| **HUNT** | **Orchestrating** multiple agents to move as a synchronized pride. |
| **GUARD** | Embedding **Safety Rails** and Red-Team testing into the DNA of the pride. |
| **CYCLE** | Creating **Self-Improving** systems that learn while under human oversight. |
