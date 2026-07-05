# Savannah Tracker Report
## AfyaTech — Precision Prompting for the SMS Maternal Health Assistant
**Prepared by:** AI Strategy Lead | **Date:** July 2026

**Frameworks used :**
- **AIM** — **A**ctor (who the AI is), **I**nput (context/data given), **M**ission (clear, measurable goal)
- **MAP** — **M**emory (conversation continuity), **A**ssets (real data grounding the AI), **P**rompt/Actions (tools doing the heavy lifting)

---

## Introduction 

In maternal health, a generic prompt isn't just unhelpful — it's dangerous. Advice that ignores diet, distance to clinics, mobile money limits, or cultural norms erodes trust and can delay care. Precision prompting turns AfyaTech's assistant from a translated urban chatbot into a tool grounded in the realities of rural life.

---

## Prompt A — Nutrition Advice

**Original:** "Give nutrition tips for pregnant women."

**Rewritten prompt:**
> "You are a Community Nutrition Advisor supporting pregnant women in rural Kenya/Uganda whose diets are built around matooke, ugali, or cassava. Using the attached nutrition-gap data sheet and the user's stated trimester and region, generate one SMS-length (≤160 character) tip that layers a specific, locally available, low-cost food onto their existing staple to close one nutrient gap (iron, folate, or protein). Before answering, check current seasonal price/availability of the recommended food using the food-price lookup tool. Never suggest imported or urban-market-only foods. Track which tip was sent last time so you don't repeat it."

**AIM annotation**
- **A (Actor):** "Community Nutrition Advisor" — a role grounded in local diets, not a generic dietitian.
- **I (Input):** Trimester, region, and a nutrition-gap data sheet (see Assets below).
- **M (Mission):** Produce exactly one nutrient-gap-closing tip per message, under 160 characters, using only locally available foods.

**MAP annotation**
- **M (Memory):** Retains which tip was already sent, so advice progresses across the pregnancy instead of repeating.
- **A (Assets):** Real grounding data — matooke supplies Ugandans with up to roughly 30% of daily caloric intake but is low in protein and iron (about 1g protein and 0.25mg iron per 100g); rural Ugandan diets typically include fewer than four food groups a day, with beans supplying most of the energy in the pulses group. This data anchors *which* nutrient gap to target instead of guessing.
- **P (Prompt/Actions):** A food-price lookup tool call checks whether the recommended food (e.g., omena/dagaa for iron) is actually in-season and affordable in that region this month, before the tip is sent.

**Key improvement over the original:** The original had zero grounding, so the model defaulted to generic, often Western nutrition advice. Loading real staple-food nutrient data as an Asset — and verifying seasonal price via a tool call — cuts hallucination risk twice over: once by fact-grounding the advice, once by confirming the suggested food is actually obtainable that week.

**Refinement Protocol — two sharper versions:**
1. *V2 (tighter output contract):* Add a required output schema: `[local food name] + [nutrient it fixes] + [approx. cost in KES/UGX]`, forcing every reply into an auditable, consistent structure.
2. *V3 (fallback branch):* If the Asset check shows the target food is out of season or above a cost ceiling, require the model to substitute the next-best local alternative and flag lower confidence, rather than silently recommending an unaffordable food.

**OCEAN verification (applied to a sample output — "Add a spoonful of dagaa to your matooke this trimester for iron. ~50 KES."):**
- **O**riginality: Tailored to matooke, not boilerplate.
- **C**oncrete evidence: Names a specific food and real cost.
- **E**videntiary chain: Traceable from matooke's low iron content → iron-rich dagaa recommendation.
- **A**ssertive stance: One clear action, not hedged.
- **N**arrative coherence: Fits the pregnancy-nutrition context without contradiction.

---

## Prompt B — Appointment Reminders

**Original:** "Remind users about doctor visits."

**Rewritten prompt:**
> "You are a Rural Antenatal Scheduling Coordinator working with community health workers (CHWs) in Kenya/Uganda. Using the attached CHW-availability and clinic-hours data, and the user's appointment record, generate an SMS reminder 48 hours in advance that includes the date, nearest clinic, estimated travel time, and the assigned CHW's contact — only if that CHW is realistically available given known ratio constraints. Check the clinic's calendar tool for hours and the local events calendar for market days or planting-season conflicts before finalizing. If a conflict exists, propose the nearest alternative slot instead of sending the original reminder unchanged. Recall whether this user has missed appointments before and note the stated reason."

**AIM annotation**
- **A (Actor):** "Rural Antenatal Scheduling Coordinator" — logistics-focused, not a generic reminder bot.
- **I (Input):** Appointment record, clinic hours, CHW roster, user's location.
- **M (Mission):** Deliver a reminder that is also a feasible travel/escort plan, with a conflict-resolution fallback — not just a date notification.

**MAP annotation**
- **M (Memory):** Recalls prior missed appointments and stated reasons, so the same avoidable failure isn't repeated.
- **A (Assets):** Real CHW capacity data — a Kenyan study found actual CHW-to-household ratios ranging from 1:37 to 1:820 against a government target of 1:20; in Uganda, the Ministry of Health target is 25–30 households per CHW, but a Wakiso District study found 61.7% of CHWs serving more than 50 households. Proximity data adds nuance too: in one Western Uganda district, 77% of residents live within 5km of a facility, yet the doctor-to-population ratio is roughly 1:18,513 — proximity doesn't guarantee availability of skilled care.
- **P (Prompt/Actions):** Calendar tool call for real clinic hours/CHW schedule, plus a local-events check for market days or planting season that predictably cause no-shows.

