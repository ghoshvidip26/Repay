# Sarvam AI capabilities for the Sarvam x Cashfree buildathon (state as of 27 Oct 2026)

Research budget note: Firecrawl hit its rate limit near the end; the "Building for India" page, Medianama critique and streaming-STT page could not be read. These are listed under Gaps. Docs pages were served from cache dated 26-27 Oct 2026, so they are current.

## 1. Which Sarvam models exist now, languages, latency, streaming, code-mixing

### Takeaway
The current lineup is Saaras v4 (STT, default), Bulbul v4 Flash and v3 (TTS), Sarvam-105B and sarvam-105b-conversations (chat), Mayura (text translation), Sarvam Vision (document AI). Sarvam-M and Sarvam-30B are deprecated and no longer served. Saarika did not appear in the current model guide; treat it as folded into Saaras (unverified).

### Cited Findings
- Model guide lists Saaras v4 (default STT, 23 languages = 22 Indian + Global/Indian English, modes: transcribe, translate, verbatim, translit, codemix), Saaras v3 (still available), Bulbul v4 Flash (latest TTS, 200+ persona voices), Bulbul v3 (API default, 30+ speakers). Sarvam-M (24B) and Sarvam-30B are marked Deprecated, "migrate to Sarvam-105B" — [Sarvam models](https://docs.sarvam.ai/api/getting-started/models)
- Saaras languages (codes): Hindi hi-IN, Bengali bn-IN, Kannada kn-IN, Tamil ta-IN, Telugu te-IN, Malayalam, Marathi, Odia, Punjabi, Gujarati, English en-IN, plus Assamese, Urdu, Nepali, Konkani, Kashmiri, Sindhi, Sanskrit, Santali, Manipuri, Bodo, Maithili, Dogri. Language code optional; omit or "unknown" for auto-detect with `language_probability` returned — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- Saaras modes demo on "मेरा फोन नंबर है 9840950950": transcribe gives digits normalised; verbatim keeps spoken digits as words; codemix gives "मेरा phone number है 9840950950"; translit gives Roman script; translate gives English only — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- Saaras transports: REST (max 30 s audio), Batch (up to 2 h per file, diarization), WebSocket streaming. Formats WAV, MP3, AAC, AIFF, OGG, OPUS, FLAC, MP4, AMR, WMA, WebM; raw PCM must be 16 kHz and needs `input_audio_codec`. Optimised for 8 kHz telephony audio — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- Saaras v4 supports Keyterm Prompting: up to 50 names/brands/terms to bias recognition; accepted on REST and Batch only, not on streaming, not on v3 — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- Timestamps are chunk (sentence/phrase) level only; no word-level timestamps — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- Bulbul v4 Flash: 10 languages (Hindi, English, English-Hindi `enhi`, Bengali, Gujarati, Kannada, Marathi, Punjabi, Tamil, Telugu); Malayalam and Odia not on v4 Flash. 222 persona voice IDs like `simran_en_customer`; default `shubh_enhi_ads`; v3 speaker names not valid on v4 — [Bulbul](https://docs.sarvam.ai/api/getting-started/models/bulbul)
- Bulbul v3: 11 languages (10 Indian + English incl. Malayalam, Odia), 30+ speakers, default `shubh`. Both: REST, HTTP streaming, WebSocket; 2,500 chars per REST request; pace 0.5-2.0; sample rates 8-48 kHz REST, 8-24 kHz streaming — [Bulbul](https://docs.sarvam.ai/api/getting-started/models/bulbul)
- Bulbul limits: no SSML; romanised Indic input "significantly reduces output quality" (use native script, e.g. "आपका order confirm हो गया है"); `opus` codec currently errors, use mp3 — [Bulbul](https://docs.sarvam.ai/api/getting-started/models/bulbul)
- Docs banner: "Bulbul v4 Flash is here — our fastest voice model, half price until Nov 12" (so the promo ends before the 21 Nov event; v4 Flash then reverts to Rs 1/1k chars) — [Sarvam docs home](https://docs.sarvam.ai)
- Bulbul V3 reaches end of life on Voice Agents on 10 Nov 2026 (Voice Agents product only; API v3 still listed as active) — [Sarvam docs changelog block](https://docs.sarvam.ai)
- Sarvam-105B: MoE, 105B+ params, 128 experts, Apache 2.0, OpenAI-compatible, streaming. Languages: 10 most-spoken Indian languages + English, native script, romanised and code-mixed input. Context 128K (`sarvam-105b`) / 32K (`sarvam-105b-conversations`, post-trained for real-time dialogue and voice agents, /v1 endpoint only). Claimed Tau2 68.3 avg (agentic/tool use), wins 90% of pairwise Indic comparisons (vendor claim) — [Sarvam-105B](https://docs.sarvam.ai/api/getting-started/models/sarvam-105b)
- Thinking mode is ON by default (`reasoning_effort` low); reasoning tokens consume `max_tokens` and add latency; set `reasoning_effort=None` to disable (important for voice latency). max_tokens: Starter 4096 / Pro 16384 / Business 128000. Temperature default 0.5 with reasoning, 0.2 without — [Sarvam-105B](https://docs.sarvam.ai/api/getting-started/models/sarvam-105b)
- Other hosted chat models via same API: Gemma 4 31B (text+image, 131K), GLM-5.3, DeepSeek V4.1 Flash, plus beta DeepSeek V4 Pro, Kimi K3, MiMo V2.6 — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing); [Chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview)
- Mayura: translation between 11 languages (10 Indian + English) with context preservation; pricing lists "Translate (formal and colloquial)" and transliteration and language-ID endpoints. "Sarvam-Translate" as a separate model was not found on the current docs home — [Docs home](https://docs.sarvam.ai)
- Sarvam Vision: 3B-parameter multimodal doc model, 23 languages; inputs PDF, PNG, JPG, ZIP of images; max 10 pages per PDF, 10 images per ZIP, 200 MB; outputs HTML/Markdown (zip) plus JSON page-level data; async job (create, poll, fetch); 10 req/min on all plans — [Sarvam Vision](https://docs.sarvam.ai/api/getting-started/models/sarvam-vision)
- Document Translation API: PDF/Office files to up to 12 Indian languages per job, layout preserved — [Docs home](https://docs.sarvam.ai)
- Latency: no hard numeric latency figures found in the pages read. Docs say sessions sharing a session ID are routed together and reuse cached prompt context to lower latency on later turns; Voice Agents claim "no external network hops between stages" — [Chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview); [Voice Agents](https://docs.sarvam.ai/voice-agents/overview)

### Inferences
- For Hindi, Tamil, Kannada, Bengali all of Saaras v4, Bulbul v4 Flash and Sarvam-105B are covered; the four target languages are safe on every layer.
- For a voice loop, use `sarvam-105b-conversations` with reasoning disabled; default reasoning will hurt latency.
- Date-sensitive: the v4 Flash half-price promo and Bulbul v3 voice-agent EOL fall before the event.

### Gaps
- No published numeric latency (ms) for STT streaming, TTS first-byte or LLM TTFT found; measure on the day.
- Saarika status and a distinct "Sarvam-Translate" model not confirmed in current docs.
- Streaming STT page (VAD, interim results) could not be read (rate limit).

## 2. Function calling, structured output, agent products, telephony, WhatsApp

### Takeaway
Yes on all counts. The chat API supports OpenAI-style `tools`/`tool_choice` and `response_format` JSON schema; Sarvam sells a no-code/low-code Voice Agents platform with tools (webhook-style), telephony and chat; LiveKit, Pipecat, Vapi, Twilio, Exotel, LangChain, Vercel AI SDK and n8n integrations and a WhatsApp bot cookbook exist.

### Cited Findings
- Chat Completions supports `tools` with `tool_choice` auto/none/required/specific function; `function.arguments` is a JSON string to parse and validate; `parallel_tool_calls:false` not enforced on Responses API — [Chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview)
- Structured output: `response_format` `json_schema` or `json_object`; docs advise validating parsed JSON with pydantic/zod before acting — [Chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview)
- Streaming via `stream:true` (SSE); reasoning effort low/medium/high — [Chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview)
- Voice Agents (Samvaad): build, test, deploy, monitor agents in 11 Indian languages (Hindi, Tamil, Telugu, Bengali, Marathi, Gujarati, Kannada, Malayalam, Punjabi, Odia, Assamese + English) "from Rs 3 a minute"; agents call your systems (CRM, core banking, payments) through tools; inbound and outbound phone GA; bring your own Exotel/Twilio/Plivo or rent a number; 60-minute max call; simulated-call tests with AI judge; campaigns; analytics API — [Voice Agents](https://docs.sarvam.ai/voice-agents/overview); [Docs home](https://docs.sarvam.ai)
- WhatsApp and text/chat agents on Voice Agents are "available to enterprise customers on request" — not self-serve — [Voice Agents](https://docs.sarvam.ai/voice-agents/overview)
- Integration guides: LiveKit, Pipecat, Vapi (Saaras v3 + Sarvam-105B + Bulbul v3), Twilio (phone and WhatsApp bots), Exotel (via Pipecat), LangChain (`langchain-sarvam`: streaming, tools, structured output), Vercel AI SDK (`sarvam-ai-sdk`), n8n community node. Also an MCP server and Agent Skills for coding assistants — [Docs home](https://docs.sarvam.ai)
- Cookbook recipes with full source: WhatsApp bot (speech + chat, answers in user's language), call analytics pipeline, live transcription, podcast generator, support voice notes — [Docs home](https://docs.sarvam.ai); GitHub: https://github.com/sarvamai/sarvam-ai-cookbook
- Pipecat Sarvam TTS service supports bulbul:v3 (pace 0.5-2.0, temperature 0.01-1.0, 24 kHz default) and v4-flash; bulbul:v2 now returns a deprecation error — [Pipecat reference](https://reference-server.pipecat.ai/en/stable/api/pipecat.services.sarvam.tts.html)
- Official Python and JS SDKs; REST, WebSocket and batch endpoints — [Docs home](https://docs.sarvam.ai)

### Inferences
- A 6-hour build is realistic using SDK + OpenAI-style tool calls (our own orchestrator calling Cashfree), or Pipecat/LiveKit for a real-time voice loop. Voice Agents platform is viable for phone demo but the tools are configured on Sarvam's side; check whether it can hit a public Cashfree-backed webhook (docs page on tools not read).
- WhatsApp: self-serve path is Twilio/Meta Cloud API + Sarvam APIs (cookbook), not Sarvam's managed WhatsApp.

### Gaps
- Voice Agents "tools" page, per-minute price breakdown (the Rs 3/min figure is "from"), and whether free credits cover Voice Agents not read.
- Exact function-calling reliability in Indic languages: only vendor benchmark (Tau2) found.

## 3. Limits, pricing, credits (what 500 credits buys)

### Takeaway
Pay-as-you-go rates are very cheap; the binding constraint is rate limits on the free/PAYG tier (60 req/min STT REST, 20 concurrent STT WebSockets, 60 req/min on 105B), which matter with 30 teams demoing. The unit of "500 Sarvam credits" is unconfirmed.

### Cited Findings
- Sarvam's own signup grant is "Rs 100 worth of free credits" per verified user; credits are universal across APIs and never expire; exhausted credits return errors — [Rate limits](https://docs.sarvam.ai/api/getting-started/ratelimits)
- Plans: PAYG free, Pro Rs 9,999/mo, Growth Rs 49,999, Business Rs 1,99,999 (includes equal credits), Enterprise custom. Prices exclude GST — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing)
- STT: Rs 0.49/min PAYG (billed per second); diarization batch Rs 0.59/min — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing)
- TTS per 1,000 chars: Bulbul v4 Flash Rs 1.00 (half price promo to 12 Nov per banner), Bulbul v3 Rs 3.00, voice cloning Rs 2.00 — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing)
- LLM per 1M tokens: Sarvam-105B and 105B-Chat input Rs 15, cached Rs 5, output Rs 60 (reasoning billed as output); Gemma 4 31B Rs 35/90; DeepSeek V4.1 Flash Rs 25/105 — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing)
- Text translate Rs 2/1k chars; transliteration Rs 2; language ID Rs 0.35; Document Digitization Rs 0.50/page; Document Extraction Rs 1.00/page (max 10 pages per job); Document Translation Rs 5/1k chars per target language — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing)
- PAYG rate limits: STT real-time REST 60 req/min; STT WebSocket 20 concurrent; STT batch 20 req/min; TTS REST 60 req/min and 60 concurrent WebSockets; 105B 60 req/min; other chat 50 req/min; translate 60 req/min; Document AI digitise 200 req/min, extract 30 req/min; Sarvam Vision 10 req/min. Limits are per account, not per key. 429/503 need exponential backoff — [Rate limits](https://docs.sarvam.ai/api/getting-started/ratelimits); [Sarvam Vision](https://docs.sarvam.ai/api/getting-started/models/sarvam-vision)
- WebSocket limiter reacts to connection burst rate; rejects with close code 1003; space new connections ~300 ms apart — [Rate limits](https://docs.sarvam.ai/api/getting-started/ratelimits)
- Commercial rights for generated audio require a licence (Commercial Licensing page) — [Pricing](https://docs.sarvam.ai/api/getting-started/pricing)

### Inferences (arithmetic, assuming 500 credits = Rs 500 at PAYG; UNVERIFIED)
- A 20-second voice turn costs roughly: STT ~Rs 0.16 + TTS ~200 chars on v4 Flash ~Rs 0.20 (Rs 0.60 on v3) + LLM ~2k in/300 out ~Rs 0.05 = about Rs 0.4-0.9 per turn, so ~500-1,200 turns per builder, ample for a day; 10-page document extraction is Rs 10, so ~50 such docs.
- If "500 credits" is a different unit (e.g. buildathon-specific), numbers change; confirm on Discord. Also unknown whether the credits are applied to rate-limit tier (probably still PAYG limits).
- Pool credits across a team of 5 (2,500) and keep a separate key per teammate is pointless because limits are per account; if the team uses one account the 60 req/min cap is shared by five people.

### Gaps
- Meaning of "500 Sarvam credits" and how they are issued (coupon, org-level top-up) not found publicly.
- Whether event credits raise rate limits not stated.

## 4. Sarvam announcements on payments, commerce, UPI voice

### Takeaway
Sarvam has a live Razorpay partnership (announced March 2026) for voice-first commerce inside its Indus app with Swiggy as first merchant, using NPCI UPI Reserve Pay. Nothing found tying Sarvam to Cashfree outside this buildathon.

### Cited Findings
- Razorpay x Sarvam: customers discover, order and pay by conversing in their language; first in Sarvam's Indus App with Swiggy ordering food; payment via Razorpay agentic payments "powered by NPCI's UPI Reserve Pay" — [Razorpay newsroom](https://razorpay.com/newsroom/razorpay-partners-with-sarvam-to-power-voice-first-conversational-commerce-for-india/); [Sarvam page](https://www.sarvam.ai/partnerships/razorpay)
- Medianama summary: agent completes food orders and payments "without a PIN at checkout"; notes the demo does not explain how Sarvam's conversational layer connects to Razorpay infrastructure — [Medianama](https://www.medianama.com/2026/03/223-razorpay-sarvam-ai-ai-agent-payments-indus-app/) (snippet only)
- Related: Razorpay and NPCI announced Agentic Payments on Claude at India AI Impact Summit, 20 Feb 2026 — [Razorpay blog](https://razorpay.com/blog/agentic-payments-and-npci/)
- Buildathon post (Cashfree, Facebook): "The Cashfree x Sarvam Buildathon is here. 150 builders. Sat, 21 Nov ... Rs 3,00,000+ prize pool* Build AI agents that move real money" — [Cashfree Facebook post](https://www.facebook.com/cashfreepayments/posts/builders-this-ones-for-you-%EF%B8%8Fthe-cashfree-sarvam-buildathon-is-here150-builders-1/1493483572826426/) (search snippet only; page not fetchable)
- Separate Sarvam Epoch Buildathon by GrowthX (8-hour, Rs 10L pool, Rs 5,000 AI credits) exists; do not confuse it with this event — [StartupGrantsIndia](https://www.startupgrantsindia.com/sarvam-epoch-buildathon-by-growthx)

### Inferences
- "Moves money" with Cashfree should be built as a Cashfree-side flow (payouts, payment links, UPI collect/QR) with human-confirmation, since Sarvam provides no payment rails itself. Judges likely know the Razorpay/Indus precedent, so differentiation (Cashfree payouts, vendor/gig-worker use cases) matters.
- Cashfree API specifics are out of this note's scope.

### Gaps
- No NPCI or UPI-voice (e.g. UPI 123PAY, Hello UPI) tie-in found for Sarvam; Luma page and Discord content were not accessible, so selection criteria and judging rubric are unverified beyond the brief.

## 5. Known weaknesses a payments agent must handle

### Takeaway
Docs explicitly flag only structural limits, not accuracy rates on amounts. Numbers, names and noisy audio are therefore unquantified and must be handled by design (read-back, confirmation, validation).

### Cited Findings
- Saaras normalises numbers in transcribe mode; `verbatim` keeps spoken digits unnormalised; `codemix` preserves English words — useful for cross-checking amounts by running two modes — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- Saaras claims "Intelligent Entity Preservation" for proper nouns and regional names, and keyterm prompting (up to 50) for names/brands — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- 30 s cap on real-time REST STT; translate mode outputs English only — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)
- TTS: no SSML, so digit-by-digit reading of amounts/account numbers cannot be forced with tags; romanised text degrades audio — [Bulbul](https://docs.sarvam.ai/api/getting-started/models/bulbul)
- LLM output: `function.arguments` is a string that must be parsed and validated; reasoning tokens can exhaust `max_tokens` leaving empty reply — [Chat overview](https://docs.sarvam.ai/api/api-guides-tutorials/chat-completion/overview); [Sarvam-105B](https://docs.sarvam.ai/api/getting-started/models/sarvam-105b)
- Sarvam's v2.5 translation benchmark (COMET ~88-92 across languages, Vistaar+IndicVoices incl. noisy, code-mixed audio) is a translation-quality metric, not a number-accuracy metric — [Saaras](https://docs.sarvam.ai/api/getting-started/models/saaras)

### Inferences
- Design pattern: never let the LLM free-form an amount. Extract amount/payee via JSON schema, parse numerals in code, cross-check transcribe vs verbatim, read back in TTS (in native script, digits spelled as words in the target language), and require explicit "haan/aamam/houdu/hyaa" confirmation or a button/OTP before calling Cashfree. Use keyterms for payee names and a pre-registered beneficiary list with fuzzy match.
- Indian number words (lakh, crore, hazaar, "dedh", "dhai", Tamil/Kannada/Bengali equivalents) and homophones like "pandrah/pachaas" are plausible failure points; build a test set per language before demo. This is reasoning, not a documented Sarvam fault.
- Noisy hall at a 150-person event: use push-to-talk, headset mic, 16 kHz clean input; telephony 8 kHz is explicitly supported.

### Gaps
- "Building for Indian languages" page (likely the most relevant guidance on numbers/names) could not be read.
- No independent third-party accuracy tests for amount/name transcription found.

## Realistic 6-hour scope (synthesis)
- Feasible: (a) voice or chat in hi/ta/kn/bn -> Saaras v4 (codemix + keyterms) -> sarvam-105b(-conversations) with tool calls -> Cashfree sandbox payout/payment link -> Bulbul v4 Flash confirmation; (b) document input via Sarvam Vision (10-page cap, 10 req/min) to extract invoice fields and then propose payment; (c) WhatsApp via Twilio/Meta + cookbook, with audio notes under 30 s for REST STT.
- Risky in 6 hours: Sarvam managed WhatsApp (enterprise-only), telephony number setup, streaming barge-in, many simultaneous WebSockets on PAYG limits.
- Event context: Sat 21 Nov 2026, 11:30-19:00, Cashfree Office, Ecoworld, Bengaluru; 150 builders, 30 teams of 5; Rs 3,00,000+ cash and credits; 500 Sarvam credits per registered builder; Discord participation affects selection (all per the brief; only the 150/21 Nov/Rs 3L/"move real money" items corroborated by the Cashfree post snippet).
- Outdated-risk flags: Bulbul v4 Flash promo ends 12 Nov; Bulbul v3 EOL for Voice Agents 10 Nov; pricing and model names change often; Voice Agents changelog shows monthly changes.
