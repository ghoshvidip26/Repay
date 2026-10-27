# Cashfree collections, settlement and reconciliation vs small-business cash-flow problems (state as of 27 Oct 2026, India)

Method: pages fetched 27 Oct 2026 from cashfree.com (docs, product pages, blog, pricing). Several fetches returned model-summarised text of the page, so figures are as reported by the fetch tool; re-verify numbers that matter. Earlier notes (webhooks, subscriptions, sandbox basics, MCP/Toolkit/HERE) are in /home/claude/research_notes/Sarvam Cashfree buildathon ideas/cashfree_capabilities.md and are only cross-referenced here.

## 1. Settlement speed and working capital

### Takeaway
Standard settlement is T+1 or T+2 working days (docs disagree on which is the default). Faster options are Instant Settlements (about 15 min, auto limit Rs 50,000, fee from 0.30% ex-GST), On-Demand Settlements (24x7 incl. holidays, small fee, needs account-manager activation) and a loan product, Cashfree Advances (repaid from PG collections; no amounts or rates public). All faster options depend on Cashfree enablement and risk review, so none is guaranteed in a buildathon sandbox.

### Cited Findings
- Standard cycles: T+1 (next business day) and T+2; weekends and bank holidays excluded; Cashfree follows the RBI Mumbai holiday list. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md)
- Default cycle is contradictory: the help page itself says "default cycle is T+2" yet also lists T+1 first; the manage-settlements page says "default T+2 bank working days, changeable on request via Support Form, T+1 also available"; the Instant Settlements product page says standard is T+2. Treat T+2 as default, T+1 as on request. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md), [Manage settlements](https://www.cashfree.com/docs/payments/manage/settlements), [Instant Settlements page](https://www.cashfree.com/instant-settlements/)
- Settlement amount = transaction value minus MDR and GST on MDR, adjusted for refunds, chargebacks. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md)
- Instant Settlements (docs): funds within about 15 minutes of capture; "nominal processing fee" (no figure in docs); needs fully onboarded KYC-verified merchant plus approval based on transaction history, business category, risk profile and bank approval; auto-activated with a Rs 50,000 settlement limit, higher limit requested from Dashboard > Settlements > Standard; usage shown under Payment Gateway > Settlements > Online. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md)
- Instant Settlements (marketing page): three types. Scheduled (multiple times a day, intervals from 15 minutes, including bank holidays), On-Demand (single-click withdrawal; can settle specific transactions), Same-Day (9 am and 5 pm). Pricing "starts at 0.30% excluding GST", zero setup and maintenance fee, varies by settlement type and risk, holiday/non-banking-hour settlements may cost more. Daily limit set at merchant level (including vendor settlements for marketplaces); once reached, settlements revert to the standard cycle until reset next day; numeric limit not published. Enablement contact channel is WhatsApp. Funds can go to a bank account or a Cashfree virtual account (to pay vendors, refunds, loan disbursal). Only successful transactions settle. — [Instant Settlements page](https://www.cashfree.com/instant-settlements/)
- Conflict on limit vs speed: docs say Rs 50,000 auto limit "until the next working day"; marketing page gives no number and mentions "within 30 minutes on request" in FAQ vs "15 minutes" elsewhere. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md) vs [Instant Settlements page](https://www.cashfree.com/instant-settlements/)
- On-Demand Settlements: any day 24x7 incl. holidays, any amount up to the withdrawable balance, small fee shown on confirmation screen; must click Request Activation (Dashboard > Settlements > On-Demand), account manager enables, email on activation; then Settle Now. No API for merchant on-demand is documented on the pages fetched. Older docs describe it as "instantly". — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md), [older On-Demand page](https://docs.cashfree.com/docs/on-demand-settlement)
- Early settlement as a separate product: not found; the Instant Settlements page treats Instant Settlements as the early-access feature. — [Instant Settlements page](https://www.cashfree.com/instant-settlements/)
- Cut-off times: no cut-off published beyond the Same-Day 9 am / 5 pm slots. — [Instant Settlements page](https://www.cashfree.com/instant-settlements/)
- Bank credit after SETTLEMENT_SUCCESS: IMPS 0-30 min, NEFT 30 min-2 h, RTGS same day usually within 2 h; no real-time bank callback, confirm by UTR. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md)
- Failed settlements retry next cycle only if the merchant had at least one successful transaction in that cycle; reversal reasons include invalid IFSC/name, inactive or NRE account, transfer limit exceeded. — [Manage settlements](https://www.cashfree.com/docs/payments/manage/settlements)
- Marketplace (Easy Split) vendor on-demand: dashboard or `create-on-demand-transfer` API; RTGS/IMPS; "On Demand Service Charge" (amount unstated); vendor balance must be >= Rs 1,000; transactions must be at least 15 min old for API, 1 h for dashboard; sandbox not mentioned. — [Easy Split on-demand](https://www.cashfree.com/docs/payments/split/settlements/on-demand)
- Cashfree Advances (working capital loan): collateral-free, for businesses using Cashfree PG; "pre-approved loan limit"; repayment "automatically managed through your payment gateway transactions" / from daily collections (not explicitly "deducted from settlements"); 48-hour disbursal after approval; basic KYC, process via account manager; no amount, tenure, rate or lending partner stated. "Cashfree Capital" under that name was not found. — [Cashfree Advances](https://www.cashfree.com/working-capital-loan/)
- Pricing context: PG "starts at 1.95%" (page layout garbled; 2.0% also shown), Rs 4,999 annual maintenance charge, zero setup; no instant/on-demand fee figures on the pricing page. — [Pricing](https://www.cashfree.com/pricing/)
- Spark can "initiate instant settlements" from the dashboard with human sign-off; hosted MCP exposes settlement tools (standard, on-demand, vendor). — [Spark blog](https://www.cashfree.com/blog/introducing-spark-the-agentic-command-surface-for-your-business/), [MCP server docs](https://www.cashfree.com/docs/tools-ai/mcp-server.md)

### Inferences
- A cash-flow agent can tell a merchant "your Rs X is stuck until T+2; you could pull Rs Y now at ~0.30%+GST" but fee and eligibility must come from the account, not a public rate card.
- Because instant/on-demand need activation and a risk review, an agent should check availability and degrade gracefully.

### Gaps
- Exact on-demand fee, instant fee tiers by category, numeric limit raises, merchant-category exclusions: not published.
- No rolling reserve or hold policy found. A search for "Cashfree rolling reserve" returned only generic third-party articles (chargeback.io, Authorize.net), nothing from Cashfree.
- Settlement-linked financing terms (amount, rate, tenure, partner): not published; no merchant-cash-advance product found.
- Sandbox behaviour for instant/on-demand settlements: not documented (the settlements pages say nothing on sandbox).

## 2. Reducing delay in collections

### Takeaway
Cashfree offers hosted Payment Links and Payment Forms for ad-hoc dues, Auto Collect virtual accounts/UPI IDs for bank-transfer dues that self-match, Subscriptions (UPI AutoPay, eNACH, cards) for recurring dues, and an international gateway for overseas customers. Reminders exist only for UPI payment links, and a dedicated receivables dunning or invoice product was not found.

### Cited Findings
- Payment Links: "Send Auto Reminders" option for UPI payment links; partial payments with a minimum amount (not for UPI); expiry; SMS and email via `link_notify` (shortened cfre.in link); webhooks on complete, partial, cancelled, expired; UPI link cap Rs 1 lakh (use all payment methods for more); `link_id` max 50 chars; bulk Batch needs Support Form activation but bulk flow testable in sandbox; fees not covered. — [Payment Links FAQ](https://www.cashfree.com/docs/help/payment-links/faqs/faqs.md)
- Payment Links known issue: `link_status` can show EXPIRED after a successful payment; reconcile on order/transaction status. Each open-and-attempt creates a new Order; use Get Orders for a Payment Link API. Payment Link Transactions report includes settlement UTR. — [Payment Links FAQ](https://www.cashfree.com/docs/help/payment-links/faqs/faqs.md)
- WhatsApp as a link channel: not in this FAQ (it lists SMS and email only); earlier notes record WhatsApp sharing — contradiction/unverified on this page. — [Payment Links FAQ](https://www.cashfree.com/docs/help/payment-links/faqs/faqs.md)
- Payment Forms: hosted, 120+ methods, domestic and international, fixed or multiple amounts, validity 1/2/7/14/30 days or none, share by SMS/email/WhatsApp/website, webhook on payment, report per date range; custom fields only by creating orders yourself with `order_tags` (tags appear in the Settlement Recon Report); subscription forms with plans. Fees, sandbox, UPI QR, reminders, partial payments not covered. — [Payment Forms FAQ](https://www.cashfree.com/docs/help/payment-forms/faqs/faqs.md)
- Auto Collect (product page): unlimited virtual payment addresses and UPI QR codes, virtual bank accounts, real-time tracking and automated reconciliation, SMS/email via webhooks, REST API; virtual account is a pass-through to a linked bank account (funds not held), accepts UPI, IMPS, NEFT, RTGS and cheques, no KYC to create one; virtual UPI IDs work from any UPI app (Google Pay, WhatsApp). Pricing, limits, settlement timing not stated. — [Auto Collect page](https://cashfree.com/auto-e-collect)
- Auto Collect (legacy docs): one virtual account per customer or event; matching is automatic so payers need not give transaction details; modes NEFT/RTGS/IMPS/UPI; says payments to a virtual bank account via TPAP apps (GPay/PhonePe) are not accepted while also saying Google Pay payments work (that is the virtual UPI ID path). Sandbox, API detail, closure rules not covered. — [Legacy Auto Collect guide](https://docs.cashfree.com/docs/cac/guide)
- Virtual UPI IDs: Cashfree blog announcement exists (WhatsApp/Google Pay/BHIM collection) but was not fetched. — [Blog](https://www.cashfree.com/blog/cashfree-introduces-virtual-upi-ids-collect-payments-directly-via-whatsapp-google-pay-bhim/)
- Current-docs naming: the llms.txt index lists virtual accounts only under Payouts > OneEscrow (create/manage/delete virtual account), and no Auto Collect section — suggests Auto Collect docs have moved or been renamed; the modern docs path was not located. — [llms.txt via fetch](https://www.cashfree.com/docs/llms.txt)
- Subscriptions (credit-sale or recurring dues): see earlier notes for UPI AutoPay limits (Rs 15,000 without AFA, Rs 1 lakh with), eNACH up to Rs 1 crore, 25 h pre-debit gap, sandbox constraints, ON_DEMAND charge usable instantly. — [Subscriptions FAQ](https://www.cashfree.com/docs/payments/subscription/faq.md)
- BNPL/Pay Later/EMI: credit-card EMI, debit-card EMI ("Coming Soon" on pricing page), cardless EMI and pay-later (LazyPay, Simpl, Ring named) are per-transaction priced with no figures public; sandbox supports EMI, PayLater, cardless EMI (earlier notes). — [Pricing](https://www.cashfree.com/pricing/)
- International: cards (Visa, Mastercard, Amex, Diners) and PayPal; local bank transfers in USD/CAD/GBP/EUR/CHF; SWIFT multi-currency virtual account (22 currencies); settles in INR "T+2"; 2.95-2.99% per transaction (pricing page shows Visa/Mastercard 2.50% — conflict); daily FIRC reports; 23 settlement currencies listed vs "100+" and "30+" claimed on the same page; needs onboarding; international cards toggled at Settings > Payment Methods; PayPal connected with own account and settled by PayPal; PayPal and bank transfer not supported in sandbox (earlier notes). — [International page](https://www.cashfree.com/growth/ipg), [Pricing](https://www.cashfree.com/pricing/)
- Relay templates relevant to dues: Subscription Dunning, Loan Repayment, Loan Recovery, Abandoned Cart Recovery, Dispute Defender; triggers include Payment Failed and schedule-based. — [Relay overview](https://www.cashfree.com/docs/tools-ai/relay/overview.md)
- Resolve: outbound AI voice call within a minute of payment failure naming the failure reason and offering a way to complete payment. — [GFF 2026 blog](https://www.cashfree.com/blog/gff-product-launches-2026/)
- Relay (FAQ): reads failed-payment error codes, explains reason to customer in 22+ Indian languages and sends a new retry link; voice models Sarvam, Bolna, Osvi (bring your own allowed). — [Relay FAQ](https://www.cashfree.com/docs/tools-ai/relay/faq.md)

### Inferences
- Overdue B2B invoice chasing (aging buckets, multi-step dunning, GST invoice matching) is not a Cashfree product: it would be an agent layer combining Payment Links (UPI reminders, partial payments, expiry), per-invoice virtual accounts and webhooks.
- Payment Links partial payments exclude UPI, which matters in India where UPI dominates; an agent should route instalments through "all methods" links or multiple links.

### Gaps
- UPI QR product docs, Payment Form sandbox support, reminder cadence/limits, WhatsApp link delivery terms: not retrieved.
- Auto Collect current docs, API names, sandbox, pricing, per-account limits: not found.
- BNPL eligibility/pricing: not public.

## 3. Reconciliation

### Takeaway
Cashfree gives an event-level Settlement Reconciliation API (`POST /settlement/recon`, version 2026-01-01, available in sandbox path), a PG Reconciliation API, scheduled SFTP reports, UTR-bearing downloads and settlement webhooks, enough to automate matching of bank credits to orders. A distinct product called "Cashfree Recon" was not found.

### Cited Findings
- Settlement Reconciliation API: prod `https://api.cashfree.com/pg/settlement/recon`, sandbox `https://sandbox.cashfree.com/pg/settlement/recon`; filters by `cf_settlement_ids`, `settlement_utrs` or initiated/processed date ranges (`start_date`/`end_date` deprecated); cursor pagination, limit default 10, max 1000 (account may cap lower); `x-idempotency-key` optional; 429 headers `x-ratelimit-*`, stated spec example 200/min, actual unstated. No separate enablement. — [API reference](https://www.cashfree.com/docs/api-reference/payments/latest/settlements/settlement-reconciliation.md), [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md)
- Response fields: event_type (PAYMENT, REFUND, REFUND_REVERSAL, DISPUTE, DISPUTE_REVERSAL, CHARGEBACK, CHARGEBACK_REVERSAL, OTHER_ADJUSTMENT), event amount/service charge/tax, order_id and `order_tags`, customer details incl. bank account/IFSC, cf_payment_id, bank_reference, forex conversion charges, cf_settlement_id, settlement_utr, settlement charge/tax, split and vendor commission, dispute and refund arrays. — [API reference](https://www.cashfree.com/docs/api-reference/payments/latest/settlements/settlement-reconciliation.md)
- No status field in recon rows (presence of `settlement_utr` = submitted); status from Get All Settlements (SUCCESS, PENDING, PENDING_WITH_CASHFREE, PENDING_WITH_BANK, FAILED); reversals not returned, use `SETTLEMENT_REVERSED` webhook. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md)
- Settlement webhooks: SETTLEMENT_INITIATED (internal; do not trigger payouts), SETTLEMENT_SUCCESS (terminal, UTR), SETTLEMENT_FAILED, SETTLEMENT_REVERSED (usually within 1-3 banking days); verify `x-webhook-signature`. — [Manage settlements](https://www.cashfree.com/docs/payments/manage/settlements)
- Other tools: PG Reconciliation API (payment-account events by date range), Schedule Reports with Settlement Recon and Transactions reports over SFTP, downloadable settlement reports, instant settlements appear immediately in reports. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md), [Instant Settlements page](https://www.cashfree.com/instant-settlements/)
- Doc-version contradiction: PG Recon API has pages for versions 2022-09-01, 2023-08-01, 2025-01-01 and latest (2026-01-01), so older integrations use older shapes. — [search listing](https://www.cashfree.com/docs/api-reference/payments/previous/v2025-01-01/settlements/settlement-reconciliation.md)
- Spark builds reconciliation reports and explains short settlements; hosted MCP has settlements and reports tools; Relay can sync transactions to Google Sheets and send scheduled reports. — [Spark blog](https://www.cashfree.com/blog/introducing-spark-the-agentic-command-surface-for-your-business/), [Relay overview](https://www.cashfree.com/docs/tools-ai/relay/overview.md)
- Wider reconciliation: Citibank joining as AD Category-I partner for collection, reconciliation, FX and remittance (no date/scope). — [GFF 2026 blog](https://www.cashfree.com/blog/gff-product-launches-2026/)

### Inferences
- An agent can reconcile bank-statement credits to orders by UTR + settlement recon rows, flag deltas (MDR, refunds, chargebacks, other adjustments) and detect "UTR present but not in bank" cases. Matching to the merchant's own ERP/invoices is the agent's job, via `order_tags`/`order_id`.

### Gaps
- Whether the sandbox returns realistic recon rows without simulated payments and settlements (settlement simulation exists only for Easy Split vendors per earlier notes): unknown.
- No "Cashfree Recon" product page found; no dashboard reconciliation module documented beyond reports.

## 4. Gateway pain points: reserves, holds, disputes, webhooks

### Takeaway
Cashfree publicly documents dispute flow, deadlines and webhooks but says nothing found about rolling reserves, hold criteria or high-risk categories beyond "risk parameters" and bank approval. Webhook retry policy is documented (see earlier notes).

### Cited Findings
- Dispute stages and response windows (calendar days): dispute 3 (1 for fraud), retrieval 3, chargeback 3, pre-arbitration 2, arbitration 1; resolution: cards chargeback 30-45 days, UPI 15-20 days, arbitration about 15 days; whole process may take about 3 months. — [Disputes overview](https://www.cashfree.com/docs/payments/manage/disputes/overview.md); the help page gives the same windows — [Transactions FAQ](https://www.cashfree.com/docs/help/payments/transactions/transactions.md)
- Disputed amount is debited from the merchant when raised and credited back on a win; not responding loses the dispute; accept = refund; arbitration and pre-arbitration carry high penalties/fees (amounts unstated). — [Transactions FAQ](https://www.cashfree.com/docs/help/payments/transactions/transactions.md), [Disputes overview](https://www.cashfree.com/docs/payments/manage/disputes/overview.md)
- APIs and webhooks: Get Disputes by Order ID, Contest Arbitration; `DISPUTE_CREATED/UPDATED/CLOSED`. Evidence upload appears dashboard-driven (no upload API listed on the page). Hosted/local MCP list disputes tools (local repo mentions disputes in its 25 PG tools). Sandbox not mentioned. — [Disputes overview](https://www.cashfree.com/docs/payments/manage/disputes/overview.md), [cashfree-mcp repo](https://github.com/cashfree/cashfree-mcp)
- Settlement-impacting risk: settlement timing "subject to bank approval" and varies by "risk parameters"; Easy Split activation may land in RISK_APPROVAL_PENDING or RISK_REJECTED. — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md), [Easy Split test data](https://www.cashfree.com/docs/payments/split/data-to-test.md)
- Webhooks: HTTPS 200 required; PG up to 3 default retries at 2/10/30 min, configurable to 10; at-least-once, unordered; Payouts/Cashgram endpoint auto-disabled after more than 5 consecutive failures. — [Webhook reliability](https://www.cashfree.com/docs/api-reference/webhooks/reliability-and-operations.md) (from earlier notes)
- Spark can replay failed webhooks and has specialist agents for disputes and webhooks/error logs; Relay has a Dispute Defender template. — [Spark blog](https://www.cashfree.com/blog/introducing-spark-the-agentic-command-surface-for-your-business/), [Relay overview](https://www.cashfree.com/docs/tools-ai/relay/overview.md)

### Inferences
- Dispute deadlines of 1-3 days make deadline alerting a strong agent use case; the contest-with-evidence step is dashboard-only per public docs, so the agent drafts evidence and a human submits.

### Gaps
- Rolling reserve, settlement holds, high-risk merchant categories, chargeback fee amounts, risk-review SLAs: no Cashfree-published information found.
- Dispute evidence-requirements page and dispute sandbox not retrieved.

## 5. Cashfree's own AI products

### Takeaway
Relay (workflow agents), Spark (dashboard assistant with actions) and Resolve (voice recovery of failed payments) all launched around GFF 2026 (9-11 Sep 2026) and are inside the Merchant Dashboard; none lists pricing or sandbox. MCP, Toolkit and HERE are developer-side (see earlier notes) and cover settlements/reports only via hosted MCP.

### Cited Findings
- Spark (blog dated 8 Sep 2026): search-bar assistant, voice/text incl. Hindi, answers from live data with citations; answers on payments, refunds, settlements, disputes, reports, links, offers, webhooks; actions: create payment links, build reconciliation reports, process refunds, initiate instant settlements, replay failed webhooks, create/pause offers, whitelist domains, adjust limits; never moves money without approval; honours dashboard permissions; audit trail; reminders not mentioned; no pricing or availability. — [Spark blog](https://www.cashfree.com/blog/introducing-spark-the-agentic-command-surface-for-your-business/)
- Relay: specialist agents for cart recovery, failed payment retries, COD confirmation, subscription renewals, refund screening, dispute filing; approval before money moves or customer contacted; plain-language setup; (dashboard) triggers Payment Success/Failed, Refund Success, schedule, inbound webhook; connections Gmail, Slack, Google Sheets, WhatsApp, SMS, email, Claude, Sarvam, Bolna, Osvi; no collections, reconciliation or settlement example agents on the page; no pricing, sandbox. — [Relay overview](https://www.cashfree.com/docs/tools-ai/relay/overview.md), [GFF 2026 blog](https://www.cashfree.com/blog/gff-product-launches-2026/), [Relay FAQ](https://www.cashfree.com/docs/tools-ai/relay/faq.md)
- Third-party reporting on Relay exists (IBS Intelligence, StartupTalky) but IBS fetch failed and StartupTalky was not fetched. — [IBS Intelligence](https://ibsintelligence.com/ibsi-news/cashfrees-relay-brings-ai-agents-to-smb-payment-operations/)
- Docs index lists "Relay MCP Server (Archived)" and "Relay Skills (Archived)" pages, i.e. earlier Relay developer surfaces were archived. — [llms.txt via fetch](https://www.cashfree.com/docs/llms.txt)
- Resolve: no docs page in the index; only the GFF blog description above. — [GFF 2026 blog](https://www.cashfree.com/blog/gff-product-launches-2026/)
- Hosted MCP/local MCP/Toolkit/HERE/Agent Skills: see earlier notes (Settlements & Recon are covered by Agent Skills docs pack; hosted MCP has settlement and reports tools, refunds, payment links, `simulate_payment`). — [cashfree_capabilities.md](/home/claude/research_notes/Sarvam%20Cashfree%20buildathon%20ideas/cashfree_capabilities.md)
- Press note: Relay called "free, deferring monetisation" by a third-party blurb (enterprisedna) — unverified secondary source, not fetched. — [Enterprise DNA](https://enterprisedna.co/resources/ai-pulse/ai-pulse-2026-08-30-a-payments-platform-launches-free-defers-monetization-to-out)

### Inferences
- Cashfree's native agents cover payment-failure recovery, refunds, disputes and dashboard Q&A; gaps they leave are cash-flow forecasting, receivables aging/dunning for invoices, and cross-source reconciliation against the merchant's books.

### Gaps
- Resolve technical docs, Spark/Relay pricing, plan availability and any API for them: not found.

## 6. What an agent can drive end to end in sandbox, and what needs enablement

### Takeaway
Fully drivable in sandbox with self-serve PG keys: create order, pay with test UPI/cards, `simulate_payment`, payment links (including bulk flow test), refunds, ON_DEMAND subscription charge, the Settlement Recon API call, and vendor settlement simulation (Easy Split). Real settlement timing, Instant/On-Demand settlements, Advances, Auto Collect, UPI AutoPay, Batch links, Easy Split and international need Cashfree enablement or are not sandboxable.

### Cited Findings
- Sandbox basics, payment modes, UPI `@gocash` test VPAs, rate limits, subscriptions limits, Easy Split settlement simulation, MCP `simulate_payment`: see earlier notes. PayPal and bank transfer are not supported in sandbox. — [Sandbox environment](https://www.cashfree.com/docs/payments/online/resources/sandbox-environment.md)
- Settlement Recon API has a sandbox base URL and needs no separate enablement. — [API reference](https://www.cashfree.com/docs/api-reference/payments/latest/settlements/settlement-reconciliation.md)
- Needs enablement: On-Demand (Request Activation, account manager), Instant limit increase (dashboard request), Batch payment links (Support Form), Easy Split, UPI AutoPay, AFA for Rs 15,000+, Advances (account manager). — [Settlements help](https://www.cashfree.com/docs/help/payments/settlements/settlements.md), [Payment Links FAQ](https://www.cashfree.com/docs/help/payment-links/faqs/faqs.md), [Subscriptions FAQ](https://www.cashfree.com/docs/payments/subscription/faq.md), [Cashfree Advances](https://www.cashfree.com/working-capital-loan/)
- Production-only in practice: real bank settlement, UTRs, MDR figures (sandbox TDR does not reflect production), chargebacks, Advances, Spark/Relay/Resolve actions on real data. — [Sandbox environment](https://www.cashfree.com/docs/payments/online/resources/sandbox-environment.md)

### Inferences
- One-day demo pipeline that stays inside sandbox: agent creates payment links/orders for dues, simulates payments, polls/receives webhooks, pulls recon rows, matches to a mock ledger, forecasts cash arrival using documented T+1/T+2 rules and holiday calendar, and proposes (not executes) an on-demand settlement or Advances enquiry. Treat instant/on-demand execution as an unverified "if enabled" step; request activation beforehand.
- Whether recon rows exist for sandbox payments should be tested in the first hour; fall back to fixture data if empty.

### Gaps
- No evidence of a buildathon-specific pre-enabled sandbox; account enablement lead times unpublished.
- Sandbox availability of Get All Settlements data for test payments: not documented.

## Flagged contradictions / possibly outdated
- Default settlement cycle T+1 vs T+2 (Settlements help vs Instant page vs Manage settlements).
- Instant settlement speed: "15 minutes" vs "30 minutes on request" (same marketing page); Rs 50,000 auto limit appears only in docs.
- International fee: 2.95-2.99% (International page) vs Visa/Mastercard 2.50% (Pricing page); currency counts 23 vs 30+ vs 100+.
- Pricing page layout garbled (1.95% vs 2.0%); no instant/on-demand/links/forms fee figures anywhere public.
- Auto Collect: product page vs legacy docs differ on TPAP support; modern docs location unclear (virtual accounts only under Payouts OneEscrow in llms.txt index).
- Payment Links: WhatsApp delivery in earlier notes vs SMS/email only on FAQ; `link_status` may show EXPIRED after payment.
- Legacy `docs.cashfree.com` pages (On-Demand, Auto Collect) are older than `cashfree.com/docs` and may be stale.
