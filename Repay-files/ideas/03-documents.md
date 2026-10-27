# Theme 3: Documents - five winning ideas

Brief: voice, chat, documents or WhatsApp. Here the "need" arrives as a document (photo, PDF or screenshot) and the agent turns it into verified money movement. An invoice reader alone is crowded; every idea below makes the document the trigger for a **verify, approve, pay** gate.

Sandbox only. Scores are our inference. Cashfree and Sarvam facts come from the notes in `../research/`.

## Shared document rules

- **Sarvam Vision** reads documents in 23 languages; limits are a 10-page cap and 10 requests a minute per account, and large PDFs run as async jobs. Cache extraction results before the demo.
- Handwritten Indic text accuracy is unverified in the notes, so use **printed or typed** documents in the demo.
- Code, not the model, does every sum. Show the extracted fields on screen beside the original image so the user can catch an extraction error before paying.
- Never use real Aadhaar, PAN, GSTIN or bank data of real people or businesses.

---

## 1. Dadu Guard (Bengali) - scam check, then family-approved payment for seniors

- **Pitch:** A senior forwards a QR or payment request. The agent checks who the payee really is, explains in Bengali whether it looks safe, and pays only after a family member approves.
- **User and document:** Senior citizen with a daughter as approver. A screenshot of a QR code plus the voice question *"Ei QR code-ta kar? Ekhane panch hajar taka deoya nirapod ki?"* (Whose QR is this? Is it safe to pay Rs 5,000 here?)
- **How money moves:** UPI Penny Drop (`valid@ybl`, `invalid@ybl`, `expired@ybl`) plus Name Match decide the verdict. If the verdict is safe and the approver taps, Payouts V2 UPI transfer to `success@upi` (`failure@upi` for the failure run). UPI payouts cap at Rs 1 lakh.
- **Sarvam:** Sarvam Vision for screenshot text; the QR payload is decoded in code with a standard decoder (inference); Saaras v4 for the voice question; Bulbul v4 Flash Bengali; JSON-schema verdict from the 105B model.
- **Demo (3 steps):** (1) Senior speaks and uploads a QR image. (2) Agent shows "payee name does not match the claimed shop" (name match 38) and says so in Bengali. (3) A legitimate QR passes; a summary goes to the daughter, who taps Approve; payout runs.
- **Why it wins:** UPI fraud still runs 10.64 to 13.42 lakh incidents a year (FY24 to FY26 to Nov 2025). Incumbent voice work targets payment initiation, not pre-payment verification plus delegated approval. NPCI's UPI Help answers queries and manages mandates but does not verify a specific payee for the senior.
- **Feasibility:** 3/5 (three inputs, two approvers).
- **Risk and guardrails:** Name match can be wrong; the verdict is advisory ("unverified", never "safe"). Approver taps, voice never authorises. No real QR data of real people.

## 2. Supplier Safe-Pay (Tamil) - MSME invoice approval with a bank-detail fraud check

- **Pitch:** A small manufacturer photographs a supplier invoice. The agent extracts it, verifies GSTIN and bank account name, flags changed bank details against the vendor master, and pays only after approval.
- **User and document:** Coimbatore-style MSME owner with a printed invoice photo and the spoken request *"Indha invoice supplier-ukku panam seluthu, aana vangi vivaram sariya irukkannu paathuttu."*
- **How money moves:** Sarvam Vision extraction, Secure ID GSTIN (`29AAICP2912R1ZR` valid, `29AAIZP2912R1ZR` invalid), bank account verification and name match against the vendor master, then Payouts V2 Standard Transfer after approval.
- **Sarvam:** Sarvam Vision (PDF up to 10 pages, async), Saaras v4 Tamil, 105B JSON schema, Bulbul v4 Flash Tamil.
- **Demo (3 steps):** (1) Invoice photo parsed; Tamil summary read aloud. (2) Verification flags that the account name differs from the vendor master. (3) Owner approves corrected details; payout runs.
- **Why it wins:** The value is the gate, not the PDF reading. Lead with the mismatch screen.
- **Feasibility:** 4/5.
- **Risk and guardrails:** It reads as "another invoice reader" if the gate is not the star. No payment without a passing GSTIN and name score; sandbox only.

## 3. Mandi Parchi (Kannada) - weighbridge slip to farmer payout

