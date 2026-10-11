# Approach

Author: Rudhran B · rudhran1108@gmail.com

## Problem

Home Credit wants a voice agent that verifies an existing customer, presents a pre-approved LAP offer of up to ₹75 lakh as a loyalty reward, collects 7 eligibility items, stops immediately on any failing answer, routes existing-loan / EMI-reduction customers to a loan-transfer specialist, and hands qualified customers to a senior loan expert. The hard part is real customers: they answer out of order, correct themselves, interrupt, and ask things the agent cannot know.

## Design

The agent is a state machine written in natural language. Each turn it extracts every fact, normalizes it, updates state, checks exits in a fixed priority order, then asks only the first missing item. The script never decides the next question; the state does.

## Key behaviours

- **Out-of-order answers:** "Residential, jointly owned with my wife, worth eighty lakh" stores three items; the next question is original documents.
- **Corrections:** the latest clear statement wins ("ten… make that twelve" → 12 years).
- **Ambiguity:** "papers should be somewhere" triggers one clarifying question; nothing is assumed.
- **Diversions:** questions are answered from RAG or deferred to the senior expert, then the agent returns to the pending item. Items skipped during a diversion are asked before handoff.
- **No hallucination:** interest rates, fees, timelines and branch details are never invented.
- **Language and gender:** `language_to_speak` and `agent_gender` are held for the whole call, including Hindi gendered verb forms.

## Testing

25 scenarios (see `tests/test_plan.md`) cover every terminal path, boundary values (tenure 2/3/15/16, loan ₹50/75/90 lakh), and natural-speech cases. Results in `tests/test_results.md` come only from real calls; anything not run is marked NOT RUN.

## Limitations

- State lives in the model's reasoning over the transcript, not in code, so long or noisy calls can still cause errors.
- Speech-to-text mistakes ("fifteen" vs "fifty") are outside the prompt's control.
- Design decisions the brief leaves open are listed in the README.
