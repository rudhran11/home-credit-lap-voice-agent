# Test Plan

Baseline answers unless stated: residential, sole, originals available, ₹50 lakh, salaried, bank, ₹1 crore, 10 years. The agent's speaking role is played by Retell; the tester plays the customer in natural sentences.

| ID | Scenario | Say at the key moment | Expected behaviour |
| --- | --- | --- | --- |
| T1 | Fully eligible | Baseline answers | Asks 7 items in order, no repeats, hands off to senior loan expert |
| T2 | Agricultural | "It's agricultural land." | Polite disqualification, ends |
| T3 | Cash income | "Self-employed, income is in cash." | Polite disqualification, ends |
| T4 | No originals | "I only have photocopies." | Polite disqualification, ends |
| T5 | Tenure 2 years | "Two years." | Disqualification |
| T6 | Tenure 3 years | "Three years." | Accepted, handoff |
| T7 | Tenure 15 years | "Fifteen years." | Accepted, handoff |
| T8 | Tenure 16 years | "Sixteen years." | Disqualification |
| T9 | ₹50 lakh | "Fifty lakh." | Continues, no cap message |
| T10 | ₹75 lakh | "Seventy-five lakh." | Continues, no cap message |
| T11 | ₹90 lakh, accepts cap | "Ninety lakh." → "Okay, 75 is fine." | Explains ₹75 lakh max, continues at ₹75 lakh |
| T12 | ₹90 lakh, declines cap | "Ninety lakh." → "No, I need ninety." | Polite close, no further questions |
| T13 | Joint ownership | "Joint, with my brother." | Continues |
| T14 | Existing loan | "I already have a loan on this property." | Loan-transfer specialist message, ends |
| T15 | EMI reduction | Mid-flow: "I just want my EMI reduced." | Loan-transfer specialist message, ends |
| T16 | Busy | "I'm in a meeting." → "Call after 6 pm." | Asks callback time, confirms, ends |
| T17 | Out-of-order | "Residential, jointly owned with my wife, worth about 80 lakh." | Next asks documents; never re-asks those 3 |
| T18 | Multiple answers | "My own house, originals at home, need 40 lakh, 10 years." | Asks only occupation/income, then market value |
| T19 | Contradiction | "Residential… actually no, agricultural." | Uses latest value → disqualification |
| T20 | Ambiguous documents | "Papers should be somewhere." | One clarifying question, no assumption |
| T21 | Interest rate | "What interest rate do you offer?" | Defers to senior expert, no number, resumes |
| T22 | Unrelated question | "Where's your nearest branch?" | No invented address, resumes |
| T23 | Interruption | Interrupt: "Wait, what documents do you need?" | Answers/defers, re-asks pending item |
| T24 | Correction | "Ten years… make that twelve." | Uses 12 |
| T25 | Repeated info | Repeat "it's residential" when asked ownership | Re-asks ownership only |
