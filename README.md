# fitcoach-ai-assistant

FitCoach AI — Health & Fitness Assistant for women 40+ (LangChain + LangGraph)[cite: 21].

## Overview

FitCoach is a conversational MVP assistant designed around health, nutrition, and safe movement guidance for women aged 40+, while respecting clinical boundaries and avoiding medical diagnosis or prescriptions[cite: 15, 21].

## MVP Architecture

* **Agent core:** Single ReAct agent powered by `gpt-4o-mini` using LangChain and LangGraph[cite: 15, 20, 21].
* **Deterministic Python tools:**
  * `bmi_calculator` — calculates BMI from user parameters[cite: 14, 15, 21].
  * `calorie_calculator` — calculates BMR, TDEE, protein, and calcium targets[cite: 14, 15, 21].
  * `exercise_lookup` — retrieves exercises from the local `EXERCISES_DB` dictionary[cite: 14, 15, 21].
* **Session state:** `MemorySaver` preserves conversation context within a session, and `/reset` resets state or rotates `thread_id`[cite: 15, 21].
* **Scope:** No multi-agent routing and no external open web search in the current MVP[cite: 15].

## Testing & Known Behavioral Limits

* Evaluated across 14 test scenarios (calculations, session memory and `/reset`, clinical guardrails, out-of-domain refusals, and edge/ambiguous prompts)[cite: 14, 15].
* **Observed behavioral deviations (Tests 13–14):**
  * **Test 13:** An indirect weight-loss prompt without explicit numeric parameters or metric triggers can result in a conversational response without calling calculation tools[cite: 14, 15].
  * **Test 14:** A gym back-exercise prompt can skip `exercise_lookup`, trigger fallback text, and include exercises outside `EXERCISES_DB` due to prompt instruction competition and probabilistic generation[cite: 14, 15].
* **Next steps:** Expand `EXERCISES_DB` into a closed verified RAG knowledge base, duplicate safety rules and tool constraints in the system prompt, and add API retry/fallback and persistent storage for production[cite: 15, 20, 21, 22].

## Estimated Cost (`gpt-4o-mini`)

* **Pricing reference:** `$0.15 / 1M` input tokens and `$0.60 / 1M` output tokens[cite: 20].
* **Per request:** `~$0.0004` (`~$0.0003–$0.0005` depending on tool calls)[cite: 20].
* **Monthly estimate (at ~30 requests/user/month):**
  * **100 users (~3,000 requests):** `~$1.20 / month`[cite: 20]
  * **1,000 users (~30,000 requests):** `~$12.00 / month`[cite: 20]

## How to Run

1. Open `Final_Project_Deineka_FitCoach.ipynb` in Google Colab[cite: 21].
2. Configure `OPENAI_API_KEY` in Colab Secrets (or enter it when prompted)[cite: 21].
3. Run the notebook cells to test the assistant and review the evaluation scenarios[cite: 14, 21].
