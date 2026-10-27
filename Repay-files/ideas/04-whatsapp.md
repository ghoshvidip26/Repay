# Theme 4: WhatsApp - five winning ideas

Brief: voice, chat, documents or WhatsApp. WhatsApp is where small businesses already chase money, so these ideas live in a chat thread: a voice note or text comes in, the agent replies in the same language, and money moves.

Sandbox only. Scores are our inference. Cashfree and Sarvam facts come from the notes in `../research/`.

## Shared WhatsApp rules and constraints

- Sarvam's own managed WhatsApp and text agents are **enterprise-only on request**. For the buildathon, use the Sarvam cookbook's WhatsApp bot recipe, or Twilio or the Meta Cloud API, and keep a web chat simulator as a fallback in case the WhatsApp channel is not approved in time.
- Cashfree Payment Links document SMS and email delivery (`link_notify`); WhatsApp delivery appears only in earlier notes and is unverified. The agent should paste the link into its own WhatsApp message.
- UPI payment links are capped at Rs 1 lakh, cannot take partial payments, and auto-reminders exist only for UPI links.
- Voice notes go through Saaras v4 (REST, 30-second real-time cap) and replies can be Bulbul v4 Flash audio. Run a per-language test set of number words before the day.
- Reminder messages must stay polite and opt-in. The RBI recovery rules the notes found are a draft that binds lenders, and a TRAI claim on AI calls came from one vendor guide; neither was verified, so treat any outbound voice calls as out of scope and send messages only.

---

## 1. Udhaar Vaani Loop (Tamil) - a credit-sale ledger that chases, reconciles, then pays your supplier

- **Pitch:** A trader sends a voice note logging a credit sale. The agent sends escalating reminders in the debtor's language with a UPI link, matches partial payments to the open ledger entry, and when cash lands, pays the trader's own supplier after verification.
- **User and message:** Tamil-speaking trader. A voice note *"Ravi-kitta munnooru roobai udhaar, aduthu vaaram vara sollunga."* (Rs 300 credit to Ravi; ask him to pay next week.)
- **How money moves:** Payment Links per entry (a second link for any remaining balance, because UPI links reject partial payments), link webhooks to reconcile, Settlement Reconciliation API and `order_tags` to match settlements, then Payouts V2 to a supplier verified by penny drop. A single paid-in step feeds a paid-out step.
- **Sarvam:** Saaras v4 (voice notes, codemix), 105B JSON schema for the ledger entry, Bulbul v4 Flash and Mayura to write each reminder in the debtor's language.
- **Demo (3 steps):** (1) Voice note logs two credit sales. (2) A reminder with a UPI link goes to one debtor; a partial test payment arrives and the agent matches it and issues a link for the balance. (3) When the entry is cleared, the agent pays the supplier after a tap and reads the outcome back.
- **Why it wins:** Reminders and links alone are crowded (OkCredit, Razorpay RAY, RazorpayX agents, Cashfree Relay). This closes the loop in a regional language for traders without a gateway account, and ends with money moving out, not just in.
- **Feasibility:** 3/5. Confirm early whether sandbox payments produce recon rows; fall back to fixtures.
- **Risk and guardrails:** Reminders can harm relationships. Opt-in, rate-limited, polite templates; no calls; no debtor data beyond name and amount; sandbox only.

## 2. Tuition Fee Munshi (Hindi) - instalment fees in, teacher payouts out

- **Pitch:** A tuition centre owner voice-notes the fee plan. The agent sends each parent an instalment link on WhatsApp, reconciles, reminds in Hindi, and pays the teachers when the fees clear.
- **User and message:** Centre owner. *"Is mahine ki fees paanch hazaar, do kisht mein - sabko link bhej do aur teachers ko pay kar dena jab paisa aa jaye."*
- **How money moves:** One Payment Link per instalment (a separate link each, since UPI links reject partial payments), link webhooks and the Settlement Reconciliation API to match, then Payouts V2 to teachers (pre-verified) after the owner taps approve.
- **Sarvam:** Saaras v4 Hindi, 105B tool use, Bulbul v4 Flash or Mayura for reminder wording.
- **Demo (3 steps):** (1) Owner sets a plan for three students. (2) Two parents pay; one is reminded and pays the second instalment. (3) Fees cleared; teacher payout runs after approval.
- **Why it wins:** A simple, relatable two-way flow that demos cleanly on stage. It is the least differentiated idea in this theme, so the clean demo matters.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Children's data (DPDP not researched). Store parent name and amount only; no student records in the demo.