**Key improvement over the original:** The original only handled *recall* ("remind"), not the actual barrier to attendance. Grounding the CHW-escort promise in real ratio data prevents the system from overpromising an escort that statistically may not exist in that ward — a reliability failure the ungrounded version couldn't catch.

**Refinement Protocol — two sharper versions:**
1. *V2 (capacity-aware branching):* "If the local CHW ratio (per Assets) is worse than 1:50 households, do not promise same-day escort — instead send clinic self-navigation directions and an estimated boda boda fare."
2. *V3 (accessibility fallback):* If Memory flags the user as low-literacy, route the reminder as a voice call instead of SMS text.

**OCEAN verification (applied to a sample reminder):**
- **O**riginality: Reflects this user's specific clinic/CHW situation, not a template.
- **C**oncrete evidence: Names real travel time, transport mode, and CHW contact status.
- **E**videntiary chain: Escort promise is conditioned on actual CHW-availability data, not assumed.
- **A**ssertive stance: One clear next step (attend, or take the alternate slot).
- **N**arrative coherence: Consistent with the user's prior appointment history.

---

## Prompt C — Emergency Triage (Chain-of-Thought + Verifier Pattern)

**Original:** "Tell me what to do if I feel unwell during pregnancy."

**Rewritten prompt:**
> "You are a maternal health triage assistant operating over SMS with no live human oversight.
>
> **Step 1 (elicit):** Ask exactly one clarifying question to narrow the symptom category (e.g., 'Is there any bleeding — yes or no?'). Do not give advice yet.
>
> **Step 2 (elicit further, only if needed):** Ask at most one more targeted question to distinguish severity (e.g., duration, amount, presence of pain).
>
> **Step 3 (Chain-of-Thought, internal):** Match the answers against a fixed danger-sign list — heavy bleeding, severe abdominal pain, convulsions, high fever, no fetal movement, severe headache with vision changes. Classify as RED (danger sign present — act now), YELLOW (concerning, contact CHW within hours), or GREEN (normal discomfort).
>
> **Step 4 (Verifier Pattern — clarifying confirmation):** Before giving advice, restate your understanding in one plain sentence and ask 'Did I understand correctly?' If the user corrects you, re-run Step 3 with the corrected information.
>
> **Step 5 (act):** Once confirmed, give exactly one calm, plain-language next action matched to the classification. Never state a diagnosis."

**Key improvement over the original:** The earlier draft relied on the AI silently double-checking its own output — useful, but it can't catch a misread *input*. Rebuilding the Verifier Pattern around the module's actual definition — the AI asking the user clarifying questions and confirming its understanding before acting — closes an earlier failure point: a misclassified symptom caught before advice is generated, not after.

**Refinement Protocol — two sharper versions:**
1. *V2 (question budget):* Cap clarifying questions at two total, to prevent an anxious user from being stuck in an endless question loop during a possible emergency.
2. *V3 (escalation override):* If any single answer immediately matches a RED danger sign, skip further clarifying questions entirely and issue the emergency action immediately — safety overrides thoroughness.

**OCEAN verification (applied to a sample RED-flagged exchange):**
- **O**riginality: Response is specific to the confirmed symptom, not generic "see a doctor."
- **C**oncrete evidence: Classification is tied to a named danger sign, not vague concern.
- **E**videntiary chain: User can see exactly how their answer led to the classification, via the confirm-back step.
- **A**ssertive stance: Exactly one unambiguous action, no hedging.
- **N**arrative coherence: Tone stays calm and non-contradictory even under a RED classification.

---

## Reflection 

Rebuilding these prompts against the correct AIM/MAP definitions changed how I see AI's role in healthcare: it's not a source of answers but a *grounding and safety layer* between generic medical knowledge and one person's real constraints. AIM forces a defined actor and measurable mission instead of a vague ask. MAP forces the model to use Memory, real Assets, and live tool Actions rather than guessing from training data. The Verifier Pattern showed me that in a low-oversight channel like SMS, safety depends on the AI confirming what it heard *before* acting — not just checking its own answer afterward.

---

## Sources (Assets grounding data)
1. Daily Monitor (Uganda), "The importance of growing green cooked banana," 2025 — matooke's share of daily caloric intake.
2. ScienceDirect, food consumption patterns study, rural Eastern Uganda, 2024 — dietary diversity and beans' share of energy.
3. Sollay Kenyan Foundation, "Surprising Facts About Healthcare Access In Rural Kenya," 2025; World Vision, "Kenya's Community Health Workers" — rural population share.
4. PLOS One, "Improving Maternal and Newborn Health: Effectiveness of a Community Health Worker Program in Rural Kenya," 2014 — CHW-to-household ratios.
5. PMC, "Performance of community health workers... Wakiso district, Uganda," 2020 — CHW caseload vs. MOH target.
6. Frontiers in Public Health, "Geographic accessibility to health facilities in Kenya," 2022; Haller Foundation, 2025 — distance-to-facility benchmarks.
7. PMC, "Community involvement in obstetric emergency management... Rukungiri district, Western Uganda" — proximity vs. doctor-population ratio.
8. mpesa.or.ke (2026) and pesatrail.com (2026) — M-Pesa fee structure for small transactions.
