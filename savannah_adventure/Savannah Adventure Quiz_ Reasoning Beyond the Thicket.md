# Savannah Adventure Quiz: Reasoning Beyond the Thicket
**Course:** Power Learn Project  
**Topic:** Advanced Prompt Engineering in Health Tech  

---

## 1. The Kakamega Kale Failure (MAP Framework)
**The Failure:** This is a failure of **Assets (A)** and **Grounding**. The AI relied on its general training data (which says kale is healthy) but lacked the specific "Asset" of a local food availability map for Kakamega County. Without this context, the AI "hallucinated" that kale was a viable option for the user.

**Redesigned Prompt (MAP):**
> "Using the **Local Food Availability Database (Asset)** for Kakamega County, identify the top 3 iron-rich leafy greens currently in season. **Memory:** The user is a mother in a rural household with no refrigeration. **Prompt:** Suggest a meal using these local greens (e.g., *mshicha* or *managu*) that can be prepared using traditional cooking methods."

---

## 2. The Abdominal Pain Panic (Verifier Pattern)
**The Concept:** The **Verifier Pattern** forces the AI to stop and check its assumptions before giving a high-stakes answer. It prevents "alarmist" advice by requiring more data first.

**Redesigned Prompt (Verifier Pattern):**
> "If a user reports abdominal pain, **DO NOT** immediately advise a clinic visit. Instead, follow this protocol:
> 1. **Clarify:** Ask the user: 'Is the pain sharp or dull? Is it accompanied by bleeding or fever?'
> 2. **Verify:** Based on their answer, check if the symptoms match the 'Emergency Red Flag' list.
> 3. **Action:** Only if a Red Flag is present, advise immediate transport. Otherwise, provide comfort measures and ask them to monitor for specific changes."

---

## 3. WHO Guidelines vs. Chain-of-Thought (CoT)
**The Justification:** Direct summarization of WHO guidelines is often too academic and "urban-centric." **Chain-of-Thought (CoT)** creates more value because it "reasons" through the implementation.
*   **WHO Summary:** "Exclusive breastfeeding is recommended for 6 months." (Fact)
*   **CoT Adaptation:** "WHO recommends 6 months of breastfeeding. *However*, in rural East Africa, mothers often return to farm work early. *Therefore*, we should provide a schedule for expressing milk and storage tips using traditional cooling pots (*pod pots*) to make the guideline achievable."
**Value:** CoT turns a "what" (the guideline) into a "how" (the local solution).

---

## 4. The Outdated Data Catch (OCEAN Framework)
**The Concept:** The **OCEAN Framework** is a quality-control checklist used to evaluate AI output for "taste" and "truth."
*   **O - Originality:** Does this sound like a generic bot or a local expert?
*   **C - Concrete Details:** Are the numbers specific and cited?
*   **E - Evident Logic:** Does the math/logic hold up?
*   **A - Assertive Direction:** Is the advice clear?
*   **N - Narrative:** Does it tell a consistent story?

**How it catches the error:** Under **Concrete Details (C)** and **Evident Logic (E)**, the OCEAN framework requires you to verify any "hard facts" against a trusted source. By checking the "Concrete" claim of 1.8 doctors against the latest 2025 Ministry of Health data, you would see the "Evident" contradiction and catch the hallucination before it reaches donors.

---

## Summary of Key Concepts
| Framework | Purpose |
| :--- | :--- |
| **MAP** | Anchors AI in real-world resources (Assets) and history (Memory). |
| **Verifier Pattern** | Prevents dangerous or panicky advice by forcing a "double-check" step. |
| **Chain-of-Thought** | Breaks down complex rules into local, step-by-step actions. |
| **OCEAN** | A final quality-control filter to ensure data is fresh, concrete, and logical. |
