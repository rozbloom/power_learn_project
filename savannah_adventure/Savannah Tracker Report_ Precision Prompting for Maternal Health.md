# Savannah Tracker Report: Precision Prompting for Maternal Health
**Organization:** AfyaTech  
**Role:** AI Strategy Lead  
**Date:** May 1, 2026  

---

## Introduction
Precision in maternal health AI is a matter of life and death. Generic advice fails to account for the unique socio-economic and cultural realities of rural Kenya and Uganda. By localizing prompts, we bridge the gap between medical theory and community reality, ensuring mothers receive actionable, safe, and culturally resonant support.

---

## Rewritten Prompts

### Prompt A: Nutrition Advice
**Original:** “Give nutrition tips for pregnant women.”

**Rewritten Prompt:**
> "You are a Kenyan maternal nutritionist (A). Your patient is an expectant mother in rural Uganda who relies on local staples like matooke and ugali (I). Provide a daily meal plan that ensures she gets 60% of her calories from matooke while supplementing with affordable local proteins like beans or silverfish (M). Ensure the advice respects local cultural norms around pregnancy diets (P) and accounts for the fact that she may not have refrigeration (A)."

**Annotations:**
*   **AIM:** **A**ctor (Kenyan nutritionist), **I**nput (Rural Uganda, matooke/ugali staples), **M**ission (Daily meal plan with specific calorie targets).
*   **MAP:** **M**emory (Expectant mother status), **A**ssets/Actions (Local proteins, no refrigeration), **P**rompt (The specific instruction for a meal plan).

**Key Improvement:** **Cultural & Nutritional Alignment.** By specifying "matooke" and "silverfish," the AI avoids suggesting expensive, imported "superfoods" (like kale or salmon) that are unavailable or unaffordable, reducing the risk of the advice being ignored.

---

### Prompt B: Appointment Reminders
**Original:** “Remind users about doctor visits.”

**Rewritten Prompt:**
> "Act as a Community Health Liaison (A). Using the clinic's Tuesday-only availability and the fact that 70% of our users live >5km from the facility (I), draft an SMS reminder for a prenatal checkup (M). Remind the mother to set aside M-Pesa funds for transport (A) and suggest she coordinates with the local Community Health Worker (CHW) for a group walk if travel feels unsafe (P)."

**Annotations:**
*   **AIM:** **A**ctor (Health Liaison), **I**nput (Tuesday-only clinic, >5km distance), **M**ission (Draft SMS reminder).
*   **MAP:** **M**emory (Upcoming prenatal checkup), **A**ssets/Actions (M-Pesa funds, CHW schedule), **P**rompt (The reminder text with safety suggestions).

**Key Improvement:** **Operational Realism.** The original prompt ignores that clinics aren't open daily and that transport costs are a major barrier. This version proactively addresses "M-Pesa constraints" and "travel time."

---

### Prompt C: Emergency Triage
**Original:** “Tell me what to do if I feel unwell during pregnancy.”

**Rewritten Prompt:**
> "First, identify the specific symptom the mother is reporting (e.g., bleeding, severe headache, or blurred vision). Then, follow this logic: 
> 1. If the symptom is a 'Red Flag' (list provided in assets), immediately advise calling the emergency CHW number. 
> 2. If it is a minor discomfort, suggest a local remedy (e.g., ginger for nausea). 
> 3. Finally, have a 'Verifier' step: Ask the AI to double-check if the advice given could cause delay in seeking life-saving care. If yes, default to 'Go to the clinic immediately' (M)."

**Annotations:**
*   **AIM:** **A**ctor (Triage Assistant), **I**nput (Specific symptoms), **M**ission (Guide safe next steps).
*   **MAP:** **M**emory (Pregnancy stage), **A**ssets/Actions (Red Flag list, CHW number), **P**rompt (Chain-of-Thought triage logic).

**Key Improvement:** **Reduced Risk of Harm.** By using the **Verifier Pattern**, the AI is forced to check its own work for safety, ensuring it never downplays a life-threatening symptom as a "minor discomfort."

---

## Reflection
This challenge has fundamentally shifted my view of AI in healthcare from a "knowledge dispenser" to a "contextual navigator." In high-stakes environments like maternal health, the AI's value isn't just in what it knows, but in how it filters that knowledge through the lens of a user's specific reality—their diet, their wallet, and their geography. Precision prompting isn't just a technical skill; it is an act of empathy that ensures technology serves the most vulnerable rather than excluding them.
