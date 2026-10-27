# Prior art: voice / vernacular / agentic payments and hackathon landscape in India (state as of 27 Oct 2026)

## 1. What exists today (voice, vernacular, agentic payments)

### Takeaway
The big incumbents already ship or pilot the obvious ideas: voice-to-pay inside UPI apps (PhonePe, Google Pay, NPCI Hello UPI), voice food/grocery ordering with UPI payment (Razorpay x Sarvam x Swiggy), UPI inside ChatGPT/Gemini (NPCI x Razorpay x OpenAI/Google), and agent-pays protocols (Pine Labs P3P, NPCI Unified Agent Protocol). A generic "talk to an agent in Hindi and it pays by UPI" demo is commonplace as of Oct 2026.

### Cited Findings
- NPCI Hello! UPI (voice-activated UPI: onboarding, balance enquiry, voice-initiated payments with "Hello" wake word, 123Pay on-call voice payments) was announced Oct 2023, initially Hindi and English with regional languages planned; the source says availability was projected, not confirmed live; UPI PIN still entered manually — [Outlook Money, 3 Nov 2023](https://www.outlookmoney.com/banking/ask-and-pay-your-upi-will-work-like-alexa-soon)
- PhonePe rolled out AI-powered natural-language voice/text search on 20 Feb 2026, built with Microsoft Foundry; intent-based routing (pay a contact, FASTag recharge, gold price), hybrid on-device/cloud, phased rollout; languages not stated — [Storyboard18, 20 Feb 2026](https://storyboard18.com/digital/phonepe-rolls-out-ai-powered-voice-and-text-payments-feature-90206.htm)
- Google Pay announced AI-powered voice payments for UPI in India, to be developed with government's Bhashini project, phased rollout, exact date not disclosed in the fetched article (article undated; treat date as unknown) — [TYN Magazine](https://tynmagazine.com/google-pay-to-introduce-ai-powered-voice-payments-in-india/)
- PhonePe launched UPI payments for feature-phone users (headline only; date not verified) — [Inc42](https://inc42.com/buzz/phonepe-launches-upi-payments-for-feature-phones/)
- 9 Oct 2025 (GFF 2025): Razorpay, NPCI and OpenAI launched an "Agentic Payments" pilot (UPI payments inside ChatGPT) using UPI Circle and UPI Reserve Pay; partner banks Axis Bank and Airtel Payments Bank; BigBasket first merchant; agent transacts only after explicit user approval — [Business Today, 9 Oct 2025](https://www.businesstoday.in/amp/tech-today/news/story/razorpay-npci-openai-launch-pilot-for-agentic-payments-where-ai-powered-shopping-meets-upi-497545-2025-10-09)
- 8 Oct 2025: NPCI live-demoed agentic UPI with Gemini, BigBasket, Google Pay, HDFC credit card, Razorpay, Axis Bank (recipe scaled for 10 people, ingredients ordered and paid in one flow, voice/text triggered); NPCI framed UPI as an "intelligent commerce layer" — [Inc42, 8 Oct 2025](https://inc42.com/buzz/npci-piloting-agentic-ai-based-payments-with-upi)
- 12 Mar (FTX26, 2026 per context) Razorpay demoed voice ordering with Zomato, payment completed without opening checkout or UPI app, expected live within a month; year not stated in the fetched text — [LetsDataScience](https://www.letsdatascience.com/news/razorpay-demonstrates-agentic-payments-with-zomato-d4361389)
- 23 Mar 2026: Razorpay + Sarvam launched voice-based food ordering with Swiggy inside Sarvam's Indus app (Hindi, Hinglish, "other Indian languages"), a voice assistant pilot on The Derma Co website, and Sarvam stack integrated into Razorpay Agent Studio; payments via NPCI UPI Reserve Pay — [Razorpay newsroom](https://newsroom.razorpay.in/newsroom/razorpay-partners-with-sarvam-to-power-voice-first-conversational-commerce-for-india/)
- MediaNama review of the demo (25-26 Mar 2026): mandate set once with UPI PIN via Reserve Pay (Single Block Multi Debit), up to Rs 10,000 per mandate, valid up to 90 days, revocable; Sarvam-to-Razorpay handoff via MCP; user still taps "Proceed to Pay" in demo; Razorpay CEO said rollout with 20+ partners; open questions on voice-error handling, fallback, voice data storage; liability: merchant handles disputes, Razorpay payment security, Sarvam no financial liability — [MediaNama, 25 Mar 2026](https://www.medianama.com/2026/03/223-razorpay-sarvam-ai-ai-agent-payments-indus-app/)
- 25 Mar 2026: Swiggy-Sarvam partnership for multilingual voice ordering across food delivery, Instamart and Dineout; Sarvam voice models support 11 Indian languages (Hindi, Tamil, Telugu, Kannada, Bengali, Marathi named) — [Storyboard18, 26 Mar 2026](https://www.storyboard18.com/amp/digital/swiggy-partners-sarvam-to-enable-multilingual-voice-ordering-across-services-ws-l-93251.htm)
- Razorpay launched an MCP server for AI payment integration (headline only; date not verified) — [CRN](https://www.crn.in/?p=71134); Razorpay Agent Studio also exists — [Fintech News SG](https://fintechnews.sg/127652/fintech-india/razorpay-agent-studio-ai-payments-claude/)
- 16 Jun 2026: Pine Labs launched P3P, agentic payments protocol on UPI (extends One Time Mandates and Reserve Pay; Grantex for identity/delegated authorisation/spend limits/audit; HTTP 402 for payment discovery); only live partner Gullak (gold buy on price trigger), Vijay Sales proof of concept; company claims, no regulator approval cited — [The Paypers, 16 Jun 2026](https://thepaypers.com/payments/news/pine-labs-launches-p3p-agentic-payment-protocol-on-upi)
- 1-3 Sep 2026 (Reuters via Deccan Chronicle / Inc42): NPCI reportedly preparing "Unified Agent Protocol" for small UPI payments by AI agents without per-transaction approval, built on UPI Circle + Reserve Pay; planned spending limits, identity checks, audit trails; liability framework planned but undefined; Mastercard completed first authenticated agentic transaction in New Delhi in June 2026; NPCI declined comment — [Deccan Chronicle](https://www.deccanchronicle.com/amp/technology/india-preparing-framework-to-let-ai-agents-make-small-upi-payments-without-approval-1983986), [Inc42, 1 Sep 2026](https://inc42.com/buzz/npci-to-launch-agentic-payments-on-upi-report/)
- GFF 2026 (Mumbai, 8-11 Sep 2026): NPCI announced AiNxt (open-source agentic AI platform, BYOM) and AtOM (agentic orchestration for UPI partner onboarding/integration); Credit Line on UPI extended to KCC and PM MUDRA; Unified Agent Protocol described as "expected", launch not confirmed in that article; no voice announcement — [Inc42, 9 Sep 2026](https://inc42.com/buzz/gff-2026-npci-launches-instant-card-issuing-atms-deepens-ai-push/)
- Startup layer: Bolna (Bengaluru, YC) raised $6.3M seed led by General Catalyst (21 Jan 2026); voice agents in 10+ Indian languages for sales, support, collections, recruitment; payments not listed as a specific use case — [Inc42, 21 Jan 2026](https://inc42.com/?p=545778)
- WhatsApp in-chat payments for India (UPI) and WhatsApp Flows launched Sept 2023 (per headlines) — [TechCrunch, 19 Sep 2023](https://techcrunch.com/2023/09/19/whatsapp-india-payments-businesses/amp/)

### Inferences
- "Voice agent + UPI + Indian language + food/grocery order" is the exact flagship demo of Razorpay x Sarvam x Swiggy/Zomato; judges from those sponsors have seen it.
- Agent-with-mandate (Reserve Pay / UPI Circle) is now the standard primitive; NPCI itself plans the authoritative protocol, so a hackathon team re-inventing delegation/mandate logic is duplicating a roadmap item.
- Whitespace appears to be what these players do NOT show: voice-error handling/fallback, consent and audit UX in vernacular, dispute/liability flows, voice data storage, merchant-side (not consumer) voice, and verticals beyond food/grocery/gold.

### Gaps
- Could not verify Juspay, Setu, Cashfree, Kiko, Gupshup, Haptik, Yellow.ai, Vapi-style product specifics for voice/agentic payments; no sources fetched (searches returned nothing relevant). Do not assume absence.
- Google Pay voice payments launch date and languages; Hello UPI actual live status in 2026 not confirmed.
- Whether NPCI formally launched Unified Agent Protocol after 9 Sep 2026: not confirmed as of sources found.

## 2. Crowded hackathon project types (Sarvam + payments)

### Takeaway
No source enumerating "typical hackathon submissions" was found, so the crowded list below is inference from what incumbents ship plus Sarvam's published tracks, not a counted survey.

### Cited Findings
- Sarvam BuildIn' Hours (Zo House Bengaluru, announced 18 Aug 2026, run by HackCulture): tracks Voice AI, Document Intelligence, Multilingual AI, AI Agents; 100 selected from 6,272 applications; Rs 5,000 Sarvam credits each; mentors from Sarvam, IBM, Qdrant, Razorpay, Google for Startups — [Digital Terminal](https://digitalterminal.in/tech-companies/zo-house-bengaluru-hosts-sarvam-ai-hackathon)
- Sarvam Epoch buildathon by GrowthX, 26 Jul 2026, 8 hours at Razorpay's Arena office, Bengaluru; Rs 10 lakh pool; themes Voice agents, Document intelligence, Multilingual AI products; top 10 demo at Sarvam Epoch on 30 Jul — [Startup Grants India](https://www.startupgrantsindia.com/sarvam-epoch-buildathon-by-growthx)
- Sponsor-published themes (Voice, Document, Multilingual, Agents) map directly onto the commodity ideas below.

### Inferences (flag as crowded)
- Voice UPI bot / "pay by speaking Hindi": crowded (shipped by PhonePe, Google Pay, NPCI Hello UPI, Razorpay x Sarvam).
- Voice food/grocery ordering agent with Razorpay/UPI checkout: crowded (Swiggy/Zomato demos).
- UPI-in-chatbot (ChatGPT/Gemini-style shopping agent): crowded (NPCI pilots Oct 2025).
- WhatsApp payment-link bot: crowded (WhatsApp native payments since 2023; Razorpay/Cashfree payment links are commodity APIs).
- Bill-split bot, expense tracker with voice: likely crowded but unverified.
- Invoice/receipt reader (Document Intelligence track, Sarvam Vision): likely crowded given an explicit track.
- Generic "agent with spending limit/mandate": being standardised by NPCI/Pine Labs.

### Gaps
- No Devpost/Devfolio/GitHub submission counts retrieved; "crowded" for bill-split and invoice reader is judgment, not data.

## 3. Winners and notable projects (2025-2026)

### Takeaway
Only one 2026 Sarvam-sponsored result was found, and winners were NOT payments projects: they were social-impact / MSME-access tools built on Sarvam Vision and Indic languages. Winners from Cashfree, Razorpay, NPCI, Juspay, Google hackathons were not found with project descriptions.

### Cited Findings
- Sarvam BuildIn' Hours (Aug 2026) winners: 1st Government Tender Discovery and Application for MSMEs (built on Sarvam Vision, working prototype); 2nd Storytelling for Kids in Indic Languages; 3rd Setu (visual communication aid for non-speaking people); Rs 5 lakh pool, jury incl. creator Arsh Goyal — [Digital Terminal](https://digitalterminal.in/tech-companies/zo-house-bengaluru-hosts-sarvam-ai-hackathon)
- Cashfree SwitchOn 2024 (internal 48-hour hackathon, 200+ participants, 43 mentors; judged by CTO, CFO, SVP Product, and the two founders); winners listed by name only, projects not described — [Cashfree blog](https://www.cashfree.com/blog/switchon-2024/)
- NPCI x Citi DRUNIX Hackathon 2026: online, registration 10-30 Sep 2026, theme "Building the future of payments through Innovation x Scale x Impact"; selected innovators get NPCI API/infrastructure access; no results published — [Internshala](https://internshala.com/competitions/drunix-hackathon-2026-by-npci-x-citi/)
- AI for Bharat Hackathon (YourStory, June 2026) has a story on winning ideas "from crop insurance to conversational analytics"; page could not be fetched, content unread — [YourStory, Jun 2026](https://yourstory.com/2026/06/crop-insurance-conversational-analytics-ai-for-bharat-hackathon)
- NPCI PayAuth Challenge winner (2021, alternative UPI authorisation) was Tech5 — [NPCI PR](https://www.npci.org.in/PDF/npci/press-releases/2021/NPCI-announces-Tech5-as-the-winner-of-the-PayAuth-Challenge-for-developing-an-alternative-solution-for-authorising-UPI-payments.pdf) (older, headline only)

### Inferences
- The one datapoint suggests Sarvam judges rewarded underserved-user problems (MSMEs, kids, accessibility) with a working prototype over another payments bot.

### Gaps
- No winners found for Razorpay, Juspay, Google, Cashfree 2025-26 public hackathons, nor AI for Bharat project details. Winner-by-winner "why they won" is mostly unavailable.

## 4. Judging criteria used by sponsors

### Takeaway
No sponsor (Sarvam, Cashfree, NPCI) published formal rubrics in sources found. Only soft signals exist.

### Cited Findings
- NPCI DRUNIX: judged on "innovation, scalability, and real-world impact" (broad pillars, no weights) — [Internshala](https://internshala.com/competitions/drunix-hackathon-2026-by-npci-x-citi/)
- Sarvam Epoch buildathon page lists no judging criteria; reward is a demo slot before investors/founders and Rs 10 lakh pool — [Startup Grants India](https://www.startupgrantsindia.com/sarvam-epoch-buildathon-by-growthx)
- Cashfree SwitchOn 2024: teams pitched to C-suite and founders; no criteria stated — [Cashfree blog](https://www.cashfree.com/blog/switchon-2024/)
- Generic judging breakdown article exists (not India/sponsor-specific, not read in full) — [Reskilll](https://blogs.reskilll.com/what-hackathon-judges-look-for-complete-judging-criteria-breakdown-2026/)

### Inferences
- Candidate rubric (inference, not sourced): working live demo, real API depth (sandbox calls, not mocks), Indic-language quality incl. code-mixing, a specific user/segment, and a viability story. Sarvam's 8-12 hour formats favor working prototypes over decks.

### Gaps
- User's prompt hypotheses (real API depth, live demo, language quality, viability) are unconfirmed by any sponsor document.

## 5. Regulatory guardrails

### Takeaway
Agent payments must fit inside existing UPI mandate rails: PIN/biometric authentication moved to mandate set-up, with caps. Fully autonomous, unlimited, or non-UPI-compliant flows are not viable; liability is unresolved.

### Cited Findings
- RBI Authentication Mechanisms for Digital Payment Transactions Directions, 2025 effective 1 Apr 2026: risk-based authentication, at least one dynamic factor, OTP cannot be sole factor; card issuers had until 1 Oct 2026 for non-recurring cross-border CNP; for UPI, UPI PIN or biometric plus device binding (summarised by Pine Labs, secondary source, verify against RBI text) — [Pine Labs, 13 Apr 2026](https://www.pinelabs.com/blog/what-are-rbis-new-upi-rules-in-2026-latest-updates-impacts-benefits)
- Reserve Pay mandate currently capped at Rs 10,000, up to 90 days; caps may change for agents (Reuters) — [Deccan Chronicle, 1 Sep 2026](https://www.deccanchronicle.com/amp/technology/india-preparing-framework-to-let-ai-agents-make-small-upi-payments-without-approval-1983986), [MediaNama](https://www.medianama.com/2026/03/223-razorpay-sarvam-ai-ai-agent-payments-indus-app/)
- UPI Circle Full Delegation: Rs 5,000 per transaction, Rs 15,000 per month (per StartupFeed summary, 7 Sep 2026; could change for agents) — [StartupFeed](https://startupfeed.in/?p=11591)
- NPCI's intended role is narrow: verify payment requests are genuine, not what is bought; liability framework not yet defined — [StartupFeed](https://startupfeed.in/?p=11591)
- Razorpay: "UPI Reserve Pay doesn't skip authentication; it merely shifts it to the beginning of the journey" — [MediaNama](https://www.medianama.com/2026/03/223-razorpay-sarvam-ai-ai-agent-payments-indus-app/)
- Hello UPI: UPI PIN is still entered manually, not by voice — [Outlook Money](https://www.outlookmoney.com/banking/ask-and-pay-your-upi-will-work-like-alexa-soon)

### Inferences
- A hackathon demo must use sandbox/test mode (Razorpay, Cashfree test keys); live money movement needs PA/PG licensing, merchant KYC and NPCI/bank onboarding. Voice should never be the authentication factor itself (voice biometrics as AFA not supported by any source found).
- Anything claiming auto-debit above the mandate caps, voice-only PIN, or storing raw voice/PII without consent is likely non-viable or a judge red flag (DPDP angle unresearched).

### Gaps
- RBI primary text not read; UPI per-transaction limits (Rs 1 lakh general), RBI e-mandate Rs 15,000 AFA threshold, KYC/PPI rules and DPDP Act obligations not verified here.
- Whether the NPCI Unified Agent Protocol has final limits: unknown as of 27 Oct 2026.
