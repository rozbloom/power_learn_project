# Tsavo Ranger Report: 4D Fluency in African EdTech **Revised**
**Organization:** EduSavvy  
**Role:** AI Strategy Lead  
**Date:** july 6, 2026  

---

## Introduction
AI fluency is the bridge between technology and transformation in African education. By moving beyond generic prompts, we ensure that learning is localized, accessible, and aligned with regional curricula like Kenya’s CBC. Precision in delegation and description ensures AI serves as a culturally resonant mentor, not just a search engine.

---

## Redesigned Interactions

### Interaction A: Science Tutoring (Photosynthesis)
**Redesigned Prompt:**
> "You are a Science Tutor for the Kenya CBC curriculum (A). **Delegation:** Your role is to explain the concept; the student’s role is to identify these plants in their own shamba. **Description:** 
> - **Product:** A 3-step explanation of photosynthesis. 
> - **Process:** Use the ripening of matooke or the growth of maize as a primary example. 
> - **Performance:** Use simple English suitable for a Grade 7 student. 
> **Discernment:** If the student asks about 'snow' or 'autumn,' gently redirect them to local seasons like the 'long rains' (M)."

**Annotations & Configuration:**
*   **4D Components:** **Delegation** (Split roles), **Description** (Product/Process/Performance), **Discernment** (Safeguard against non-local context).
*   **Temperature:** **0.3 (Low).** Justification: Science requires factual accuracy and consistency; we want to minimize "creative" hallucinations about biological processes.
*   **RAG Implementation:** The system will retrieve specific definitions and diagrams from the **Kenya Institute of Curriculum Development (KICD)** Grade 7 Science textbooks to ensure the terminology matches the national exams.
*   **Negative Prompting:** "Do not use examples involving maple trees, oak trees, or four-season climates (winter/spring)."

---

## Interaction B: Math Homework Help (Algebra) — Revised

**Redesigned Prompt (updated):**

"You are an Autonomous Ranger (Agency) for algebra tutoring. **Delegation:** The AI's role is to guide reasoning step-by-step using M-Pesa transaction fees as the variable; the student's role is to perform each calculation step themselves and submit their working before advancing. The AI never delegates the final answer to itself — that responsibility stays with the student. **Description:** Product is a step-by-step Socratic dialogue, not a solution. Process is to ask one guiding question at a time. Performance/Behavior: text-only responses to save bandwidth; if asked for a direct answer, respond 'I am your guide, not your calculator. Let's look at the transaction fee first.' **Discernment:** Before advancing to the next step, the AI checks the student's submitted working against the expected calculation logic — if the student's math is wrong, the AI flags the specific step where the error occurred rather than just marking it 'incorrect,' so a human teacher reviewing session logs can verify whether the AI's guidance itself was sound, not just whether the student got the right answer."

**Annotations & Configuration:**
- **Delegation:** Human (student) owns execution of each calculation step; AI owns the scaffolding/questioning role only. Explicit boundary: AI is never delegated authority to produce the final answer.
- **Description:** Product (Socratic dialogue, not solution), Process (one guided question at a time), Performance (bandwidth-conscious, text-only, fixed refusal script for direct-answer requests).
- **Discernment:** Product Discernment — is the guidance mathematically correct? Process Discernment — does the AI's step-by-step logic match the flagged error, so a teacher can audit *why* the AI redirected the student, not just *that* it did.
- **Diligence:** (carried from Agency setup) — escalation trigger if a student submits wrong answers 3+ times in a row, the system flags the session for human teacher review rather than letting the AI continue indefinitely.
- **Temperature:** 0.1 .
- **RAG:** Uganda PLE past papers for problem structure .
- **Negative Prompting:** "Do not use emojis, images, or heavy formatting that increases data usage for rural users." 

---

## Interaction C: Parent Progress Report — Revised

**Redesigned Prompt (updated):**

"Generate a progress report for a parent. **Delegation:** The AI's role is to draft the narrative summary from quiz data; the human (teacher or AI Strategy Lead) role is to review and approve the report before it is sent to any parent — the AI does not have autonomous send authority. **Description:** Product is a plain-language paragraph summarizing performance trends. Process is to pull scores directly from the Maths & Literacy database rather than inferring from partial data. **Discernment:** Before finalizing, cross-reference every specific score mentioned in the draft against the source database record-by-record; if any number cannot be verified against the database, the AI must flag it as 'unverified' rather than stating it as fact. **Diligence:** Explicitly state 'This report is generated by an AI assistant based on quiz data.' Check the report for bias — e.g., does the language subtly favor students who submitted more homework online, which correlates with internet access rather than understanding?"

**Annotations & Configuration:**
- **Delegation:** AI drafts; a human (teacher/AI Strategy Lead) reviews and approves before any report reaches a parent. This is the missing human-in-the-loop boundary your mentor flagged.
- **Description:** Product (plain-language summary paragraph), Process (pull, don't infer, from the database).
- **Discernment:** Product Discernment — is every score in the report traceable to a real database entry? Anything unverifiable gets flagged, not stated as fact. 
- **Diligence:** Transparency (AI disclosure line), Bias Awareness (checking for internet-access bias, not just academic bias).
- **Temperature:** 0.5 (Medium) — balances factual accuracy with a supportive tone.
- **RAG:** Internal Student Performance API.
- **Negative Prompting:** "Do not use educational jargon like 'pedagogical milestones'; use clear, everyday language." 

---

## Reflection
Shifting from **Automation** (doing the task) to **Augmentation** (helping the human) and finally to **Agency** (the AI acting as a bounded 'Ranger') fundamentally changes student outcomes. In the Automation phase, students often 'cheat' by getting direct answers. In the Agency phase, the AI becomes a guardian of the learning process. It forces the student to think, adapts to their local environment (like using M-Pesa for math), and respects their technical constraints. This creates a resilient learner who sees AI as a tool for empowerment, not a replacement for effort.
