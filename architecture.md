# Architecture

Author: Rudhran B

## Per-turn loop

```
customer turn
  → extract every fact (not just the answer asked for)
  → normalize ("my flat" → residential, "half a crore" → ₹50 lakh, "36 months" → 3 years)
  → update state (latest clear correction wins; unclear values stay UNKNOWN)
  → check exits, first match wins:
      1. wrong person        → apologise, no offer details, end
      2. busy                → ask callback time, end
      3. existing loan / EMI → loan-transfer specialist message, end
      4. rule failed         → polite disqualification, end
      5. > ₹75 lakh pending  → offer ₹75 lakh (no → end)
      6. customer question   → answer from RAG or defer to senior expert
      7. unclear value       → one clarifying question
      8. item still UNKNOWN  → ask the first missing one (order 1→7)
      9. all 7 passed        → senior loan expert handoff, end
```

## State

| Field | Values |
| --- | --- |
| customer_verified, customer_available | UNKNOWN / TRUE / FALSE |
| property_type | residential / commercial / industrial / agricultural |
| ownership_status | sole / joint |
| documents_available | TRUE / FALSE |
| loan_amount | ₹ value, or pending cap confirmation |
| occupation, income_mode | salaried / self_employed; bank / cash |
| market_value | ₹ estimate (no threshold) |
| tenure | years |
| existing_property_loan, emi_reduction_request | TRUE / FALSE |
| disqualified, transfer_required, callback_required | flags with reason / time |

The LLM is stateless, so the prompt tells it to rebuild this state silently from the conversation history every turn.

## Why exits come before questions

A disqualified or transfer customer can never be asked another eligibility question, because those checks run before the "ask next item" step on every turn.
