# Move JanSaarthi From Links to Payouts

JanSaarthi AI satisfies the brief's letter but not its spirit. It is a competent, safe, voice-in/voice-out payment-link agent in Kannada, but it is built around the one flow the prior-art notes mark as crowded (WhatsApp/payment-link collection), it leaves Cashfree's most distinctive money-movement surface (Payouts V2 with deterministic failure states) as an "optional" extra, bolts on a risky Aadhaar/public-services story, and plans a **4-hour** build for an event that runs **11:30 AM to 7 PM (about 7.5 hours including the demo)**. Our score is **5.5/10 as written**, rising to roughly 8/10 if it keeps its validate-confirm-persist-execute architecture, cuts the government-ID layer, and swaps the headline flow from "collect Rs 1,200" to "verify, approve, pay out, and recover when the transfer fails." The report then proposes ten distinct, payout-first ideas for underserved Hindi, Tamil, Kannada and Bengali users; the best single bet is **Raitha Payout**, a Kannada farmer-producer payout desk with beneficiary verification, batch approval and live failure recovery. Every Cashfree and Sarvam capability cited comes from the research notes and is sandbox-only. The judging criteria are unpublished, the meaning of "500 Sarvam credits" is undocumented, and Cashfree's Agentic Payments has no technical docs, so rankings below are labelled inference.

## JanSaarthi meets the logistics but is weakest on the brief's headline phrase

