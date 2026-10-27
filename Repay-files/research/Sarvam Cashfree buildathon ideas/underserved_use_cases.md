# Underserved money-movement pain points for Indian-language-first users (state as of 27 Oct 2026)

Research budget was about 15 tool calls. Macro UPI, fraud and NPCI-initiative data are well sourced. Segment-level sizing (kirana, SHG, farmers, chit funds, RWA, tuition and others) is largely NOT verified. See Gaps in each section and do not treat unsourced segment numbers as facts.

## 1. Data points: UPI scale, vernacular users, voice, fraud

### Takeaway
UPI is now about 24 billion transactions a month, and rural and Indic-language users are the growth frontier. Reported UPI fraud is falling in rupee terms but still runs about 10-13 lakh incidents a year. NPCI itself is moving to voice and agentic assistants in Hindi, Bengali and Telugu, with Tamil and Kannada not confirmed.

### Cited Findings
- UPI Sept 2026: 24.07 bn transactions, Rs 29.37 lakh crore, about 802 mn/day, volume +23% YoY and value +18% YoY. Volume dipped 1.8% month on month from August. — [StartupTalky, Sept 2026](https://startuptalky.com/upi-transactions-september-2026/)
- UPI August 2026 was a record 24.51 bn transactions (headline only, not fetched in full). — [Startupfeed](https://startupfeed.in/upi-record-24-51-billion-transactions-august-2026/)
- FY2025-26: over 24,162 crore (241.6 bn) UPI transactions and about Rs 314 lakh crore in value. Banks live on UPI grew from 44 (FY17) to 703 (FY26). UPI turned 10 on 25 Aug 2026. — [YourStory, Aug 2026](https://yourstory.com/2026/08/upi-completes-10-years-clocks-nearly-13000-fold-rise-transaction-volume)
- Transactions up to Rs 2,000 are more than 96% of UPI merchant volume. The same source gives no P2M share. — [StartupTalky](https://startuptalky.com/upi-transactions-september-2026/)
- Internet users: Kantar-IAMAI ICUBE 2024 projects over 900 mn active users in 2025, with 488 mn rural against 397 mn urban. Rural growth is about 2x urban. Women are 47% of rural users. About 41% of the population is still offline. 57% of users prefer regional-language content. — [Business Today, Jan 2025](https://www.businesstoday.in/amp/technology/news/story/indias-internet-revolution-key-insights-from-kantar-and-iamai-report-461043-2025-01-16); original PDF: [IAMAI-Kantar 2024](https://www.iamai.in/sites/default/files/research/Kantar_%20IAMAI%20report_2024_.pdf)
- UPI fraud (Finance Ministry, Lok Sabha, Dec 2025):

  | Fiscal year | Amount (Rs crore) | Incidents (lakh) |
  |---|---|---|
  | FY22 | 242 | not reported |
  | FY23 | 573 | not reported |
  | FY24 | 1,087 | 13.42 |
  | FY25 | 981 | 12.64 |
  | FY26 (to Nov 2025) | 805 | 10.64 |

  — [IANS, 15 Dec 2025](https://ianslive.in/upi-frauds-worth-rs-805-crore-witnessed-this-fiscal-so-far-minister--20251215175703)
- NPCI "UPI Help" is an AI assistant for queries, status checks, complaints and mandate management (view, pause, resume, revoke autopay). It is reachable via bank websites and chatbots and the DigiSathi site. In-app access was "planned" as of Oct 2025. — [Angel One, 23 Oct 2025](https://www.angelone.in/news/personal-finance/npci-launches-ai-powered-upi-help-to-assist-users-with-digital-payments)
- NPCI announced FiMI (Finance Model for India), an in-house payments LLM, on 17 Feb 2026. It is already deployed through the UPI Help assistant and covers disputes, mandate lifecycle and regulatory queries. Languages: English, Hindi, Telugu and Bengali, with more planned in 6-8 months (so roughly Aug-Oct 2026; I did not verify that they shipped). NPCI ED Sohini Rajola: "Voice is the differentiator." Hello UPI is voice-based in 10 languages. An NPCI closed-user-group pilot for agent-linked payment delegation was reported for Oct 2025. — [MediaNama, 19 Feb 2026](https://www.medianama.com/2026/02/223-fimi-npci-in-house-ai-model-upi-ecosystem/)
- Hello UPI (announced at Global Fintech Fest) enables voice commands inside PSP apps, voice UPI PIN entry (flagged as a risk), and telecom/IoT integration. It is built on Bhashini and AI4Bharat. It launched with Hindi and English. BillPay Connect is a missed-call-then-callback flow for users without smartphones. — [Smartprix](https://www.smartprix.com/bytes/upi-payments-with-voice-commands)
- WhatsApp launched bill payments in India over the Bharat Connect (BBPS) network (headline only; details not verified). — [Morung Express](https://morungexpress.com/whatsapp-launches-bill-payments-in-india-via-bharat-connect-network)

### Inferences
- With UPI at about 800 mn transactions a day and about 96% of merchant volume under Rs 2,000, the unserved gap is not payment initiation. It is everything around the payment: reminders, reconciliation, collections, disputes, scheduling and fraud checks, in the user's language.
- NPCI is building language models for disputes and mandates. A third-party agent should differentiate on money-moving workflows (collect, split, schedule, payout) rather than on Q&A support, which NPCI is commoditising. NPCI's language list (Hindi, Bengali, Telugu) leaves Tamil and Kannada comparatively open, but I did not confirm this.
- Fraud amounts are falling from the FY24 peak, but incident counts of 10-13 lakh a year still make a "check before you pay or approve" feature credible. The headline numbers undercount, because the Lok Sabha figures are bank-reported UPI cases only.

### Gaps
- P2M vs P2P share of UPI (current): not found in the fetched sources. Needs the NPCI statistics page.
- Voice and feature-phone adoption: no TRAI or ICUBE figures found. UPI123PAY volumes not found.
- Collect-request scam statistics, I4C cyber-fraud totals (the all-cyber-crime figure, not UPI-only) and financial-literacy stats (NCFE 2019, RBI FLS) were not retrieved.
- Findex 2025, TRAI feature-phone counts and the latest ICUBE (2025) edition were not retrieved. The ICUBE figures above are the 2024 edition published Jan 2025.

## 2. Segments: size, workaround, money action, language barrier

### Takeaway
Only three segments have usable, sourced numbers: gig workers, internal migrants (old data) and SHGs (not retrieved). The remaining segments are listed below as hypotheses from general knowledge and need verification before the report writer uses them.

### Cited Findings
- Gig workers: about 7.7 mn today (about 1.5% of the workforce) with a projection of 23.5 mn by 2029-30 (about 4.1% of the workforce). Skill mix is 47% medium, 22% high and 31% low. — [IndBiz / NITI Aayog](https://indbiz.gov.in/indias-gig-economy-to-have-23-5-million-workers-by-2029-30-niti-aayog/). The article does not state a year for "today", and the figure is a 2020-21 baseline from the 2022 NITI report. Treat it as dated.
- Internal migrants and remittances (2011 CGAP study, 274 migrants, four corridors): 57% used an informal channel (hawala 26%, cash courier 19%, friend 11%), 30% used banks and 13% India Post. On a Rs 2,000 transfer, hawala cost about 4.6%, India Post about 6% including indirect costs, and banks about 3%. A bank transfer took about 2.5 hours of combined sender and recipient travel and waiting. Nearly half of hawala users said they would avoid it if they could. — [CGAP, Feb 2011](https://www.cgap.org/blog/how-do-migrant-workers-move-money-in-india). This is 15 years old; use it only to show the structure of the pain, not current sizes.
- The IFMR/CGAP "Putting Money in Motion" and "Domestic Transfers" PDFs and the NABARD SHG pages surfaced in search but were not read. — [IFMR](https://ifmrlead.org/files/OWC/42_Putting_Money_in_Motion.pdf), [NABARD](https://www.nabard.org/content.aspx?id=477)

### Inferences (hypotheses, not sourced; verify before use)
| Segment | Money action | Workaround | Why language and voice matter | Cashfree capability (conceptual) |
|---|---|---|---|---|
| Kirana and khata (udhaar) | Collect dues, remind, partial payments | Paper or app ledger plus phone and in-person reminders | Owner is vernacular-first; typing amounts and names is slow; voice entry ("Raju ka 500 baaki") is natural | Payment links over WhatsApp, UPI collect via links, auto-reconciliation |
| Street vendors and micro-merchants | Collect (QR), pay suppliers, settle | Static QR plus cash | Low literacy in English UIs; settlement and dispute status is opaque | Payouts to suppliers, settlement visibility, verification |
| Gig and delivery workers | Instant earnings, bill pay, savings | Weekly platform payout; ad hoc UPI | Contract and payout screens are English-heavy | Payouts (instant), account/UPI verification, scheduled bill pay |
| Farmers, FPOs, mandi | Mandi payments, FPO payouts, input purchases | Cash or cheque to the commission agent; delayed bank transfers | Low digital literacy; payee names and amounts must be checked in the local language | Bulk payouts with beneficiary verification, payment links, split between FPO and members |
| SHG and microfinance collections | Weekly collections, group-to-member disbursement, internal loans | Cash meetings, a notebook, a bank-linked passbook | Group leader is the operator; records are in the local script | Subscriptions and mandates for recurring savings, payouts, collection links |
| Domestic workers' salaries | Monthly salary, advances, split across employers | Cash or UPI to the worker | Worker cannot read confirmations; employer wants a schedule and proof | Scheduled payouts, subscriptions, verification |
| Migrant remittances | Send home, schedule, verify receiver | Hawala, cash courier, UPI to a relative's account | Sender may read Bengali, Tamil or Hindi, not English; receiver may have a basic phone | Payouts, UPI/account verification, scheduled transfers, voice or WhatsApp trigger |
| Senior citizens | Pay bills, pay family, avoid scams | Ask children for help | Fear of collect-request and OTP scams; need a confirmation voice | Fraud-protective pre-payment checks, delegated payments, mandates view |
| Tuition and school fees | Collect, instalments, refund | Cash or bank deposit | Parents prefer vernacular reminders | Payment links, subscriptions for instalments, refunds |
| RWA and housing society dues | Collect, reconcile, send reminders | Treasurer's Excel and WhatsApp | Mixed-language communities | Payment links, auto-reconciliation, split |
| Small clinics and freelancers | Invoice, collect, follow up | Verbal quote plus UPI | Invoicing tools are English-only | Payment links and invoices, reminders |
| Temple and community donations | Collect, receipts | QR and cash box | Receipt formalities | Payment links, receipts |
| Chit funds and rotating savings | Monthly contribution, auction payout, trust and record keeping | Cash and a notebook with an organiser | High trust dependence and a regulatory grey area | Subscriptions, payouts, verification (compliance review needed) |

### Gaps
- No sourced sizes for: kirana count, udhaar volumes, street vendors (PM SVANidhi), farmers and FPOs, SHG members and savings (NRLM/NABARD), domestic workers, current migrant counts and remittance flows, senior citizens, tuition/RWA/clinic/freelancer/donation markets, chit-fund size.
- No sourced evidence that these segments want a voice agent. Any claim of "willingness to adopt" is an inference.
- The gig-worker figure is from the 2022 NITI Aayog baseline. Later numbers (for example e-Shram platform registrations or the Code on Social Security gig provisions) were not retrieved.

## 3. Government and industry initiatives that make this timely

### Takeaway
NPCI is actively building voice, multilingual and agent-delegation infrastructure (Hello UPI, UPI Help, FiMI, agent payment pilot). Bharat Connect is on WhatsApp. These show the direction but leave the money-moving agent layer for third parties.

### Cited Findings
- Hello UPI voice payments across 10 languages, per NPCI's ED. — [MediaNama](https://www.medianama.com/2026/02/223-fimi-npci-in-house-ai-model-upi-ecosystem/)
- UPI Help and mandate management are live via bank chatbots and DigiSathi. — [Angel One](https://www.angelone.in/news/personal-finance/npci-launches-ai-powered-upi-help-to-assist-users-with-digital-payments)
- A Bharat Connect overview deck exists on the official site (not read). — [Bharat Connect](https://www.bharat-connect.com/strapi/uploads/Bharat_Connect_Overview_Deck_for_website_29c4340e4a.pdf)
- UPI Circle was named in search results (Zeta explainer, not read). — [Zeta](https://www.zeta.tech/us/?p=7329)

### Inferences
- UPI Circle (delegated payments) is the closest regulatory primitive for a senior-citizen, domestic-worker or family-remittance agent that pays on behalf of someone. Its limits and current status need verification.
- Bharat Connect on WhatsApp makes WhatsApp a credible channel for bill payment for vernacular users.

### Gaps
- Not verified: UPI Circle limits and adoption, ONDC volumes, Account Aggregator numbers, Digital Payment Intelligence Platform status, RBI vernacular push specifics, RBI MuleHunter and fraud-reporting rules, and the status of the agentic-payments pilot after Oct 2025.
- No source fetched for Cashfree's own product details. The capability mapping above uses generic category names (payment links, payouts, subscriptions/mandates, split, verification) and should be checked against Cashfree docs.

## Suggested next steps for a follow-up pass
Fetch the NPCI UPI statistics page (P2M/P2P split), the RBI Annual Report FY26 on frauds, the NABARD SHG status report, the PM SVANidhi dashboard, TRAI's latest subscriber report, World Bank Findex 2025, and NPCI circulars on UPI Circle and agent payments.
