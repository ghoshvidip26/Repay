# Cashfree payables, treasury, verification, financing and cross-border products mapped to cash-flow problems (state as of 27 Oct 2026)

Method note: ~17 tool calls in this pass. Fresh fetches (27 Oct 2026) of Cashfree marketing pages, a 2023 newsroom release, a Skydo competitor review and a TipRanks auto-summary of a Cashfree LinkedIn post. Docs-level facts (Payouts V2 limits, sandbox magic values, approvals, Payout Protect, rate limits) are carried over from the earlier notes file at /home/claude/research_notes/Sarvam Cashfree buildathon ideas/cashfree_capabilities.md, which cited cashfree.com/docs pages; I did not re-fetch those docs and tag them "(prior notes)". The Payouts docs FAQ page fetch hit a size limit and I only grepped it. Not found: any Cashfree-owned payroll product, TReDS connection, working-capital loan to merchants, or cash-flow forecasting feature (see Gaps). Several Cashfree marketing pages are undated; treat feature claims there as marketing, not verified behaviour.

## 1. Vendor and payroll payouts (payables side): Payouts, bulk, Cashgram, approvals, Payout Protect, balances, accounting integrations

### Takeaway
Cashfree Payouts is a mature API/dashboard disbursal engine (bank/UPI/card/wallet, bulk up to ~10k txns/min, maker-checker approval, risk holds) with a separate "Vendor Payments" invoice-to-pay layer and Tally integrations; it is not a payroll product (no payroll/HRMS suite found) and has no documented native scheduled-payout or auto-top-up API. For a buildathon, Payouts V2 sandbox is rich, but accounting-software integrations (Tally, Zoho, SAP) are production-side and not sandbox-demoable.

