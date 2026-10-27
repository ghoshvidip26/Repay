# Theme 1: Voice - five winning ideas

Brief: the agent hears what someone needs in Hindi, Tamil, Kannada or Bengali and actually moves the money. Voice is the headline channel, so every idea below is spoken in, spoken back, and ends with a Cashfree sandbox payout or settlement.

Sandbox only. Scores are our inference (not a sponsor rubric). Example utterances are romanised with an English gloss and need a native-speaker check before the demo. Facts about Cashfree and Sarvam come from the notes in `../research/`.

## Shared voice rules (all five ideas)

- **Saaras v4** for speech to text: run `transcribe` and `verbatim` modes and refuse to continue if the two amounts disagree; use keyterm prompting (up to 50 names, REST only) for payee names. Real-time REST speech to text is capped at 30 seconds, so keep utterances short.
- **sarvam-105b-conversations** with reasoning turned off, emitting only a JSON-schema intent. Code does the arithmetic and parses the numerals, never the model.
- **Bulbul v4 Flash** for spoken replies. No SSML, and romanised input degrades the audio, so write every readback in native script with amounts as words. Cache demo phrases ahead of time.
- Confirmation is a **tap**, not a voice command. Every payout carries an idempotency key or unique `transfer_id`. After a 5XX, check status; never re-send blindly.

---

## 1. Raitha Payout (Kannada) - farmer-producer mandi payout desk

- **Pitch:** An FPO secretary dictates the day's purchases. The agent verifies every farmer's bank account, gets one approval, pays the whole batch, and explains in Kannada which payments are done, pending or reversed.
- **User and utterance:** Kannada-speaking FPO or mandi secretary. *"Ramannanige hadinentu kvintal ragi, kvintalge mooru saavirada munnooru rupayi - hana haaki."* (Pay Ramanna for 18 quintals of ragi at Rs 3,300 per quintal.) Code computes Rs 59,400.
- **How money moves:** Secure ID bank account verification plus Name Match gates each farmer (mock scores: 100 direct, 85 good, 70 moderate, 38 poor). Then Payouts V2 Batch Transfer (bank only) with the optional approve-batch step and Get Batch Transfer Status. Sandbox magic accounts give success, invalid IFSC, pending-then-failure and a 25-second timeout. IMPS limit Rs 5 lakh.
- **Sarvam:** Saaras v4 (codemix, keyterms for farmer names), sarvam-105b-conversations, Bulbul v4 Flash Kannada summary.
- **Demo (3 steps):** (1) Secretary dictates three purchases; agent reads the total back in Kannada words. (2) Verification table flags one farmer (bad IFSC); secretary removes him and taps Approve. (3) Batch runs: two succeed, one goes pending then fails; the agent says so and offers a corrected-IFSC retry.
- **Why it wins:** The incumbent Sarvam-plus-payments demos are food, grocery and gold ordering. A producer-side batch payout with visible failure and recovery is absent, and it uses Cashfree's most deterministic sandbox surface.
- **Feasibility:** 4/5. Batch approval and Secure ID must be enabled on the test account, so request them early.
- **Risk and guardrails:** Enablement delay. Payee only from a pre-registered farmer list; per-batch cap; mock adapter if batch approval is not enabled.

## 2. Dihadi Desk (Hindi) - daily-wage payouts, including workers with no bank details

- **Pitch:** A contractor dictates the day's wages for twelve workers. Workers with UPI are paid at once; the rest get a payout link to supply their own details.
- **User and utterance:** Hindi-speaking building contractor. *"Aaj barah mazdooron ki dihaadi, har ek ko paanch sau bees rupaye - jinka bank nahi hai unhe link bhej do."* (12 x Rs 520 = Rs 6,240.)
- **How money moves:** Payouts V2 Validate Payout (UPI VPA or phone, returns a single-use token), then Process Validated Payout. Batch Transfer for bank-account workers. Cashgram payout link for workers without details (sandbox behaviour unconfirmed, so build a mock link page behind the same interface).
- **Sarvam:** Saaras v4 Hindi (spoken numbers like "paanch sau bees"), 105B JSON schema with a worker list, Bulbul v4 Flash Hindi.
- **Demo (3 steps):** (1) One Hindi sentence yields a 12-row wage table totalling Rs 6,240. (2) Agent splits "pay now" from "needs link". (3) Contractor taps Approve; statuses stream in; one failure is retried on a corrected VPA.
- **Why it wins:** Unbanked-recipient payouts are a Payouts workflow, not a payment-link one, and contractor-side voice is absent from incumbent demos.
- **Feasibility:** 3/5.
- **Risk and guardrails:** Wrong worker matched, duplicate payment. Pre-registered site payee list; idempotent `transfer_id` per worker per date; per-worker cap; total read back.

## 3. Ghar Bhejo (Bengali) - verified remittance home