The brief's hook is "Agents that move real money... It hears what someone needs in Hindi, Tamil, Kannada or Bengali, and then actually moves the money. Demo it live by evening." JanSaarthi's loop (speak, extract JSON intent, confirm, create a Cashfree payment link, verify by webhook, answer in the user's language) is architecturally sound, and its rule that "the model must never be allowed to invent an amount, bypass confirmation, or call arbitrary payment APIs" matches what the Sarvam docs recommend (parse `function.arguments`, validate with a schema before acting) ([Sarvam chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview)). The table maps each keyword to the document. "Status" is our judgment.

| Brief keyword | How JanSaarthi handles it | Status |
|---|---|---|
| 150 builders | Not addressed. Nothing in the plan anticipates competing with 29 other teams that will each have seen the same sponsor demos. | Gap |
| One day | Plans **4 hours** (0 to 240 min). The event is 11:30 to 19:00, about **7.5 hours gross**, so the plan leaves roughly half the available time unplanned (see the timeline note below). | Mismatch |
| Agents that move real money | Collection via payment link: the agent only creates a link and a customer must pay it. Payouts (the agent itself sending money) are "optional advanced feature." | Partial |
| Sarvam Indian-language models | Saaras v4, Sarvam-105B, Bulbul v4 Flash, Sarvam Vision (stretch). Correct, current model names ([Sarvam models](https://docs.sarvam.ai/api/getting-started/models)). | Meets |
| Cashfree payment APIs | One product: payment links plus webhook verification. The doc itself advises "one Cashfree product deeply." Payouts and Secure ID are named but unplanned. | Partial |
| Hears what someone needs | Voice plus text fallback, JSON intent schema. The "need" is one narrow intent (collect Rs 1,200). | Meets, narrow |
| Hindi, Tamil, Kannada, Bengali | Names all four, demos only Kannada. No per-language number-word test set (lakh, "dedh", "dhai" and Tamil/Kannada/Bengali equivalents) is planned. | Partial |
| Actually moves the money | In sandbox the "customer" must complete test checkout by hand; the agent never debits or credits anyone itself. A payout with a webhook-confirmed status is the stronger proof. | Partial |
| Demo live by evening | Demo flow is clear and shows paid versus pending honestly. Failure paths exist only in prose. | Meets, thin |
| Discord | Not mentioned at all, though the brief says "who is selected has a lot to do with who shows up there." The doc also refers to a "Cashfree Showcase channel," which the research could not confirm exists. | Gap |
| Voice, chat, documents or WhatsApp | All four are designed in, voice and chat as must-haves; documents and WhatsApp as stretch. A reasonable scoping call. | Meets |
| 500 Sarvam credits | Not budgeted or mentioned. | Gap |
| 30 teams of 5 | Assumes a pre-formed team of five with named roles. The brief says to "find your team" on Discord, so the roster may not be fixed (inference). | Risk |

The honest critique has five parts. First, **collection is the crowded lane**: the prior-art notes list "WhatsApp payment-link bot" and "generic agent with spending limit/mandate" as crowded because WhatsApp has had native payments since 2023, payment-link APIs are a commodity, and NPCI and Pine Labs are standardising agent mandates ([Pine Labs P3P, The Paypers](https://thepaypers.com/payments/news/pine-labs-launches-p3p-agentic-payment-protocol-on-upi)). That crowding is the notes' inference rather than a counted survey of submissions, but the sponsors' own tracks (Voice, Document, Multilingual, Agents) point the same way ([Sarvam Epoch](https://www.startupgrantsindia.com/sarvam-epoch-buildathon-by-growthx)). Second, **Cashfree already ships the tooling a "create link, check status, refund" agent would be built from**: a hosted MCP server with `create_payment_link`, `create_order` and `simulate_payment` ([MCP docs](https://www.cashfree.com/docs/tools-ai/mcp-server.md)), the Agent Toolkit ([docs](https://www.cashfree.com/docs/tools-ai/cashfree-agent-toolkit.md)), Cashfree HERE in-chat payment widgets ([docs](https://www.cashfree.com/docs/tools-ai/cashfree-here.md)), and Relay and Spark dashboard agents with human approval ([GFF launches](https://www.cashfree.com/blog/gff-product-launches-2026/)). Cashfree judges have seen "agent makes a link." Third, **"money moves" is stronger with Payouts**. Payouts V2 has deterministic magic accounts: success, invalid IFSC, pending-then-success, pending-then-failure, success-then-reversed and a 25-second timeout ([payouts test data](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md)), so an agent can visibly handle reversal and recovery, which a payment link cannot show.

Fourth, **the public-services and Aadhaar layer is a distraction and a risk**. The document itself concedes that automatic record changes need department integrations and approval, so on the day it is slideware. Secure ID's mocks cover Aadhaar number validity and masking but DigiLocker document-fetch and offline-KYC mocks are undocumented ([VRS test data](https://www.cashfree.com/docs/api-reference/vrs/data-to-test-integration.md)), and showing identity documents invites DPDP-style exposure (the DPDP angle was not researched in the notes; this is inference). Fifth, **the document contradicts its own calendar.** It promises "a focused product to demonstrate in four hours" with a 0 to 240 minute plan, but the brief gives 11:30 to 7 PM. The correction: plan for about 7.5 hours gross and **roughly 5.5 to 6.5 hours of real build time** once Discord team introductions, setup and the demo slot are subtracted (inference; the day's agenda is unpublished). The notes' own estimate of a "realistic 6-hour scope" supports this. That spare time is exactly what a payout-plus-recovery flow needs.

On a 10-point scale (our inference, not a sponsor rubric): brief fit 7, strength of the money-movement proof 5, uniqueness 3, feasibility 7, risk hygiene 6 (good confirmation design, bad Aadhaar exposure), plan realism 4. **Keep:** the validate-confirm-persist-execute pipeline, the JSON intent schema, honest paid/pending/failed language, the text fallback, webhook-verified status (never a browser redirect), and the stated limits on autonomy. **Cut:** Aadhaar/PAN/DL/Ayushman propagation, "civic-tech vision" slides, the four-hour timeline, and unspecific WhatsApp ambitions. **Change:** make Payouts V2 plus Secure ID verification the headline, demo at least one failure and recovery on stage, add the other three languages to a test set, budget the 500 credits and Sarvam rate limits, and start on Discord now.

## Ten payout-first ideas that sidestep the crowded list

Every idea below is built on the same trust layer, because the notes show where Indic voice agents can fail on money. Saaras v4 normalises spoken numbers in transcribe mode but keeps spoken digits in `verbatim` mode and preserves English words in `codemix` mode, so the agent can transcribe twice and refuse to proceed if the amounts disagree; keyterm prompting (up to 50 names, REST and batch only) biases payee names ([Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)). Bulbul v4 Flash covers Hindi, Bengali, Kannada and Tamil, has no SSML, and degrades on romanised input, so every readback is written in native script with amounts spelled as words ([Bulbul](https://docs.sarvam.ai/api/getting-started/models/bulbul)). `sarvam-105b-conversations` (32K context, real-time dialogue) should run with reasoning disabled, because thinking mode is on by default and burns tokens and latency ([Sarvam-105B](https://docs.sarvam.ai/api/getting-started/models/sarvam-105b)). The LLM only emits a JSON-schema intent; code parses numerals and computes totals. A human confirms by tap, not by voice, since the notes found no source supporting voice as an authentication factor and Hello UPI still takes the PIN manually ([Outlook Money](https://www.outlookmoney.com/banking/ask-and-pay-your-upi-will-work-like-alexa-soon)). Every mutating call carries an idempotency key or unique `transfer_id`; after any 5XX on a transfer the agent checks status instead of re-initiating ([Payouts V2](https://www.cashfree.com/docs/api-reference/payouts/v2/payouts-api-v2-new.md)); webhooks are HMAC-verified on the raw body and deduplicated by business ID ([webhook reliability](https://www.cashfree.com/docs/api-reference/webhooks/reliability-and-operations.md)). Everything runs on test keys only. Payouts are available only to registered businesses, so each scenario assumes the operator is a licensed or registered entity in a real deployment, which is outside what a sandbox demo proves (inference).

Selection logic: the one 2026 Sarvam hackathon result found rewarded underserved-user problems with working prototypes over another payments bot (1st place was MSME tender discovery on Sarvam Vision) ([Digital Terminal](https://digitalterminal.in/tech-companies/zo-house-bengaluru-hosts-sarvam-ai-hackathon)). That is one data point, not a rubric. Segment sizes in the notes are mostly unverified hypotheses, so the ideas argue from workflow pain, not market size. Utterances are illustrative and need a native-speaker check before the demo (inference). Feasibility is rated 1 to 5 for about 6 build hours with 5 people.

### 1. Raitha Payout (Kannada) - farmer-producer mandi payout desk

| Field | Detail |
|---|---|
| One-line pitch | An FPO secretary dictates the day's purchases, the agent verifies every farmer's bank account, gets one approval, pays the whole batch, and explains in Kannada which payments are done, pending or reversed. |
| User and utterance | Kannada-speaking FPO or mandi secretary. "ರಾಮಣ್ಣನಿಗೆ ಹದಿನೆಂಟು ಕ್ವಿಂಟಾಲ್ ರಾಗಿ, ಕ್ವಿಂಟಾಲ್‌ಗೆ ಮೂರು ಸಾವಿರದ ಮುನ್ನೂರು ರೂಪಾಯಿ - ಹಣ ಹಾಕಿ." (Ramannanige hadinentu kvintal ragi, kvintalge mooru saavirada munnooru rupayi - hana haaki: "Pay Ramanna for 18 quintals of ragi at Rs 3,300 per quintal.") Code computes Rs 59,400. |
| How money moves | Secure ID bank account verification plus name match gates each farmer (score thresholds: 100 direct, 85 good, 70 moderate, 38 poor on the mock) ([VRS test data](https://www.cashfree.com/docs/api-reference/vrs/data-to-test-integration.md)). Then Payouts V2 Batch Transfer (bank-only) with the optional "Approve Batch" step, and Get Batch Transfer Status ([Payouts FAQ](https://www.cashfree.com/docs/payouts/payouts/general-faqs.md)). Magic accounts give one success, one invalid IFSC (`1234567890`/ICIC0000001), one pending-then-failure (`7766666351000`/YESB0000001) and one 25-second timeout (`34978321547298`/KKBK0000001). IMPS handles up to Rs 5 lakh ([payout methods](https://www.cashfree.com/docs/payouts/payouts/make-payouts/payout-methods.md)). |
| Sarvam models | Saaras v4 (codemix plus keyterms for farmer names), sarvam-105b-conversations with JSON-schema intent, Bulbul v4 Flash Kannada summary. Sarvam Vision optional for a typed or printed weighbridge slip (handwriting accuracy is unverified in the notes). |
| Channel | Voice primary, document (slip photo) stretch. |
| Live demo, 3 steps | 1) Secretary dictates three purchases in Kannada; agent reads back the total in Kannada words. 2) Verification table shows one farmer flagged (bad IFSC) and removed; secretary taps Approve. 3) Batch runs: two succeed, one goes pending then fails; the agent says so in Kannada and offers a corrected-IFSC retry, never a blind re-send. |
| Why unique | Prior art on Sarvam plus payments is food, grocery and gold ordering ([Razorpay newsroom](https://newsroom.razorpay.in/newsroom/razorpay-partners-with-sarvam-to-power-voice-first-conversational-commerce-for-india/)); a producer-side batch payout with recovery is absent, and Cashfree ships no vernacular payout agent (the notes' Relay offers approvals but not a farmer workflow). |
| Feasibility | 4/5. Batch approval is "enabled on request" and Secure ID features must be enabled on TEST, so request early. |
| Main risk and guardrails | Enablement delays; a CGAP-era segment pain (informal channels) is 15 years old ([CGAP](https://www.cgap.org/blog/how-do-migrant-workers-move-money-in-india)). Guardrails: sandbox only, readback of each amount and total in Kannada words, tap-to-approve with a per-batch cap, no re-initiation after 5XX, payee only from a pre-registered farmer list, mock adapter if batch is not enabled. |

### 2. Ghar Bhejo (Bengali) - verified remittance home

| Field | Detail |
|---|---|
| One-line pitch | A Bengali-speaking migrant worker in Bengaluru asks to send money to his mother; the agent verifies the account name, reads it back in Bengali, sends, and shows what happens if the transfer reverses. |
| User and utterance | Migrant worker. "মুর্শিদাবাদে মাকে আজই দুই হাজার টাকা পাঠিয়ে দাও।" (Murshidabade make ajoi dui hajar taka pathiye dao: "Send Rs 2,000 to Mother in Murshidabad today.") |
| How money moves | Pre-registered family payee, then Secure ID bank verification and Name Match (`026291800001191` "JOHN DOE" vs "john d" scores 85); Payouts V2 Standard Transfer via IMPS. Sandbox accounts: success `026291800001191`/YESB0000262, pending-then-success `00224412311300`, success-then-reversed `7766671735000`/SBIN0000004 ([payouts test data](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md)). Webhooks TRANSFER_SUCCESS and TRANSFER_REVERSED ([payout webhooks](https://www.cashfree.com/docs/payouts/payouts/make-payouts/webhooks.md)). The same account number works in Secure ID and Payouts, so the demo chain is coherent. |
| Sarvam models | Saaras v4 (Bengali, `bn-IN`, codemix), sarvam-105b-conversations tool call, Bulbul v4 Flash Bengali, Mayura to render a short Hindi or English note if the receiver needs one. |
| Channel | Voice, chat; WhatsApp note to receiver as stretch. |
| Live demo, 3 steps | 1) Bengali request; agent matches "Ma" to the saved payee. 2) Name-match score appears; a "j doe" variant (70) triggers an extra confirmation, while a poor match (38) blocks. 3) Transfer succeeds; the reversed-account run shows the agent explaining in Bengali that a bank reversal returned the money. |
| Why unique | The notes' CGAP evidence shows the structural pain (57% of surveyed migrants used informal channels, nearly half of hawala users would avoid it) but it is 2011 data. The incumbent voice products are generic UPI apps; none was found that verifies the payee and narrates reversals. |
| Feasibility | 4/5. |
| Main risk and guardrails | Real-world remittance is regulated (RBI/NPCI rules unresearched in the notes); never imply the sandbox proves licensing. Guardrails: sandbox only, UPI-Circle-style caps shown as illustrative, cooling-off for first-time payees, readback of amount and payee name in Bengali script, human tap before execution. |

### 3. Dadu Guard (Bengali) - scam-check then family-approved payment for seniors

| Field | Detail |
|---|---|
| One-line pitch | A senior forwards a QR or payment request; the agent checks who the payee really is, explains in Bengali whether it is safe, and only pays after a family member approves. |
| User and utterance | Senior citizen, with a daughter as approver. "এই QR কোডটা কার? এখানে পাঁচ হাজার টাকা দেওয়া নিরাপদ কি?" (Ei QR code-ta kar? Ekhane panch hajar taka deoya nirapod ki?: "Whose QR is this? Is it safe to pay Rs 5,000 here?") |
| How money moves | UPI Penny Drop (valid `valid@ybl`, invalid `invalid@ybl`, expired `expired@ybl`) and Name Match decide a verdict; if safe and approved by the family approver, Payouts V2 UPI transfer to `success@upi` (`failure@upi` for the failure run) ([VRS test data](https://www.cashfree.com/docs/api-reference/vrs/data-to-test-integration.md), [payouts test data](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md)). UPI payouts cap at Rs 1 lakh and bank limits vary ([payout methods](https://www.cashfree.com/docs/payouts/payouts/make-payouts/payout-methods.md)). |
| Sarvam models | Sarvam Vision to read the forwarded screenshot text (10 requests/min limit) ([Vision](https://docs.sarvam.ai/api/getting-started/models/sarvam-vision)); QR payload decoded in code with a standard decoder (inference, not from the notes); Saaras v4 for the voice question; Bulbul v4 Flash Bengali; JSON-schema verdict from the 105B model. |
| Channel | Documents (screenshot) plus voice, delivered through a chat UI; WhatsApp forwarding is stretch. |
| Live demo, 3 steps | 1) Senior speaks and uploads a QR image. 2) Agent shows "payee name does not match the claimed shop" (name match 38) and says so in Bengali. 3) A second, legitimate QR passes; a push notification summary goes to the daughter, who taps Approve; payout runs. |
| Why unique | UPI fraud still runs 10.64 to 13.42 lakh incidents a year (FY26 to Nov 2025 and FY24) ([IANS](https://ianslive.in/upi-frauds-worth-rs-805-crore-witnessed-this-fiscal-so-far-minister--20251215175703)), yet incumbents' voice work targets payment initiation, not pre-payment verification plus delegated approval. NPCI's UPI Help assistant answers queries and manages mandates but does not verify a specific payee on the senior's behalf ([Angel One](https://www.angelone.in/news/personal-finance/npci-launches-ai-powered-upi-help-to-assist-users-with-digital-payments)). |
| Feasibility | 3/5 (three channels, two approvers). |
| Main risk and guardrails | Penny-drop name match can be wrong; the agent must say "unverified" rather than "safe." Guardrails: verdict is advisory, approver taps (not voice), amount cap, sandbox-only, no real QR data of real people (DPDP), note that UPI Circle's real limits are unverified. |

### 4. Dihadi Desk (Hindi) - daily-wage payouts to workers with no bank details

| Field | Detail |
|---|---|
| One-line pitch | A building contractor dictates the day's wages for twelve workers; workers who have UPI are paid, and the rest receive a payout link to supply details themselves. |
| User and utterance | Hindi-speaking contractor. "आज बारह मज़दूरों की दिहाड़ी, हर एक को पाँच सौ बीस रुपये - जिनका बैंक नहीं है उन्हें लिंक भेज दो।" (Aaj barah mazdooron ki dihaadi, har ek ko paanch sau bees rupaye... : 12 x Rs 520 = Rs 6,240.) |
| How money moves | V2 "Validate Payout" (UPI VPA or phone, returns a single-use transfer token) then "Process Validated Payout" ([Payouts V2](https://www.cashfree.com/docs/api-reference/payouts/v2/payouts-api-v2-new.md)); alternate beneficiary identification (phone plus name "John Snow" rows) for the no-bank-account path; Cashgram payout link for details collection if enabled (sandbox behaviour unstated) ([Cashgram](https://www.cashfree.com/docs/payouts/cashgram/cashgram.md)). Batch Transfer for the bank-account workers. |
| Sarvam models | Saaras v4 Hindi (`hi-IN`, codemix, numbers from "paanch sau bees"), 105B JSON schema with a list of workers, Bulbul v4 Flash Hindi. |
| Channel | Voice, chat; the worker link flow is WhatsApp/SMS as stretch. |
| Live demo, 3 steps | 1) One Hindi sentence yields a 12-row wage table with total Rs 6,240. 2) Agent separates "pay now" (validated UPI) from "needs link." 3) Contractor taps Approve; statuses stream in; one failure is retried on the corrected VPA. |
| Why unique | Informal-labour payouts to unbanked recipients are a Payouts workflow, not a payment-link one; contractor-side voice is absent from the incumbent demos (food, grocery, gold). |
| Feasibility | 3/5. Cashgram sandbox is unconfirmed, so build the mock link page behind the same interface. |
| Main risk and guardrails | Wrong worker matched by name; duplicate payment. Guardrails: payee list is per-site and pre-registered, idempotent `transfer_id` per worker per date, cap per worker, readback of total, sandbox-only. |

### 5. Salary Saathi (Hindi) - household payroll with advance deductions

| Field | Detail |
|---|---|
| One-line pitch | An employer says the monthly salary and the advance to recover; the agent verifies the worker's UPI ID, pays the net, keeps the advance ledger, and sends the worker a voice receipt in her language. |
| User and utterance | Employer paying a domestic worker. "सुनीता दीदी की इस महीने की तनख्वाह आठ हज़ार, पंद्रह सौ का एडवांस काटकर भेज दो।" (Sunita didi ki is mahine ki tankhwah aath hazaar, pandrah sau ka advance kaatkar bhej do: net Rs 6,500.) |
| How money moves | UPI Penny Drop (`valid@ybl`) then Payouts V2 UPI transfer (`success@upi`; `failure@upi` for the recovery run). Scheduling is agent-side cron plus Standard Transfer, because the notes found no scheduled-payout API ([Payouts](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md)). |
| Sarvam models | Saaras v4, 105B tool call (net computed in code), Bulbul v4 Flash voice receipt, Mayura if employer and worker languages differ. |
| Channel | Voice, chat; WhatsApp voice-note delivery as stretch (needs Twilio or Meta Cloud API, not Sarvam's enterprise-only managed WhatsApp). |
| Live demo, 3 steps | 1) Hindi request; agent reads back "Rs 8,000 minus Rs 1,500 equals Rs 6,500." 2) Employer approves; UPI payout succeeds. 3) The worker's receipt plays in Hindi and the remaining advance balance updates. |
| Why unique | Net-pay-with-advance logic plus a spoken receipt for a worker who "cannot read confirmations" is a ledger problem the voice-UPI incumbents do not solve. It is, however, the least differentiated payout idea. |
| Feasibility | 5/5. |
| Main risk and guardrails | A compelling but plain demo; a mis-heard "dedh" or "dhai" quantity. Guardrails: spell amounts as words, require confirmation of the net figure, single-payee cap, sandbox-only. |

### 6. Saath-Saath Split (Tamil) - seller-side settlement for informal collectives

| Field | Detail |
|---|---|
| One-line pitch | One customer order is automatically split among the members of a women-run tiffin collective by the percentages the leader speaks, with simulated vendor settlements and failure handling. |
| User and utterance | Collective leader. "இன்னைக்கு பொங்கல் ஆர்டர் மூவாயிரம் ரூபாய் - மீனாவுக்கு நாற்பது சதவீதம், கலாவுக்கு முப்பத்தைந்து, மீதி குழு நிதிக்கு." (Innaikku Pongal order moovayiram roobai - Meenavukku narpadhu sadhaveedham, Kalavukku muppathi-anju, meedhi kuzhu nidhikku: 40/35/25 of Rs 3,000.) |
| How money moves | Cashfree Easy Split: Enable Easy Split (async, poll for `ACTIVE`), create an order with `order_splits`, customer pays via UPI test VPA `testsuccess@gocash`, then Simulate Vendor Settlement to `INITIATED`, `SUCCESS` or `FAILED`; test vendor accounts include invalid IFSC and name-mismatch (Jon D vs John Doe). `VENDOR_SETTLEMENT_*` webhooks need webhook version `2025-01-01` ([Easy Split test data](https://www.cashfree.com/docs/payments/split/data-to-test.md)). |
| Sarvam models | Saaras v4 Tamil (`ta-IN`), 105B JSON schema with percentages that must sum to 100 (checked in code), Bulbul v4 Flash Tamil. |
| Channel | Voice, chat. |
| Live demo, 3 steps | 1) Tamil sentence builds a split table; agent reads each share in Tamil words. 2) Customer pays; the agent confirms the collection. 3) Settlement simulated: Meena succeeds, Kala fails (name mismatch); agent explains and queues a fix. |
| Why unique | Not generic bill-split (payer-side) but seller-side marketplace split for collectives, using a Cashfree product the research found no AI precedent for. Easy Split is the most Cashfree-specific feature in the notes. |
| Feasibility | 3/5. Enablement may land in `RISK_APPROVAL_PENDING` or `RISK_REJECTED`; reversals cannot be simulated. |
| Main risk and guardrails | Enablement failure kills the demo. Guardrails: ask for Easy Split enablement now, keep a recorded-response fallback, percentage-sum check, readback of rupee shares, sandbox-only. |

### 7. SHG Hisaab (Tamil) - self-help-group weekly meeting, collections and loan disbursement

| Field | Detail |
|---|---|
| One-line pitch | The group leader speaks the meeting's results; the agent sends dues links to members, reconciles who has paid, and releases an approved member loan by payout. |
| User and utterance | SHG leader. "இந்த வாரம் ஒவ்வொரு உறுப்பினரும் ஐம்பது ரூபாய் சேமிப்பு; ராணிக்கு இரண்டாயிரம் கடன் கொடு." (Indha vaaram ovvoru uruppinarum aimbadhu roobai semippu; Raanikku irandaayiram kadan kodu: Rs 50 savings from each; lend Rs 2,000 to Rani.) |
| How money moves | Payment Links for dues (partial payments allowed with a minimum, but not for UPI; expiry and `link_notify` available) ([Payment Links FAQ](https://www.cashfree.com/docs/help/payment-links/faqs/faqs.md)); reconciliation from payment-link webhooks; the loan goes out by Payouts V2 Standard Transfer after a two-person approval (leader plus treasurer, by tap). Subscriptions ON_DEMAND can model recurring savings charges (the periodic cycle cannot be forced in sandbox) ([Subscriptions FAQ](https://www.cashfree.com/docs/payments/subscription/faq.md)). |
| Sarvam models | Saaras v4 Tamil, 105B tool use, Bulbul v4 Flash, Sarvam Vision for a printed ledger photo (handwritten Tamil is unverified). |
| Channel | Voice plus document. |
| Live demo, 3 steps | 1) Tamil statement creates a dues table and sends links. 2) Test payments land, the agent says who has not paid. 3) Loan request needs two approvals; payout succeeds. |
| Why unique | Group finance with two-sided money movement (collect and disburse) and multi-approver governance; SHG sizes are unsourced in the notes, so the case is workflow-based. |
| Feasibility | 3/5 (scope creep is the danger; ship the payout half first). |
| Main risk and guardrails | Chit-fund and lending regulation is a grey area (notes flag compliance review). Guardrails: frame as a savings-and-internal-loan ledger, two-approver rule, cap per loan, sandbox-only. |

### 8. Udhaar AutoPay (Kannada) - consented khata recovery for kirana owners

| Field | Detail |
|---|---|
| One-line pitch | A kirana owner says who owes what; for customers who have previously consented to a capped on-demand mandate, the agent charges the dues, and otherwise falls back to a link. |
| User and utterance | Kirana owner. "ರಾಜು ಅವರ ಉದ್ರಿ ಐನೂರು ರೂಪಾಯಿ ಬಾಕಿ ಇದೆ - ಅವರ ಅನುಮತಿಯ ಮೇಲೆ ಕಟ್ ಮಾಡು." (Raju avara udri ainooru rupayi baaki ide - avara anumatiya mele cut maadu: "Raju owes Rs 500 on credit - charge it under his consent.") |
| How money moves | Cashfree Subscriptions ON_DEMAND: `POST /pg/subscriptions/pay` with `payment_type: CHARGE` can fire immediately in sandbox; a fallback Payment Link; refunds for disputes ([Subscriptions FAQ](https://www.cashfree.com/docs/payments/subscription/faq.md)). The notes flag UPI AutoPay as not self-serve, a 25-hour pre-debit notification gap, a Rs 15,000 cap without AFA, and no documented sandbox failure simulation. |
| Sarvam models | Saaras v4 Kannada, 105B JSON schema, Bulbul v4 Flash Kannada. |
| Channel | Voice, chat. |
| Live demo, 3 steps | 1) Kannada khata entry. 2) Agent finds a consented mandate and shows the capped charge. 3) On-demand charge executes; the unconsented customer gets a link instead. |
| Why unique | Khata recovery with explicit consent and caps rather than a plain link, using mandate primitives; the notes' kirana and udhaar segment rows are hypotheses with no sourced sizing. |
| Feasibility | 2/5. Mandate authorisation in sandbox and the 25-hour gap interact in ways the notes could not resolve. |
| Main risk and guardrails | Debiting a customer who did not consent. Guardrails: consent record shown on screen, per-charge cap, readback in Kannada, no AutoPay claim unless Cashfree enables it, sandbox-only. |

### 9. Supplier Safe-Pay (Tamil) - MSME invoice approval with bank-detail fraud check

| Field | Detail |
|---|---|
| One-line pitch | A small manufacturer photographs a supplier invoice; the agent extracts it, verifies GSTIN and bank account name, flags changed bank details, and pays only after approval. |
| User and utterance | Coimbatore-style MSME owner. "இந்த இன்வாய்ஸ் சப்ளையருக்கு பணம் செலுத்து, ஆனா வங்கி விவரம் சரியா இருக்கான்னு பார்த்துட்டு." (Indha invoice supplier-ukku panam seluthu, aana vangi vivaram sariya irukkannu paathuttu: "Pay this supplier's invoice, but check the bank details first.") |
| How money moves | Sarvam Vision extraction; Secure ID GSTIN (`29AAICP2912R1ZR` valid, `29AAIZP2912R1ZR` invalid), bank account verification and name match; mismatch against the vendor master holds the payment; approved invoices pay through Payouts V2 Standard Transfer ([VRS test data](https://www.cashfree.com/docs/api-reference/vrs/data-to-test-integration.md)). |
| Sarvam models | Sarvam Vision (PDF up to 10 pages, async job, 10 requests/min), Saaras v4 Tamil, 105B JSON schema, Bulbul v4 Flash. |
| Channel | Documents plus voice. |
| Live demo, 3 steps | 1) Invoice photo parsed; the Tamil summary is read aloud. 2) Verification flags that the account name or number differs from the vendor master. 3) Owner approves the corrected details; payout runs. |
| Why unique | An invoice reader alone is crowded; this is the verify-then-approve-then-pay gate, whose value is in catching the changed-bank-details pattern, not reading PDFs. |
| Feasibility | 4/5. |
| Main risk and guardrails | Reads as "just another invoice reader" if the gate is not the star. Guardrails: show the mismatch screen first, no payment without a passing GSTIN and name score, sandbox-only, no real GSTIN or PAN of real businesses. |

### 10. Paisa Wapas (Kannada) - "my money was debited but the order failed"

| Field | Detail |
|---|---|
| One-line pitch | A customer reports a debited-but-failed payment; the agent traces the order, explains the failure in Kannada, and either retries or raises a refund that the merchant approves. |
| User and utterance | Customer of a small online shop. "ನನ್ನ ಅಕೌಂಟ್‌ನಿಂದ ಹಣ ಕಟ್ ಆಗಿದೆ, ಆದರೆ ಆರ್ಡರ್ ಆಗಿಲ್ಲ. ನನ್ನ ದುಡ್ಡು ವಾಪಸ್ ಕೊಡಿ." (Nanna account-inda haNa cut aagide, aadare order aagilla. Nanna duddu vaapas kodi.) |
| How money moves | PG Get Order and Get Payments; the UPI failure matrix (`testinsufficientfunds@gocash`, `testexpired@gocash`, `testbankdeclined@gocash` and about 24 more) drives the explanation; `create_refund` with an `x-idempotency-key`; sandbox `simulate_payment` through the MCP server ([PG test data](https://www.cashfree.com/docs/api-reference/payments/data-to-test-integration.md), [MCP docs](https://www.cashfree.com/docs/tools-ai/mcp-server.md)). Sandbox refund semantics are undocumented. |
| Sarvam models | Saaras v4, 105B JSON-schema classification (success, failed, pending), Bulbul v4 Flash Kannada. |
| Channel | Voice, chat. |
| Live demo, 3 steps | 1) Kannada complaint. 2) Agent finds the payment and explains the cause. 3) Merchant approves; refund issues; agent confirms. |
| Why unique | Modest. Cashfree already ships Resolve (outbound AI voice within a minute of payment failure) and Relay (approval before refunds) ([GFF launches](https://www.cashfree.com/blog/gff-product-launches-2026/)), so judges will compare it to their own product. The differentiator is customer-initiated and in Kannada. |
| Feasibility | 4/5. |
| Main risk and guardrails | Low novelty; refund sandbox behaviour unknown. Guardrails: merchant approval before any refund, idempotency key, 429 backoff, mock refund adapter, sandbox-only. |

## Ranking: Raitha Payout is the best single bet, with a JanSaarthi hybrid as plan B

The ranking below sums five 1-to-5 scores that are **our inference, not a sponsor rubric**: how strongly the demo shows real money movement, uniqueness against the crowded list, how underserved the user is, feasibility for roughly six build hours, and demo impact. Ties break on feasibility, then on overlap with Cashfree's own products.

| Rank | Idea | Language | Money | Unique | Underserved | Feasible | Demo | Total /25 | Key Cashfree surface |
|---|---|---|---|---|---|---|---|---|---|
| 1 | Raitha Payout | Kannada | 5 | 5 | 5 | 4 | 4 | 23 | Batch Transfer, bank verification, approval |
| 2 | Ghar Bhejo | Bengali | 5 | 4 | 5 | 4 | 4 | 22 | Name Match, IMPS, reversal webhooks |
| 3 | Dadu Guard | Bengali | 4 | 5 | 5 | 3 | 5 | 22 | UPI Penny Drop, UPI payout |
| 4 | Dihadi Desk | Hindi | 5 | 4 | 5 | 3 | 4 | 21 | Validate Payout, Cashgram |
| 5 | Salary Saathi | Hindi | 5 | 3 | 4 | 5 | 3 | 20 | UPI payout, ledger |
| 6 | Saath-Saath Split | Tamil | 4 | 5 | 4 | 3 | 4 | 20 | Easy Split settlements |
| 7 | SHG Hisaab | Tamil | 4 | 4 | 5 | 3 | 4 | 20 | Payment Links, payouts |
| 8 | Udhaar AutoPay | Kannada | 4 | 4 | 5 | 2 | 4 | 19 | Subscriptions ON_DEMAND |
| 9 | Supplier Safe-Pay | Tamil | 5 | 3 | 3 | 4 | 3 | 18 | GSTIN, name match, payout |
| 10 | Paisa Wapas | Kannada | 3 | 3 | 4 | 4 | 4 | 18 | Get Payments, refunds |

**Recommended single bet: Raitha Payout.** It is the strongest on the brief's central phrase because the agent itself moves money out, in a batch, with a visible verify, approve, pay, recover arc that uses the sandbox's deterministic failure states, and it scores well on the underserved-user signal from the only Sarvam hackathon result in the notes. Kannada fits a Bengaluru venue (inference). Its main exposure is enablement of Secure ID and batch approval, so a recorded-response fallback must exist before the day. If Payouts is not enabled in time, fall back to Dadu Guard or Salary Saathi, whose UPI paths need fewer enablements. A cautious team could also build Ghar Bhejo first, since a single transfer is simpler than a batch.

**Hybrid if you want to keep JanSaarthi.** Keep the name, the Kannada opener and the confirm-before-execute architecture, but restructure the demo into a two-way ledger: a 30-second "collect Rs 1,200" payment-link moment (already built) followed by the headline "pay the farmer" flow from Raitha Payout, where the money collected is paid out after verification and approval, including one failed transfer and its recovery. The public-services vision stays as a single roadmap slide with a statement that no record changes are made. This keeps the document's work, adds Payouts and Secure ID as depth, and answers "money moves" in both directions.

## Pre-event checklist: selection happens before the day

1. **Start on Discord now.** The brief says "who is selected has a lot to do with who shows up there." Introduce yourself, post a 60-second recording of what you have built (for example Saaras to a sandbox payout), look for teammates, and ask engineers concrete questions (below). The Discord server specifics were not accessible to the research.
2. **Ask the Cashfree and Sarvam engineers:** what "500 Sarvam credits" means; whether sandbox accounts are shared or per team; which products will be pre-enabled; whether Payouts, Secure ID, Easy Split, Subscriptions ON_DEMAND, batch approval and Cashgram can be enabled on test keys; whether the hosted MCP server and Agentic Payments are available; and what the judging criteria are.
3. **Request Cashfree feature enablement early.** Payouts modes, Secure ID APIs, Easy Split, Batch links, TPV and UPI AutoPay must be enabled by Cashfree per account and are not instant; the notes found no turnaround time ([Payouts test data](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md), [Subscriptions FAQ](https://www.cashfree.com/docs/payments/subscription/faq.md)). PG keys do not work for Payouts or Secure ID, so create both key sets. Do not attempt production KYC (business PAN and about 24 working hours) ([activation](https://www.cashfree.com/docs/help/account/account-activation.md)).
4. **Sarvam credits and dates.** Registered builders get 500 credits; Sarvam's own signup grant is Rs 100 and credits are universal and never expire ([rate limits](https://docs.sarvam.ai/api/getting-started/ratelimits)). If 500 credits equal Rs 500 (unverified), a voice turn costing about Rs 0.4 to 0.9 allows several hundred turns, which is ample (inference). The Bulbul v4 Flash half-price promo ends 12 Nov, before the event ([Sarvam docs](https://docs.sarvam.ai)). Cache the demo's TTS phrases ahead of time.
5. **Sandbox only.** Use test keys and magic values; no real Aadhaar, PAN or bank data; no claim that an agent moves money without approval; never show unmasked identity numbers.
6. **Rate limits, especially on a shared account.** Sarvam is per account, not per key: 60 req/min for STT REST and 105B, 20 concurrent STT WebSockets, 10 req/min for Vision, and a ~300 ms spacing between WebSocket connects ([Sarvam rate limits](https://docs.sarvam.ai/api/getting-started/ratelimits)). Cashfree sandbox is 30/min for Create Order and Pay Order, and Secure ID standard APIs are 100 TPM ([PG limits](https://www.cashfree.com/docs/api-reference/payments/rate-limits.md), [VRS limits](https://www.cashfree.com/docs/api-reference/vrs/v2/verification-rate-limits.md)). If 30 teams share an account (unknown), these are shared too (inference): rehearse once, cache results, and do not loop-test during demos.
7. **Fallback mocks.** Put every Cashfree and Sarvam call behind an adapter with a recorded-response mode, a typed-chat fallback, pre-recorded audio, a webhook tunnel (the dashboard needs an HTTPS URL) and a headset with push-to-talk for a noisy hall. Build a per-language test set of number words before the day.

## What remains unverified

The notes could not confirm the following, and this report treats them as open: the **judging criteria** (no sponsor rubric was found; NPCI's DRUNIX lists only innovation, scalability and real-world impact) ([Internshala](https://internshala.com/competitions/drunix-hackathon-2026-by-npci-x-citi/)); the **meaning of "500 Sarvam credits"** and whether they raise rate limits; **Agentic Payments**, for which Cashfree published a press release and marketing but no technical docs, sandbox access or caps ([Cashfree newsroom](https://www.cashfree.com/news-room/cashfree-payments-becomes-one-of-the-first-fintechs-in-india-to-unveil-agentic-payments-bringing-end-to-end-ai-commerce-inside-chat/)); whether a **Cashfree Showcase** channel or any external Cashfree hackathon exists; the Discord server and the event agenda (only the 150 builders, the date and "build AI agents that move real money" were corroborated, by a search snippet of Cashfree's post); sandbox behaviour for **Cashgram, refunds, Reverse Penny Drop, DigiLocker, UPI QR/intent and ON_DEMAND mandate authorisation**; Payouts rate limits and minimum amounts; whether a sandbox payout wallet is pre-funded; and the time Cashfree takes to enable test features. Documentation conflicts remain too: the Agent Toolkit has 13 tools in the docs but "over thirty" in a blog, the hosted MCP lists PG-only tools while an overview says Payouts and Secure ID, and account `000100289877623` is "Success" in one page and "Failed at bank" in another ([Cashfree capabilities, MCP docs](https://www.cashfree.com/docs/tools-ai/mcp-server.md)). Sarvam has no published latency numbers, no third-party accuracy figures for amounts and names, and a vendor-only Tau2 tool-use claim ([Sarvam-105B](https://docs.sarvam.ai/api/getting-started/models/sarvam-105b)). The "crowded" list is the notes' inference from what incumbents ship, not a count of submissions. Segment sizes (kirana, SHG, farmers, domestic workers, seniors) are unsourced hypotheses; the sourced numbers are UPI at 24.07 billion transactions in September 2026 ([StartupTalky](https://startuptalky.com/upi-transactions-september-2026/)) and the 2011 CGAP remittance study. Regulatory limits (Reserve Pay Rs 10,000 over 90 days, UPI Circle Rs 5,000 per transaction and Rs 15,000 per month) come from press and secondary summaries and may change for agents ([MediaNama](https://www.medianama.com/2026/03/223-razorpay-sarvam-ai-ai-agent-payments-indus-app/), [Deccan Chronicle](https://www.deccanchronicle.com/amp/technology/india-preparing-framework-to-let-ai-agents-make-small-upi-payments-without-approval-1983986)). Finally, the utterances here are illustrative and unverified by a native speaker, and the scores are the author's judgment.

## Conclusion

The sharpest insight from the notes is that the brief's "actually moves the money" phrase is best answered where Cashfree's sandbox is most deterministic and least imitable: payouts that can pend, fail, time out or reverse, verified by Secure ID before and by signed webhooks after. A payment link ends when a customer clicks; a payout creates a story with a beginning, a failure and a recovery that can be told in 90 seconds in Kannada or Bengali. That, plus a visible approval step, also happens to be where the regulatory direction of travel (approval at the start of the journey, caps, audit trails) is heading, so it demonstrates the thing the incumbents have not.

The practical implication for the team is to treat the next 25 days as the competition's first half: Discord presence, feature-enablement requests and a one-minute proof-of-concept clip will matter as much as the build. JanSaarthi's best assets (the validate-confirm-persist-execute pipeline and its honest paid/pending/failed language) survive the pivot unchanged; what dies is the four-hour plan, the Aadhaar layer, and the assumption that a payment link is a sufficient answer to "move real money."
