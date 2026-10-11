# Home Credit LAP Voice Agent

AI voice agent that calls existing Home Credit customers about a pre-approved Loan Against Property (LAP) offer of up to ₹75 lakh, runs a 7-item preliminary eligibility check, and hands qualified leads to a senior loan expert.

**Author:** Rudhran B · rudhran1108@gmail.com
**Platform:** Retell AI (single-prompt agent)

## What's in this repo

| Path | Contents |
| --- | --- |
| `system_prompt.txt` | Final system prompt, pasted unchanged into Retell |
| `docs/architecture.md` | State model, per-turn decision order, eligibility rules |
| `docs/approach.md` | Approach note: problem, design, edge cases, limitations |
| `docs/assessment-submission.pdf` | Full design and test document |
| `tests/test_plan.md` | 25 test scenarios with expected behaviour |
| `tests/test_results.md` | Observed results from real test calls |
| `evidence/` | Links to call recordings and transcripts |

## How the agent works

Every customer turn runs the same loop: extract all facts → normalize → update state → check exits → ask the first missing item.

Exits are checked before any question, in this order: wrong person → busy (ask callback time) → existing loan / EMI reduction (loan-transfer specialist) → failed eligibility rule (polite disqualification) → loan above ₹75 lakh (offer ₹75 lakh) → next missing item → senior loan expert handoff.

## Eligibility rules (from the assignment brief only)

| Item | Eligible | Not eligible |
| --- | --- | --- |
| Property type | Residential, Commercial, Industrial | Agricultural |
| Ownership | Sole or Joint | — |
| Original documents | Available for verification | Not available |
| Loan amount | ≤ ₹75 lakh | > ₹75 lakh → offer ₹75 lakh |
| Occupation + income | Salaried or Self-employed, income via bank | Income in cash |
| Market value | Any estimate (no threshold) | — |
| Tenure | 3–15 years | < 3 or > 15 years |

The agent never invents interest rates, fees or timelines; it defers them to the senior loan expert.

## Running it

1. Create a Single Prompt agent in Retell AI and paste `system_prompt.txt`.
2. Set test values for the dynamic variables (`company_name`, `customer_name`, `agent_name`, `agent_gender`, `language_to_speak`, `current_date`, `current_day`, `current_time`, `additional_context_from_rag`, `conversation_history`, `customer_utterance`).
3. Set the agent to speak first, then run web test calls using the scripts in `tests/test_plan.md`.

## Design decisions not fixed by the brief

- Existing loan + a disqualifying answer together → loan-transfer path wins.
- "Between 3 and 15 years" is treated as inclusive.
- The agent asks once whether a loan already exists on the property.
- An occupation that is neither salaried nor self-employed is treated as not meeting criteria after one clarification.
