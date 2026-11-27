# fitcoach-ai-assistant

FitCoach AI — Health & Fitness Assistant for women 40+ (LangChain + LangGraph)

## Overview

FitCoach is a conversational MVP assistant designed around health, nutrition, and safe movement guidance for women aged 40+, while respecting clinical boundaries and avoiding medical diagnosis or prescriptions

## MVP Architecture

* **Agent core:** Single ReAct agent powered by `gpt-4o-mini` using LangChain and LangGraph
* **Deterministic Python tools:**
  * `bmi_calculator` — calculates BMI from user parameters
  * `calorie_calculator` — calculates BMR, TDEE, protein, and calcium targets
  * `exercise_lookup` — retrieves exercises from the local `EXERCISES_DB` dictionary
* **Session state:** `MemorySaver` preserves conversation context within a session, and `/reset` resets state or rotates `thread_id`
* **Scope:** No multi-agent routing and no external open web search in the current MVP

## Testing & Known Behavioral Limits

* Evaluated across 14 test scenarios (calculations, session memory and `/reset`, clinical guardrails, out-of-domain refusals, and edge/ambiguous prompts)
* **Observed behavioral deviations (Tests 13–14):**
  * **Test 13:** An indirect weight-loss prompt without explicit numeric parameters or metric triggers can result in a conversational response without calling calculation tools
  * **Test 14:** A gym back-exercise prompt can skip `exercise_lookup`, trigger fallback text, and include exercises outside `EXERCISES_DB` due to prompt instruction competition and probabilistic generation.
* **Next steps:** Expand `EXERCISES_DB` into a closed verified RAG knowledge base, duplicate safety rules and tool constraints in the system prompt, and add API retry/fallback and persistent storage for production

## Estimated Cost (`gpt-4o-mini`)

* **Pricing reference:** `$0.15 / 1M` input tokens and `$0.60 / 1M` output tokens
* **Per request:** `~$0.0004` (`~$0.0003–$0.0005` depending on tool calls)
* **Monthly estimate (at ~30 requests/user/month):**
  * **100 users (~3,000 requests):** `~$1.20 / month`
  * **1,000 users (~30,000 requests):** `~$12.00 / month`

## How to Run

1. Open `Final_Project_Deineka_FitCoach.ipynb` in Google Colab
2. Configure `OPENAI_API_KEY` in Colab Secrets (or enter it when prompted)
3. Run the notebook cells to test the assistant and review the evaluation scenarios