- **Pitch:** A buyer or FPO clerk photographs a printed weighbridge or purchase slip. The agent reads the weight and rate, computes the amount payable, verifies the farmer's account, and pays.
- **User and document:** Mandi clerk. A printed slip photo plus the Kannada instruction *"Ee parchi pramaaNa hana haaki."* (Pay as per this slip.)
- **How money moves:** Same verify-then-pay chain as Raitha Payout: Secure ID bank verification plus Name Match, then Payouts V2 (single or batch) with approval. The amount comes from code applying the slip's weight and rate.
- **Sarvam:** Sarvam Vision for the printed slip (handwriting unverified), Saaras v4 Kannada, Bulbul v4 Flash Kannada readback.
- **Demo (3 steps):** (1) Clerk photographs three slips. (2) Extraction table appears beside each image; one weight is flagged as inconsistent with the slip total. (3) Clerk fixes it, taps Approve; payouts run.
- **Why it wins:** Document-first version of the farmer payout, which is the strongest payout story in the notes, with a visible extraction-to-payment chain.
- **Feasibility:** 3/5. Pick this or Raitha Payout, not both.
- **Risk and guardrails:** Extraction errors become payment errors. Side-by-side image and fields; flag any weight-times-rate mismatch; per-farmer cap.

## 4. Tender EMD Desk (Hindi) - tender notice to earnest-money payment

- **Pitch:** An MSME owner uploads a government tender notice. The agent reads the earnest money deposit (EMD) amount, deadline and the beneficiary account, verifies the beneficiary, and pays after approval.
- **User and document:** Small contractor. A PDF tender notice and the Hindi request *"Is tender ka EMD kitna hai aur kab tak bharna hai?"* (How much is the EMD and when is it due?)
- **How money moves:** Sarvam Vision extracts the EMD and account details; Secure ID bank verification and Name Match confirm the beneficiary; Payouts V2 by NEFT or RTGS after approval. EMD payment channels differ by tender portal, so treat this as an inference about how a real payment would be routed.
- **Sarvam:** Sarvam Vision (10-page cap), Saaras v4 Hindi, 105B JSON schema with the deadline, Bulbul v4 Flash Hindi.
- **Demo (3 steps):** (1) Owner uploads a sample tender PDF. (2) Agent states EMD amount, deadline and beneficiary, and verifies the account. (3) Owner approves; payout runs and the agent reads the status back.
- **Why it wins:** It builds on the one 2026 Sarvam hackathon winner the notes found (an MSME tender discovery tool on Sarvam Vision, one data point not a rubric) and adds the missing step: moving the money.
- **Feasibility:** 3/5.
- **Risk and guardrails:** Real tender payment rules vary by portal; use a synthetic tender. Deadline shown prominently; payment to government accounts is not claimed to be supported.

## 5. Muster Roll Reader (Hindi) - paper attendance sheet to wage payouts

- **Pitch:** A contractor photographs the site's printed attendance sheet. The agent computes wages from days times rate, flags arithmetic mismatches, and pays workers after approval.
- **User and document:** Hindi-speaking contractor with a printed muster roll photo and the message *"Is hafte ki hazri ke hisaab se sabko paise bhej do."*
- **How money moves:** Payouts V2 Validate Payout (UPI VPA or phone) then Process Validated Payout, with Batch Transfer for bank accounts; idempotent `transfer_id` per worker per week.
- **Sarvam:** Sarvam Vision for the printed sheet (handwritten accuracy is unverified), Saaras v4 Hindi, Bulbul v4 Flash Hindi.
- **Demo (3 steps):** (1) Sheet photographed; extracted attendance table shown. (2) Agent flags one row where days times rate does not match the written total. (3) Contractor corrects and approves; payouts run.
- **Why it wins:** The paper-to-payment path for informal labour, with the error check on the page being the visible intelligence.
- **Feasibility:** 3/5. Overlaps Dihadi Desk in the voice theme, so build one.
- **Risk and guardrails:** Extraction and matching errors. Per-site pre-registered worker list; per-worker cap; total read back.

---

## Ranking within this theme (our inference, /25)

| Rank | Idea | Total |
|---|---|---|
| 1 | Dadu Guard | 22 |
| 2 | Supplier Safe-Pay | 18 |
| 3 | Tender EMD Desk | 18 |
| 4 | Mandi Parchi | 18 |
| 5 | Muster Roll Reader | 17 |
