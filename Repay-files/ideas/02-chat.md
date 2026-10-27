# Theme 2: Chat - five winning ideas

Brief: voice, chat, documents or WhatsApp. Chat here means a typed (or tapped) conversation in the app: the user types in Hindi, Tamil, Kannada or Bengali, the agent understands it, and money actually moves. Chat is also the fallback every voice idea needs, so these ideas lean on structure, ledgers and approvals.

Sandbox only. Scores are our inference. Example messages are romanised with an English gloss and need a native-speaker check. Cashfree and Sarvam facts come from the notes in `../research/`.

## Shared chat rules

- **sarvam-105b-conversations** with reasoning turned off, emitting a JSON-schema intent; code validates and computes. Use **Mayura** if the two parties use different languages.
- Spell amounts as words in the reply; confirm by button, not by a typed "yes". Idempotency key on every mutating call.
- Sarvam Chat is rate limited to 60 requests a minute per account, shared across a team.

---

## 1. Salary Saathi (Hindi) - household payroll with advance deductions

- **Pitch:** An employer types the monthly salary and the advance to recover. The agent verifies the worker's UPI ID, pays the net, keeps the advance ledger, and sends the worker a receipt in her language.
- **User and message:** Employer paying a domestic worker. *"Sunita didi ki is mahine ki tankhwah aath hazaar, pandrah sau ka advance kaatkar bhej do."* Net Rs 6,500, computed in code.
- **How money moves:** UPI Penny Drop (`valid@ybl`), then Payouts V2 UPI transfer (`success@upi`; `failure@upi` for the recovery run). Scheduling is an agent-side cron plus a Standard Transfer; the notes found no native scheduled-payout API or payroll product.
- **Sarvam:** 105B tool call, Bulbul v4 Flash voice receipt for the worker, Mayura if the employer and worker languages differ.
- **Demo (3 steps):** (1) Hindi request; agent shows "Rs 8,000 minus Rs 1,500 equals Rs 6,500". (2) Employer approves; UPI payout succeeds. (3) Worker's spoken receipt plays and the advance balance updates.
- **Why it wins:** A ledger problem the voice-UPI incumbents do not solve, with a receipt for a worker who cannot read confirmations. The plainest demo in the set, so polish matters.
- **Feasibility:** 5/5.
- **Risk and guardrails:** A mis-heard "dedh" or "dhai" quantity. Single-payee cap; net figure confirmed; no scheduled-payout claim unless built.

## 2. Bank-Badal Guard (Tamil) - supplier changed-bank-details guard

- **Pitch:** A supplier emails "our bank account has changed". The owner pastes it into chat; the agent re-verifies the new account, compares the name to the GSTIN legal name and the vendor on file, and holds the payout until it passes and an approver taps.
- **User and message:** MSME owner. *"Indha supplier pudhu account number anuppiirukkaanga - paanam anuppalaama?"* (This supplier sent a new account number; is it safe to pay?)
- **How money moves:** Secure ID bank account verification (penny drop with name-match scores) and GSTIN check (mock `29AAICP2912R1ZR` valid, `29AAIZP2912R1ZR` invalid), then Payouts V2 Standard Transfer after approval. Cashfree has no "changed bank detail" feature; the guard is orchestration we build (inference).
- **Sarvam:** 105B JSON schema to extract the new account details from pasted text, Saaras v4 if spoken, Bulbul v4 Flash Tamil explanation.
- **Demo (3 steps):** (1) Owner pastes the "new account" message. (2) Agent shows the mismatch (name score 38) and holds the payment. (3) Corrected details verify; owner approves; payout runs.
- **Why it wins:** Real Indian evidence: Dr. Reddy's reportedly lost about Rs 2.16 crore to a spoofed vendor email (reported Nov 2025). It is the verify-then-approve-then-pay gate, which no Cashfree product ships.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Name match can be wrong; say "unverified", never "safe". No real GSTIN or PAN of real businesses; sandbox only.

## 3. SHG Hisaab (Tamil) - self-help-group collections and member loans