- **Pitch:** A Bengali-speaking migrant worker asks to send money to his mother. The agent verifies the account name, reads it back in Bengali, sends, and explains what happens if the transfer reverses.
- **User and utterance:** Migrant worker in Bengaluru. *"Murshidabade make ajoi dui hajar taka pathiye dao."* (Send Rs 2,000 to Mother in Murshidabad today.)
- **How money moves:** Pre-registered family payee, Secure ID bank verification and Name Match (a "john d" vs "JOHN DOE" variant scores 85; a 38 blocks), then Payouts V2 Standard Transfer by IMPS. Sandbox accounts cover success, pending-then-success and success-then-reversed. Webhooks TRANSFER_SUCCESS and TRANSFER_REVERSED.
- **Sarvam:** Saaras v4 (`bn-IN`, codemix), sarvam-105b-conversations tool call, Bulbul v4 Flash Bengali, Mayura for a short Hindi or English note to the receiver.
- **Demo (3 steps):** (1) Bengali request; "Ma" matches the saved payee. (2) Name-match score shown; a mid score forces extra confirmation. (3) Transfer succeeds; the reversed-account run shows the agent explaining in Bengali that the bank returned the money.
- **Why it wins:** The structural pain is real (57% of surveyed migrants used informal channels, but that CGAP study is from 2011). Incumbent voice products are generic UPI apps; none found verifies the payee and narrates reversals.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Remittance is regulated and the notes did not research RBI/NPCI rules for it. Never imply the sandbox proves licensing. Cooling-off for first-time payees; human tap before execution.

## 4. Saath-Saath Split (Tamil) - seller-side settlement for informal collectives

- **Pitch:** One customer order is split among the members of a women-run tiffin collective by the percentages the leader speaks, with simulated vendor settlements and failure handling.
- **User and utterance:** Collective leader. *"Innaikku Pongal order moovayiram roobai - Meenavukku narpadhu sadhaveedham, Kalavukku muppathi-anju, meedhi kuzhu nidhikku."* (40/35/25 of Rs 3,000.)
- **How money moves:** Cashfree Easy Split: enable via API (poll for ACTIVE), create an order with `order_splits`, pay with test UPI VPA `testsuccess@gocash`, then Simulate Vendor Settlement to INITIATED, SUCCESS or FAILED. Test vendor accounts include invalid IFSC and a name mismatch. Vendor settlement webhooks need webhook version 2025-01-01.
- **Sarvam:** Saaras v4 Tamil, 105B JSON schema with percentages that must sum to 100 (checked in code), Bulbul v4 Flash Tamil.
- **Demo (3 steps):** (1) Tamil sentence builds a split table; each share is read back in Tamil words. (2) Customer pays; agent confirms the collection. (3) Settlement simulated: Meena succeeds, Kala fails on a name mismatch; the agent explains and queues a fix.
- **Why it wins:** Seller-side marketplace split for collectives, not payer-side bill split, and the research found no AI precedent for Easy Split. It is the most Cashfree-specific feature in the notes.
- **Feasibility:** 3/5. Enablement can stall in RISK_APPROVAL_PENDING or RISK_REJECTED; reversals cannot be simulated.
- **Risk and guardrails:** If enablement fails the demo dies. Request it now; keep a recorded-response fallback; percentage-sum check; readback of rupee shares.

## 5. Doodh Hisaab (Kannada) - dairy cooperative fortnightly payouts

- **Pitch:** A village dairy cooperative secretary speaks each farmer's milk collection for the fortnight. The agent applies the co-op's rate chart in code, shows what each farmer is owed, verifies accounts, and pays the batch.
- **User and utterance:** Cooperative secretary. *"Lakshmammanige ippattu litre, fat naalku point eppattu - eshtu aagutte?"* (Lakshmamma delivered 20 litres at 4.8 fat; what is she owed?) The agent answers in Kannada after code applies the rate chart.
- **How money moves:** Same verify-then-batch chain as Raitha Payout (Secure ID bank verification plus Name Match, Payouts V2 Batch Transfer with approval and status). The rate chart is supplied by the co-op as a file (inference: the notes contain no dairy rate data).
- **Sarvam:** Saaras v4 Kannada with keyterms for member names, 105B JSON schema for litre and fat values, Bulbul v4 Flash Kannada.
- **Demo (3 steps):** (1) Secretary dictates four farmers' collections; agent reads back each amount in Kannada words. (2) One farmer's account fails Name Match and is held for correction. (3) Approved batch pays; one pending-then-failed transfer is explained and retried.
- **Why it wins:** Recurring fortnightly cycle, a computed rate (not a typed amount), and an underserved rural payer. It reuses the strongest sandbox surface with a different user.
- **Feasibility:** 4/5 once Raitha Payout's chain works. Pick one of the two for the demo, not both.
- **Risk and guardrails:** Rate-chart errors. Show the computation for each farmer on screen; code, not the model, calculates; sandbox only; per-farmer cap.

---

## Ranking within this theme (our inference, /25)

| Rank | Idea | Total |
|---|---|---|
| 1 | Raitha Payout | 23 |
| 2 | Ghar Bhejo | 22 |
| 3 | Dihadi Desk | 21 |
| 4 | Saath-Saath Split | 20 |
| 5 | Doodh Hisaab | 19 |