## 3. Puja Committee Kitty (Bengali) - community collections and vendor payouts

- **Pitch:** A Bengali community or Durga Puja committee collects contributions on WhatsApp, tracks pledged against paid, and pays pandal, decoration and catering vendors only after two committee approvals.
- **User and message:** Committee treasurer. *"Ei saal chanda target dosh hajar - jara dei ni tader link pathao; decorator-ke tin hajar dao."* (Annual contribution target Rs 10,000... send links to those who have not paid; pay the decorator Rs 3,000.)
- **How money moves:** Payment Links for contributions, reconciliation from webhooks, Payouts V2 to vendors verified by penny drop, with a two-approver rule and a total spending cap.
- **Sarvam:** Saaras v4 Bengali, 105B JSON schema, Bulbul v4 Flash Bengali summary, Mayura for a Hindi or English version for non-Bengali members.
- **Demo (3 steps):** (1) Treasurer sets a target and sends links. (2) Pledged-versus-paid summary read out in Bengali. (3) Two members approve a vendor payout; the payout succeeds, and a reversed run shows the agent explaining it.
- **Why it wins:** Community and festival money is a distinct, culturally specific use case with built-in governance (two approvers), and it uses Bengali, which most demos skip.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Collections by informal groups have regulatory edges the notes did not research. Frame it as a committee ledger with caps; sandbox only.

## 4. PG Rent and Deposit Desk (Kannada) - rent in, deposit refund out with deductions

- **Pitch:** A paying-guest (PG) owner voice-notes the rent roll. The agent sends monthly links, reconciles, pays the cook and electricity, and at move-out computes the security-deposit refund with itemised deductions and pays it.
- **User and message:** PG owner in Bengaluru. *"Ee tingalina rent link ellarigu kalisu; Ravi-ge move-out - deposit-inda eraDu saavira kaTTu maadi bere hana kaLisu."* (Send this month's rent links; Ravi is moving out, deduct Rs 2,000 from the deposit and refund the rest.)
- **How money moves:** Payment Links for rent, recon via `order_tags` per room, then Payouts V2 for the deposit refund (verified by penny drop) and for vendors, with approval. Code computes the refund.
- **Sarvam:** Saaras v4 Kannada, 105B JSON schema for deductions, Bulbul v4 Flash Kannada to explain the deductions.
- **Demo (3 steps):** (1) Rent links go to three tenants; two pay. (2) A tenant moves out; the agent itemises deductions and reads them back. (3) Owner approves; the refund payout runs.
- **Why it wins:** A Bengaluru-native problem; the payout step (a deposit refund with explained deductions) is concrete and emotional, and the refund explanation is where Sarvam's language models add value.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Disputed deductions. Show every deduction line; the tenant must confirm the refund account; sandbox only.

## 5. Netting Desk (Hindi) - two owe each other, move only the difference

- **Pitch:** Two small businesses that owe each other money (for example a restaurant and its vegetable vendor) each voice-note their side on WhatsApp. The agent nets the two amounts, shows both parties the same sum, and moves only the difference.
- **User and message:** Restaurant owner. *"Sabzi wale ko do hazaar teen sau chalis dene hain, usne mujhse paanch sau advance liya tha - baaki bhej do."* (I owe the vendor Rs 2,340; he took Rs 500 advance from me; send the rest.)
- **How money moves:** Penny drop on the vendor's UPI, then Payouts V2 UPI transfer of the net amount (code computes Rs 1,840). If the net runs the other way, a Payment Link is sent to the other party instead.
- **Sarvam:** Saaras v4 Hindi (both parties' notes), 105B JSON schema for the two sides, Bulbul v4 Flash Hindi readback to both.
- **Demo (3 steps):** (1) Each party sends a voice note with their side. (2) Agent shows one netted figure to both and waits for both taps. (3) Net payout runs; both ledgers close.
- **Why it wins:** A distinctive mechanic (bilateral netting) that none of the crowded lanes cover, and it moves less money more safely.
- **Feasibility:** 4/5.
- **Risk and guardrails:** Both parties must agree before any money moves; no payout without both taps; amounts read back in words; sandbox only.

---

## Ranking within this theme (our inference, /25)

| Rank | Idea | Total |
|---|---|---|
| 1 | Udhaar Vaani Loop | 22 |
| 2 | Netting Desk | 20 |
| 3 | Puja Committee Kitty | 19 |
| 4 | PG Rent and Deposit Desk | 19 |
| 5 | Tuition Fee Munshi | 17 |