- **Pitch:** The group leader types the week's results. The agent sends dues links to members, reconciles who has paid, and releases an approved member loan by payout after two approvals.
- **User and message:** SHG leader. *"Indha vaaram ovvoru uruppinarum aimbadhu roobai semippu; Raanikku irandaayiram kadan kodu."* (Rs 50 savings from each; lend Rs 2,000 to Rani.)
- **How money moves:** Payment Links for dues (expiry and `link_notify`; UPI links do not allow partial payments), reconciliation from link webhooks, then Payouts V2 Standard Transfer for the loan after leader plus treasurer approval. Subscriptions ON_DEMAND can model recurring savings charges, but the periodic cycle cannot be forced in sandbox.
- **Sarvam:** 105B tool use, Bulbul v4 Flash, Sarvam Vision for a printed ledger photo (handwritten Tamil accuracy is unverified).
- **Demo (3 steps):** (1) Tamil statement builds a dues table and sends links. (2) Test payments land; agent names who has not paid. (3) Loan needs two approvals; payout succeeds.
- **Why it wins:** Two-way group money (collect and disburse) with multi-approver governance.
- **Feasibility:** 3/5. Scope creep is the danger; ship the payout half first.
- **Risk and guardrails:** Chit-fund and lending rules are a grey area the notes did not research. Frame it as a savings-and-internal-loan ledger; two-approver rule; cap per loan.

## 4. RWA Society Desk (Kannada) - apartment dues in, vendor payouts out

- **Pitch:** A housing society treasurer types maintenance and defaulter updates. The agent chases dues, reconciles payments, and pays the security agency, cleaners and lift vendor after two committee approvals.
- **User and message:** Society treasurer. *"Flat nalku-nooru-eppattu avara maintenance baaki ide - link kalisu; security agency-ge hattu saavira kodu."* (Send a link for flat 470's dues; pay the security agency Rs 10,000.)
- **How money moves:** Payment Links or Payment Forms per flat (`order_tags` carry the flat number into the settlement recon report), Settlement Reconciliation API to match deposits to flats, then Payouts V2 to pre-verified vendors via penny drop and approval.
- **Sarvam:** 105B JSON schema, Saaras v4 if spoken, Bulbul v4 Flash Kannada summaries.
- **Demo (3 steps):** (1) Treasurer enters dues; links go out. (2) Test payments land and the recon API shows which flats cleared. (3) Vendor invoice approved by two committee members; payout runs.
- **Why it wins:** Two-sided money (collect and pay out) for a Bengaluru-native user, with `order_tags` used for real reconciliation instead of a spreadsheet.
- **Feasibility:** 3/5. First test whether sandbox payments produce recon rows; keep fixture data if not.
- **Risk and guardrails:** Defaulter naming is a privacy risk (DPDP is not researched here). Show amounts only, not names, in group views; sandbox only.

## 5. Paisa Wapas Referee (Kannada) - "debited but the order failed"

- **Pitch:** A customer of a small online shop reports a debited-but-failed payment. The agent traces the order, explains the cause in Kannada, and either retries or raises a refund the merchant approves.
- **User and message:** Customer. *"Nanna account-inda haNa cut aagide, aadare order aagilla. Nanna duddu vaapas kodi."*
- **How money moves:** PG Get Order and Get Payments, the UPI failure matrix (`testinsufficientfunds@gocash`, `testexpired@gocash`, `testbankdeclined@gocash` and others) to explain the cause, `create_refund` with an `x-idempotency-key`, and `simulate_payment` through the MCP server. Sandbox refund semantics are undocumented.
- **Sarvam:** Saaras v4, 105B classification (success, failed, pending), Bulbul v4 Flash Kannada.
- **Demo (3 steps):** (1) Kannada complaint. (2) Agent finds the payment and explains why it failed. (3) Merchant approves; refund issues; agent confirms.
- **Why it wins:** Honest caveat: lowest novelty in the set. Cashfree already ships Resolve (a call within a minute of a failed payment) and Relay (approval before refunds). The only differentiator is customer-initiated and in Kannada.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Judges will compare it to Cashfree's own products. Merchant approval before any refund; mock refund adapter if sandbox refunds misbehave. Treat as the safe fallback, not the headline.

---

## Ranking within this theme (our inference, /25)

| Rank | Idea | Total |
|---|---|---|
| 1 | Bank-Badal Guard | 21 |
| 2 | RWA Society Desk | 20 |
| 3 | SHG Hisaab | 20 |
| 4 | Salary Saathi | 20 |
| 5 | Paisa Wapas Referee | 18 |