### Cited Findings
- Payouts pricing, pay-as-you-go (excl. GST and bank charges): NEFT Rs 3 (up to Rs 1,000), Rs 5 (Rs 1,000-25,000), Rs 8 (above Rs 25,000); RTGS Rs 4 / 6 / 10; IMPS/UPI/Amazon Pay Rs 6 / 8 / 15. Enterprise pricing is custom/quote. — [Cashfree Connected Banking page](https://www.cashfree.com/connected-banking/)
- Fund sources: Payouts can be funded from the Cashfree wallet (virtual account auto-created on Payouts activation), multiple connected current accounts (ICICI, YES, Kotak, IndusInd, Axis, RBL, HDFC, DBS, IDFC First listed), Paytm wallet, corporate credit card (Mastercard/Visa), and escrow accounts; "External Credit Line" is shown as "Coming Soon". Users can define weightages between sources, check balances and switch banks to avoid single-bank downtime. — [Connected Banking](https://www.cashfree.com/connected-banking/)
- Wallet low-balance alert: user can set a threshold and be notified (webhook event LOW_BALANCE_ALERT also exists). — [Connected Banking](https://www.cashfree.com/connected-banking/); event list in prior notes ([Payout webhooks](https://www.cashfree.com/docs/payouts/payouts/make-payouts/webhooks.md))
- Payouts page (Oct 2026 cache) claims multi-bank routing, Tally sync, reconciliation across banks, pre-payout checks (validate UPI handle / account+IFSC, confirm beneficiary, flag payouts for review, duplicate/incorrect payout avoidance, manual review steps), Cashgram payout links via SMS/email/WhatsApp, and use cases for lending, marketplaces ("scheduled bulk disbursements"), gig, insurance, e-commerce, quick commerce. — [Cashfree Payouts](https://www.cashfree.com/payouts/)
- Docs FAQ: Batch Transfers need approval before funds move; approvers are assignable; dashboard filters include Pending Approval / Rejected / Cancelled; bulk "up to 10,000 transactions in a minute"; modes include IMPS, NEFT, RTGS, UPI, Mastercard MoneySend, Visa Direct, Amazon Pay; Paytm wallet payouts now require the merchant's own Paytm business account. — [Payouts General FAQs](https://www.cashfree.com/docs/help/payouts/general-faqs)
- Blog says bulk payouts "almost up to 10,000 disbursals" by uploading an Excel sheet. — [Cashfree blog: What are Payouts](https://www.cashfree.com/blog/what-are-payouts/)
- Vendor Payments (launched 22 Aug 2023): purchase-to-pay platform; upload/import invoices (manual, bulk, or from accounting tools), add beneficiaries, initiate single or bulk payouts for unpaid invoices, covers vendor bills, utilities, other expenses and taxes; modes IMPS, NEFT, IFT, UPI; syncs unpaid invoices and payment status with Tally, TallyPrime "and other ERPs"; Maker-Checker flow with access levels; invoice reports for audit (invoice no., date, payment terms, beneficiary, mode). — [Cashfree newsroom, Vendor Payments](https://www.cashfree.com/news-room/cashfree-payments-launches-vendor-payments-to-empower-businesses-to-streamline-purchase-to-pay-cycle/)
- "Banking Payouts on Tally", built with IDFC FIRST Bank, unveiled at GFF 2026: IDFC FIRST Bank customers initiate vendor payouts from within Tally, with payment statuses flowing back for reconciliation (secondary summary of Cashfree's LinkedIn post; Cashfree's own Facebook post exists but was not scrapable). — [TipRanks summary](https://www.tipranks.com/news/private-companies/cashfree-payments-and-idfc-first-bank-integrate-payouts-into-tally-erp), [Cashfree Facebook post](https://www.facebook.com/cashfreepayments/posts/unveiled-at-gff-2026-banking-payouts-on-tally-launched-in-partnership-with-idfc-/1451084240399693/)
- Zoho Books: a "Cashfree Payments for Zoho Books" marketplace app exists; its snippet mentions accepting payments, refunds and paying (vendor-side wording truncated). — [Zoho Marketplace listing](https://marketplace.zoho.com/app/books/cashfree-payments-for-zoho-books)
- (prior notes) Payouts V2 sandbox: base `https://sandbox.cashfree.com/payout`; deterministic magic accounts (e.g. `026291800001191`/YESB0000262 success, `007711000031`/HDFC0000077 pending, pending-then-failure, success-then-reversed, 25s timeout), `success@upi`/`failure@upi`; each payout mode must be enabled by Cashfree on test and prod; OTP `111000`. — [Payouts test data](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md)
- (prior notes) Limits: IMPS up to Rs 5 lakh, NEFT default above Rs 2 lakh, UPI up to Rs 1 lakh, Paytm Rs 1 lakh, Amazon Pay Rs 10,000; batch transfers are bank-transfer only; wallet withdrawals max 3/day to registered business bank; payouts only for registered businesses with business PAN. — [Payout methods](https://www.cashfree.com/docs/payouts/payouts/make-payouts/payout-methods.md), [Payouts FAQ](https://www.cashfree.com/docs/payouts/payouts/general-faqs.md)
- (prior notes) Payout Protect: ML anomaly detection, blacklists, ATO detection, Smart Rules velocity limits; risky transfers held for approve/block; fraud blocks show as `Rejected`. Webhooks TRANSFER_APPROVED / TRANSFER_REJECTED. — [Lifecycle](https://www.cashfree.com/docs/payouts/payouts/make-payouts/lifecycle.md)
- (prior notes) Relay (dashboard no-code agent) supports schedule-triggered vendor payouts with human approval via email/Slack; sandbox not mentioned. — [Relay overview](https://www.cashfree.com/docs/tools-ai/relay/overview.md)
- Third-party: Cashfree was claimed (2023 release, self-reported) to have ">50% market share among payment processors" in bulk disbursals and 3,00,000+ businesses; the 2026 site says "1M+ businesses". — [Newsroom 2023](https://www.cashfree.com/news-room/cashfree-payments-launches-vendor-payments-to-empower-businesses-to-streamline-purchase-to-pay-cycle/), [Cashfree intl page](https://www.cashfree.com/accept-international-payments-from-india/), [Skydo review](https://www.skydo.com/compare/cashfree-overview)

### Inferences
- "Pay the right vendor on the right day" is best expressed as an agent over Payouts V2 (Standard Transfer + Batch) plus verification and an approval step; scheduling and top-up logic would be agent-side (cron + balance check + LOW_BALANCE_ALERT webhook), not Cashfree features.
- Low-balance alert plus multi-source weightages is the closest Cashfree gets to payables-side treasury; there is no documented auto-top-up from a linked account.
- Cost of a payout is low relative to ticket size (Rs 15 for IMPS above Rs 25,000), so "pay early to capture discounts vs pay late to preserve cash" decisions are not constrained by fees.

### Gaps
- No Cashfree payroll/salary product (HRMS, statutory compliance, payslips) found; only "wage payouts" and "salary distributions" as payout use cases (FAQ). Verified only via marketing text.
- Native scheduled-payout API, auto-top-up, SAP integration: not found. Zoho Books vendor-payout capability not confirmed beyond a truncated snippet. Tally integration is production-only per available sources; sandbox availability not documented.
- Whether sandbox Payouts wallet is pre-funded remains unknown (carried from prior notes).
- Payouts docs FAQ was only grepped, not fully read.

## 2. Payables fraud: Verification Suite / Secure ID for vendor onboarding and changed-bank-detail fraud

### Takeaway
Cashfree sells bank account verification (penny drop, penny-less, bulk, UPI penny drop, reverse penny drop) with name-match scores, plus GSTIN/PAN/CIN/Udyam APIs; vendor verification is an explicitly named use case. There is no product named for "changed bank details" detection; the pattern must be built by re-verifying and name-matching whenever a vendor's account changes, then gating payouts via maker-checker.

### Cited Findings
- Bank Account Verification methods: penny-drop (Rs 1 deposit; returns account status plus name and match score), bulk verification (file or API, per-row status, lower cost), reverse penny drop (customer sends Rs 1 from own account, consent on record), UPI penny drop (resolves account holder behind VPA, no IFSC), and penny-less verification (real-time, no deposit). Availability of some methods "depends on business use case and applicable regulations". — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- Named use case "Vendor Verification: validate supplier accounts before you release funds"; also onboarding, lending, insurance, payroll/gig; claims 600+ banks incl. 126+ co-operative banks; maker-checker approval built in; 99.5% verification success rate (marketing claim); returns status, name, match score, IFSC and branch. — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- Pricing on that page: Rs 100 free credits on signup, pay-as-you-go Rs 3 per verification, enterprise custom. Page FAQ says verification is "part of the SecureID suite" and bundle-able with Payouts. — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- Page names mule-account detection ("Detect mule accounts before transfers") for fintech/NBFC use; mechanics not described. — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- Named verification customers on the page: Zomato (delivery-partner onboarding), BookMyForex, Bajaj Finserv. — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- (prior notes) Secure ID APIs: Bank Account Verification sync/async V2, IFSC, UPI Penny Drop, PAN Lite/360, Name Match, GSTIN, GSTIN-with-PAN, CIN, Udyam, Mobile 360. Sandbox mocks: name match JOHN DOE vs JOHN DOE = 1.0 DIRECT, vs JOHN DE 0.93, JOHN 0.77, J DO 0.53, DO 0 NO_MATCH; Reverse Penny Drop has little mock data. Rate limits per merchant: 100 TPM standard (bank verification, UPI penny drop, name match, GSTIN, PAN 360), 200 TPM medium (reverse penny drop create), 5 TPM restricted (bulk PAN). Features must be enabled by Cashfree on TEST and PROD. — [VRS test data](https://www.cashfree.com/docs/api-reference/vrs/data-to-test-integration.md), [Rate limits](https://www.cashfree.com/docs/)
- (prior notes) V2 "Validate Payout" checks UPI VPA/phone and returns a single-use transfer token; bank-account validation is via Secure ID. — [Payouts V2 overview](https://www.cashfree.com/docs/api-reference/payouts/v2/payouts-api-v2-new.md)

### Inferences
- Defence against changed-bank-detail (BEC-style) fraud = on any vendor record change: re-run penny drop/UPI penny drop, compare returned name to GSTIN legal/trade name (GSTIN API) and the vendor's name on file, hold the payout in Cashfree approval if score below a threshold. All components exist; the orchestration is not a Cashfree feature.
- The shared magic account numbers across Secure ID and Payouts sandbox make a coherent verify-then-pay demo feasible.

### Gaps
- No Cashfree document describing a "bank detail change" alert, vendor master-data monitoring, or duplicate-invoice detection.
- Per-API prices for GSTIN/PAN/name-match not found (only Rs 3 per bank verification headline).
- Whether Secure ID is enabled by default on a fresh test account remains unclear.

## 3. Timing of outflows vs inflows: forecasting, scheduled payments, balances, nodal/escrow, Easy Split, instant settlements

### Takeaway
Cashfree offers building blocks that move timing of money (instant settlements, escrow, Easy Split vendor settlements, multi-source payout funding, reconciliation) but no cash-flow forecasting or payment-date optimisation product was found. A forecasting/timing layer would be an agent on top of Cashfree data.

### Cited Findings
- Instant Settlements: blog pitches ready funds for bill repayments, taxes and other liabilities (details on eligibility, fee not fetched). — [Cashfree blog, Instant Settlements](https://www.cashfree.com/blog/instant-settlements-daily/)
- Marketplace payouts: Payouts page lists "scheduled bulk disbursements", "commission and settlement flows", "real-time settlement cycles". — [Cashfree Payouts](https://www.cashfree.com/payouts/)
- Easy Split: collect, deduct commission, split to vendors/partners (YouTube listing). — [Easy Split video listing](https://www.youtube.com/watch?v=7gWk996tLM4)
- (prior notes) Easy Split sandbox: enable via 2026-01-01 API version, status ACTIVE/HOLD/NOT_ACTIVE/RISK_APPROVAL_PENDING/RISK_REJECTED; `order_splits` on order create; Simulate Vendor Settlement gives INITIATED/SUCCESS/FAILED; vendor-settlement webhooks need webhook version 2025-01-01. Enablement can stall in risk approval. — [Easy Split test data](https://www.cashfree.com/docs/payments/split/data-to-test.md)
- One Escrow ("Escrow as a Service") and Co-lend escrow: Co-lend (Feb 2023) automates escrow disbursal/repayment and reconciliation for co-lending, with ICICI, Kotak, Yes, IndusInd, Axis integrations; auto-updates LMS. — [Co-lend release](https://www.cashfree.com/news-room/cashfree-payments-launches-co-lend-indias-first-fully-automated-escrow-management-solution-for-co-lending/); NBFC escrow page: [Cashfree NBFC lending](https://www.cashfree.com/nbfc-digital-lending-solutions/)
- (prior notes) Webhook events CREDIT_CONFIRMATION, TRANSFER_REVERSED, LOW_BALANCE_ALERT; status polling of banks for 72 hours before manual reconciliation. — [Payout webhooks](https://www.cashfree.com/docs/payouts/payouts/make-payouts/webhooks.md)

### Inferences
- A cash-position agent could read Cashfree settlements/payout statuses + wallet balance, project 7-30 day inflows (Payment Links/PG/subscriptions) vs outflows (vendor payables with due dates), and decide which bills to pay now vs hold; execution through Payouts, approvals through Payout Protect/batch approval. This is a build opportunity, not a Cashfree feature.

### Gaps
- No evidence of cash-flow forecasting, early/late payment optimisation, dynamic discounting, or nodal-account-specific features for merchants (nodal/escrow for PA operations is regulatory, not a merchant treasury tool).
- Instant Settlement fee/limits and One Escrow specifics were not fetched.
- Easy Split public pricing not found.

## 4. Access to capital: loans, invoice discounting, BNPL, advances, TReDS, Account Aggregator/ULI

### Takeaway
Cashfree does not lend to merchants from its own balance sheet, as far as public sources show. It provides infrastructure for lenders (escrow, co-lending, disbursal, embedded lending APIs) and a BNPL gateway product; TReDS and ULI involvement were not found. Account Aggregator participation is TSP-level (2023 statement).

### Cited Findings
- Embedded Lending ("lending as a service"): platforms can embed credit; supported product types listed are Line of Credit, Term Loan, "Supply Chain Financing and Invoice Discounting", and BNPL; for expense-management, SME neobanks, logistics, B2B e-commerce, agri-tech, pharma-tech SaaS; 5-step flow (details/PAN/GST, documents, offer, repayment via recurring billing, agreement); "go live in 2 weeks"; sales-led (Contact Sales). Page does not name the lenders. — [Cashfree Embedded Lending](https://www.cashfree.com/embedded-lending/)
- NBFC digital lending page: escrow accounts with partner banks/trustees plus dashboard/API for loan disbursal and repayment collection; blog lists supply chain financing and BNPL among supported loan types. — [NBFC solutions](https://www.cashfree.com/nbfc-digital-lending-solutions/), [P2P/digital lending blog](https://www.cashfree.com/blog/p2p-loans-digital-lending-guide/)
- Payouts used for "loan disbursals, EMI refunds and reversals, multi-bank fund allocation" (lending/NBFC use case); insurance for "claims disbursement, policy refunds". — [Cashfree Payouts](https://www.cashfree.com/payouts/)
- Cashfree "joined the Account Aggregator framework as a TSP" (Aug 2023 release). — [Vendor Payments release](https://www.cashfree.com/news-room/cashfree-payments-launches-vendor-payments-to-empower-businesses-to-streamline-purchase-to-pay-cycle/)
- BNPL listed under Collect Payments ("Buy Now Pay Later", consumer checkout product; business BNPL not found). — [Cashfree site nav](https://www.cashfree.com/news-room/cashfree-payments-launches-vendor-payments-to-empower-businesses-to-streamline-purchase-to-pay-cycle/)
- Fund Sources shows "External Credit Line" as coming soon (i.e. paying from a credit line); corporate credit card already supported as payout source. — [Connected Banking](https://www.cashfree.com/connected-banking/)
- TReDS: web search for Cashfree + TReDS/invoice discounting returned only TReDS platforms (RXIL, M1xchange, KredX/DTX, Invoicemart) with no Cashfree connection. — search on 27 Oct 2026; e.g. [RXIL](https://www.rxil.in/benefits-of-treds/)

### Inferences
- For "my cash is tight" an agent could use Cashfree rails for disbursing an approved loan or paying from a credit card fund source, but the credit decision/lender must be external (TReDS platforms, NBFCs); Cashfree is plumbing.
- Account Aggregator data (bank statements) is the natural cash-flow input, but Cashfree's own AA usage for merchants is unconfirmed; a separate AA FIU/TSP route would be needed.

### Gaps
- No Cashfree-branded "Capital"/merchant cash advance, settlement-based advance, or merchant loan found; no newsroom entry on TReDS or ULI found. No pricing for embedded lending. Lending partners behind embedded lending not named.
- Unified Lending Interface: no Cashfree mention found.

## 5. Cross-border: international collections and payouts, FEMA/RBI PA-CB

### Takeaway
Cashfree holds an RBI PA-CB authorisation and runs both import-side (collect INR from Indian customers for overseas merchants) and export-side (Global Collections accounts and International PG for Indian exporters) products, with a USD 10,000 per-invoice cap for exporter Global Collections per third-party; fee is unpublished. It is inward-focused, not a tool to chase an overseas debtor.

### Cited Findings
- Cashfree claims to be the first non-bank with a PA-CB-E&I (export & import) certificate from RBI, processing inward and outward cross-border flows. — [Cashfree blog, foreign companies in India (2026)](https://www.cashfree.com/blog/best-payment-gateway-for-entering-india-market-guide/) (search snippet; Skydo says "Full PA-CB (2025)")
- Collect-from-India product: overseas merchants collect from Indian customers via UPI, cards, net banking (90+ banks) without a local entity; settlement in USD/EUR/GBP/SGD/AUD/CAD/HKD and 100+ others; "zero FX cost" claim but FX at partner bank rate on settlement date; settlement TAT = verification + 1 business day; unverified transactions auto-reversed; GST registration not required to start; "Powered by J.P. Morgan Chase". — [Accept International Payments from India](https://www.cashfree.com/accept-international-payments-from-india/)
- Export-side: facility limited to export of goods/services up to USD 10,000 per transaction (search snippet of Cashfree page). — [Cashfree international payments](https://www.cashfree.com/accept-international-payments/)
- Global Collections (third-party view): receiving accounts in USD, GBP, EUR, CAD plus Global SWIFT account for 30+ currencies from 180+ countries; auto-converted to INR (no foreign balances), settlement within 7 days of NOSTRO credit; free e-FIRA in 24 hours; eBRC not automated; fee unpublished (third parties estimate 1-1.5% plus GST); Trustpilot 1.8/5 (competitor-authored page, treat with caution). — [Skydo comparison](https://www.skydo.com/compare/cashfree-overview)
- International PG: foreign cards 2.99% regardless of currency (third-party). — [Xflow summary](https://www.xflowpay.com/blog/cashfree-payment-gateway-charges)
- Global Payouts: "send money to sellers, service providers or freelancers in India directly to local bank accounts" (inward to India). Outward remittance product is LRS-oriented (individuals: study abroad, travel, investments), with TCS and FX computation, same-day if before 1 PM IST; not an AP product for paying overseas vendors. — [Connected Banking page](https://www.cashfree.com/connected-banking/), [Outward Remittance](https://www.cashfree.com/international-payments/outward-remittance/)
- Skydo FAQ asserts Cashfree has no outward remittance/LRS product (contradicted by Cashfree's own outward remittance page, which is LRS for individuals via partners; both can be true for business import payments). — [Skydo](https://www.skydo.com/compare/cashfree-overview) vs [Cashfree](https://www.cashfree.com/international-payments/outward-remittance/)

### Inferences
- Useful for "overseas customer paid late / FX uncertainty": Global Collections or IPG gives a local-currency pay link/account plus e-FIRA for compliance; it does not extend credit or insure non-payment.
- Paying an overseas vendor (import payables) via Cashfree is not clearly supported for Indian businesses; bank/AD route likely needed.

### Gaps
- Cross-border sandbox and docs (docs.cashfree.com "international-payments/collect-from-india") not examined; likely requires enablement. Current PA-CB rule specifics (e.g. 2025 RBI Master Directions on cross-border PAs) not fetched, so cap values should be re-checked against RBI.
- Official pricing for Global Collections/IPG not published (aside from third-party figures).

## 6. Large-enterprise use cases and named public customers

### Takeaway
Cashfree publicly lists payout, lending, insurance and marketplace use cases and displays enterprise logos; explicit case-study attributions are thin in what I could fetch.

### Cited Findings
- Logos shown on the Fund Sources page: Meesho, MakeMyTrip, CRED, Rapido, Khatabook, Paytm First Games (logos only; the page does not state which products they use). — [Connected Banking](https://www.cashfree.com/connected-banking/)
- Verification page customer stories: Zomato (delivery-partner account verification so payouts land), BookMyForex (real-time account confirmation for high-value transfers), Bajaj Finserv (account/name checks in KYC). — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- Payouts use cases for enterprise: lending/NBFC disbursals with multi-bank allocation, marketplace seller payouts and commission flows, gig daily settlements, insurance claims, e-commerce COD reconciliation, quick-commerce high-frequency payouts. Reconciliation and Tally sync listed. — [Cashfree Payouts](https://www.cashfree.com/payouts/)
- Co-lend integrates with ICICI, Kotak, Yes, IndusInd, Axis for co-lending escrow. — [Co-lend release](https://www.cashfree.com/news-room/cashfree-payments-launches-co-lend-indias-first-fully-automated-escrow-management-solution-for-co-lending/)
- Investors: SBI, Y Combinator, Apis Partners, Krafton (latter per Skydo). — [Newsroom](https://www.cashfree.com/news-room/cashfree-payments-launches-vendor-payments-to-empower-businesses-to-streamline-purchase-to-pay-cycle/), [Skydo](https://www.skydo.com/compare/cashfree-overview)

### Gaps
- No quantified enterprise case study (volumes, savings) found; customer-story index (cashfree.com/case-study/) not scraped.
- Payouts page headline stats rendered as animation placeholders (success rate, Rs K Cr+ processed), so figures not captured.

## 7. Buildathon feasibility: what an agent can exercise in sandbox vs what needs Cashfree enablement

### Takeaway
In one day, the strongest feasible stack is Payouts V2 sandbox (transfer, batch, beneficiary, status/webhooks, approvals logic) + Secure ID sandbox (bank/UPI penny drop, name match, GSTIN) + Easy Split settlement simulation, all behind magic values; each requires Cashfree to enable features on the test account in advance. Tally/Zoho/SAP, Payout Protect, Cashgram live behaviour, Vendor Payments UI, lending, TReDS and cross-border are not sandbox-demoable by a participant and should be mocked.

### Cited Findings
- Sandbox-demoable (prior notes, per Cashfree docs): Payouts V2 standard/batch transfers with outcomes success/failure/pending/reversal; UPI payout `success@upi`; Secure ID bank verification/UPI penny drop/name match/GSTIN/PAN mocks; Easy Split simulate vendor settlement; PG order/refund/payment-link; local MCP (github.com/cashfree/cashfree-mcp, `ENV=sandbox` default) exposes PG 25 tools, Payouts 8 tools (incl. Cashgram create/deactivate/status), Secure ID tools. — [Payouts test data](https://www.cashfree.com/docs/payouts/payouts/integrations/data-to-test.md), [VRS test data](https://www.cashfree.com/docs/api-reference/vrs/data-to-test-integration.md), [Easy Split test data](https://www.cashfree.com/docs/payments/split/data-to-test.md)
- Needs Cashfree enablement even in test (prior notes): Payouts payment modes and validations, Secure ID features, Easy Split (risk approval states), UPI AutoPay, batch links; production requires business KYC (business PAN, website) so not feasible in a day. — [Account activation](https://www.cashfree.com/docs/help/account/account-activation.md)
- Verification free credits: Rs 100 on signup with sandbox/test data. — [Bank Account Verification](https://www.cashfree.com/bank-account-verification/)
- Production-side, sales-led, not self-serve: Embedded Lending, Vendor Payments/Tally integration, enterprise Payouts pricing, Global Collections (documents + quote), outward remittance, Co-lend. — pages cited above.

### Inferences
- Request enablement of Payouts modes (IMPS/NEFT/UPI), batch approval, and Secure ID (bank verification, UPI penny drop, name match, GSTIN) before the event; keep an in-code mock with the documented magic values as fallback.
- Mapping to problems: "paying wrong/fraudulent vendor" -> Secure ID + approval; "failed or reversed payouts" -> Payouts status/webhooks/reconciliation; "split of customer money among sellers" -> Easy Split; "need credit" -> mock lender (no Cashfree product); "overseas customer didn't pay" -> out of sandbox scope.

### Gaps
- Turnaround for Cashfree enablement requests undocumented. Cross-border/lending sandbox access unknown (none found).
- Conflicts flagged: hosted MCP covers PG/dashboard only vs overview text claiming Payouts/SecureID (prior notes); Agent Toolkit blog (30+ tools incl. identity checks) vs docs (13 tools, no verification); sandbox account `000100289877623`/SBIN0008752 documented as Success on one page and "Failed at bank" on others (prior notes); Skydo's "no outward remittance" vs Cashfree's outward remittance page; "Full PA-CB (2025)" (Skydo) vs "first non-bank PA-CB-E&I" (Cashfree blog snippet, date not captured).
