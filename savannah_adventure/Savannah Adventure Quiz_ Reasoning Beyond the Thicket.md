## Savannah Adventure Quiz — Answers

### 1. Kale in Kakamega: MAP Framework Failure Analysis

**Where it broke down:**
- **Memory failure:** The AI had no continuity — it didn't recall the user's region from earlier in the conversation, so it treated every user as a blank slate rather than someone already anchored to Kakamega County.
- **Assets failure (the core problem):** The AI wasn't given any real data about what grows or sells locally. "Leafy greens" defaulted to kale because that's the dominant example in the AI's training data (a heavily Western/urban-market vegetable), not because kale is what's actually in Kakamega's markets.
- **Prompt/Actions failure:** No tool was invoked to check local food availability. A live lookup — even a simple crop-calendar or market-price API call — would have caught the mismatch before the message was sent.

**Redesigned prompt (MAP-anchored):**
> "You are a Community Nutrition Advisor. Memory: recall the user's stated county (Kakamega) from earlier in this conversation. Assets: use the attached local-crop data sheet for Western Kenya, which lists managu (African nightshade), terere (amaranth), and sukuma wiki (collard greens) as the dominant leafy vegetables grown and sold in Kakamega markets — not kale. Prompt/Actions: before naming a vegetable, call the crop-availability tool to confirm it's in season this month in Kakamega. Then recommend ONE locally available leafy green, in ≤160 characters, with no assumption of urban supermarket access."

This forces every recommendation through a real-data gate instead of a training-data default.

---

### 2. Panic Over Abdominal Pain: Rebuilding with the Verifier Pattern

The original prompt jumped straight to an action ("visit your doctor immediately") without ever confirming what the AI actually understood — a classic case of acting before verifying.

**Verifier Pattern rebuild:**
> "You are a maternal triage assistant. Before generating any advice, ask the user one clarifying question to narrow the symptom (e.g., 'Is the pain constant or does it come and go?'). Then ask a second question only if needed to gauge severity (e.g., 'Is there any bleeding with it?'). Restate what you understood in one plain sentence and ask 'Did I get that right?' Only after the user confirms should you classify severity and give exactly one action. If the pain is mild/intermittent with no danger signs, do not default to 'go to the clinic' — instead give a monitoring instruction plus a clear trigger for when to seek help (e.g., 'if pain becomes constant or you see bleeding, contact your CHW today')."

**Why this matters here specifically:** the panic wasn't caused by bad medical logic — it was caused by the AI treating "immediately go to a clinic" as the only possible instruction regardless of severity, ignoring that transport cost is itself a barrier. Clarifying questions let the AI distinguish "mild, watch-and-wait" from "danger sign, act now" *before* committing to an action that many users literally cannot afford to follow.

---

### 3. Why Chain-of-Thought Localization Beats Verbatim WHO Summarization

Summarizing WHO guidelines verbatim gives you *accuracy at the global level* but *zero relevance at the point of use*. WHO guidelines are written for health systems that assume things rural East Africa often doesn't have: refrigeration, daily clinic access, formula availability, diverse diets, stable mobile connectivity for follow-up. A verbatim summary either overwhelms an SMS user with clinical language or gives advice that's technically correct but practically impossible to act on.

Chain-of-Thought adaptation adds value because it makes the *reasoning steps* — not just the conclusion — visible and adjustable for local constraints:
1. State the WHO guideline.
2. Reason about which local constraint might block it (cost, distance, food availability, literacy).
3. Adapt the recommendation to something achievable within that constraint, without contradicting the underlying medical intent.

This is the difference between "exclusively breastfeed for 6 months" (WHO verbatim — true but assumes no return-to-work pressure) and a CoT-adapted version that reasons through what support a mother without maternity leave actually needs, then gives an achievable version of the same medical goal. The value isn't a better summary — it's translating global evidence into something a specific person can actually do, which is the entire premise of hyper-localization.

---

### 4. Catching the Stale Doctor-Ratio Statistic with OCEAN

Applying OCEAN to "Kenya has 1.8 doctors per 10,000 people" before it reaches donors:

- **Originality:** A red flag on its own — 1.8/10,000 is a widely recycled figure (it traces back to a 2014 WHO estimate) that AI models tend to reproduce because it's common in training data, not because it's current. A recycled number should trigger a freshness check.
- **Concrete evidence:** Ask for the source and date. When I checked current sources, figures vary wildly depending on methodology and year — a 2022 World Bank estimate implies roughly 1/10,000, Statista cites 19 registered medical officers per 100,000 (≈1.9/10,000) for 2022, and a 2017/18 fact-check calculated one doctor per 6,355 people (≈1.6/10,000) versus a disputed government claim of 1:16,000. None of these cleanly confirms either "1.8" or "0.9" — which itself is the finding: this statistic is contested and stale across the board, not a single settled number.
- **Evidentiary chain:** Does the report show *how* the number was derived (registered doctors ÷ population, which year's population estimate)? If that chain isn't visible, the number can't be verified — only repeated.
- **Assertive stance:** Flag any report presenting one figure with false confidence. Given how much these numbers vary by source and year, a defensible donor report should state a range with citation and year, not a single bare number.
- **Narrative coherence:** Cross-check against other figures in the same report — e.g., does a 1.8/10,000 doctor density line up with claimed CHW ratios and facility-access numbers elsewhere in the document, or does it contradict them?

The catch here isn't "the AI got the exact number wrong" — it's that **OCEAN forces you to ask for the source and year before publishing any statistic**, which would have surfaced that this specific figure is genuinely disputed territory rather than a stable fact, well before it reached a donor.

---
