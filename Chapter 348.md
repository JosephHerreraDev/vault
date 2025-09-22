---
tags:
  - note/herrerasoft
---

# Resource
---
[Chapter 348, Texas Finance Code: Motor Vehicle Installment Sales](https://statutes.capitol.texas.gov/Docs/FI/htm/FI.348.htm)

# Summary
---
**Chapter 348** governs retail installment sales of motor vehicles in Texas. It defines key terms, specifies contract content and form requirements, limits interest and fees, mandates disclosures, and sets out buyer/holder rights and duties. Below is a structured summary of each subchapter, highlighting what a Dealer Management System (DMS) must handle.

## A. Definitions and General Scope (Subchapter A)
---
- **Key terms (Sec. 348.001)** – For example, _“Buyer's order”_ is a nonbinding, preliminary written computation for a sale. The _“holder”_ is the seller or any assignee of the contract. A _“motor vehicle”_ is broadly defined (cars, trucks, motor homes, etc.). An _“itemized charge”_ is any fee **not** included in the cash price (such as title/registration fees, taxes, warranty or service contract fees, insurance or protection product charges). The _cash price_ means the in‑cash sale price (no finance charge).
- **Cash price and itemized charges (Sec. 348.004–.005)** – The cash price may include accessories, service contracts, taxes, and title/registration fees. Anything added beyond cash price must be itemized (registration fees, taxes, insurance, warranties, GAP/“debt cancellation” plans, etc.).
- **Documentary (doc) fee (Sec. 348.006)** – If charged, a doc fee (for processing sale documents) is added to the _principal balance_ (cash price + itemized charges – down payment). To include a doc fee in finance, the seller must impose it on all buyers (cash and credit), cap it at a “reasonable amount,” and clearly disclose it by bold notice near the fee amount. A DMS should track any doc fee amount, display the legally-required notice text, and ensure it’s uniform and reasonable.
	- "A DOCUMENTARY FEE IS NOT AN OFFICIAL FEE. A DOCUMENTARY FEE IS NOT REQUIRED BY LAW, BUT MAY BE CHARGED TO BUYERS FOR HANDLING DOCUMENTS RELATING TO THE SALE. A DOCUMENTARY FEE MAY NOT EXCEED A REASONABLE AMOUNT AGREED TO BY THE PARTIES. THIS NOTICE IS REQUIRED BY LAW."
- **Retail installment transaction (Sec. 348.002–.003)** – A long-term lease or bailment of a vehicle is treated as a retail installment sale if the lessee pays roughly the vehicle’s value and will become owner. Routine financing practices (charts, preprinted forms, credit checks) do _not_ exclude a deal from being governed by Chapter 348.
 VVV
## B. Retail Installment Contract (Subchapter B)
---
- **Form requirements (Sec. 348.101)** – Every sale must have a written retail installment contract. It may be multi-part but must be dated and **signed by both buyer and seller**. All essential terms must be filled in before the buyer signs (except VIN and first payment date, which can be added later). Standard print (except instructions) must be ≥8-point font. A DMS must flag and disallow unsigned or undated contracts, or any blank required fields at signing.
    
- **Contract contents (Sec. 348.102)** – The contract must include, at minimum: seller’s name and address; buyer’s name and (residence) address; vehicle description (make/model/VIN); cash price; down payment (separating cash vs. trade-in credit); and each itemized charge. (It also must state the finance charge method if a _variable-rate_ contract is used.) The chapter permits adding non-required info, and items need not appear in any set order. A DMS should ensure all required fields are present and clearly labeled on contract templates.
    
- **Statutory notices** – By law the contract (in 10pt boldface or similar) must display a consumer notice (e.g. _“DO NOT SIGN… IF ANY BLANK SPACES…”_ and a warning about prepayment rights). If any of these conspicuous warnings are missing or modified, a DMS should flag it as non-compliant.
    
- **Trade-in equity disclosure (Sec. 348.0091)** – If the buyer is trading in a vehicle, the dealer must provide a **completed “Trade-In Equity” disclosure form** _before_ the contract is signed. The form (prescribed by rule) must show buyer/seller info, trade-in vehicle details, offered trade-in value vs. buyer’s loan balance, and clearly indicate if equity is positive or negative. It must also show the purchase vehicle’s cash price and the financed amount. A DMS should collect and store these values, and ensure the printed form fields exactly match Sec. 348.0091 requirements.
    
- **Conditional delivery agreements (Sec. 348.013)** – This governs “spot delivery” deals (dealer allows buyer to take a car home on a temporary agreement). These contracts (max 15 days) are enforceable and must state an agreed price and trade-in value if applicable. If the parties do _not_ complete a purchase contract, the dealer must return any trade-in (or pay its agreed value) plus any down payment within 7 days. A DMS can track such conditional deals, ensure the term ≤15 days, and trigger a return or refund transaction if no sale closes.
    
- **Prohibited conditions (Sec. 348.1015, 348.014)** – Contracts may **not** be conditioned on assignment to a financer (Sec. 348.1015). Any such clause is void. Also, a dealer may _not_ require the buyer to purchase any ancillary “vehicle protection product” (like an extended service or security plan) as a _condition_ of the sale, unless it’s installed on the vehicle at sale time. A DMS should prevent adding forbidden contingencies, and ensure buyer optional add-ons are truly optional.
    

## C. Finance Charges and Fees

- **Time-price differential (TPD)** – This is the interest/finance charge on the unpaid balance. Contracts may charge any TPD amount _up to_ the maximums set by law (which vary by vehicle age and whether new/used). For equal monthly payments (one-month delay to first payment), the TPD cannot exceed statutory “add-on” rates (for example, $7.50–$18 per $100/year depending on age/model year). If payment terms aren’t standard, the effective annual return must match what these rates would yield. (Alternatively, a lender may elect to use the Chapter 303 “optional ceiling” as a cap.) The DMS must compute and enforce these caps: e.g. given balance and term, it should calculate TPD by formula and ensure it does not exceed Sec. 348.104 limits.
    
- **Late or default charges (Sec. 348.107)** – If a payment is more than 15 days late, the holder may charge _either_: a late fee up to **5%** of that installment amount; _or_ interest on the past-due amount at the contract rate (but not both). The system must restrict late fees to ≤5%, or instead apply interest (per the agreement’s rate). Only one late fee per installment is allowed.
    
- **Collection costs (Sec. 348.108)** – Contracts may include reasonable expenses for debt collection: attorney’s fees (if outside counsel), court costs, and out‑of‑pocket costs of repossessing/storing/reselling the vehicle. A DMS should allow these charges to be added only under “good faith and commercial reasonableness” (per Business & Commerce Code standards).
    
- **Acceleration (Sec. 348.109)** – A holder cannot accelerate the balance (demand full payment) unless the buyer is actually in default or the holder in good faith believes payment is unlikely. This is a legal restriction; the DMS should prevent any user from marking a loan accelerated unless a default event occurred.
    
- **Contract amendments** – Holders may agree to amend contracts (defer payments, extend term) at buyer’s request (Sec. 348.113). Deferring installments (up to 3 months) on an add-on or schedule-of-payments contract allows a **deferment fee** (computed on deferred amount at a rate ≤ the original contract’s effective rate) plus any additional insurance premiums or official fees. Alternatively, if using true-daily-interest, the holder simply continues accruing interest and must give the buyer a bold notice about extra finance charges. Other extensions or restatements (that aren’t “short deferrals”) may be charged by financing the new balance at the prevailing TPD for the new term. Any amendment must be confirmed in writing and signed by the buyer; a copy is delivered to the buyer. The DMS should support contract changes, recalc of balance/interest, and ensure legal notices are generated on deferment.
    
- **Prepayment (Sec. 348.118–.121)** – The buyer may prepay the contract in full at any time. Prepayment overrides any contrary contract clause. Upon full prepayment (or on demand of payoff), the buyer is entitled to a refund (rebate) of unearned finance charges. For standard monthly contracts, the refund credit is computed by a formula: subtract a $25 _acquisition cost_ from the original finance charge, then multiply by the ratio of remaining balances to original scheduled balances. (If the remaining term is <1 year or lump-sum, a proportional method applies.) No refund is due if under $1. The DMS must calculate payoff amounts including these statutory rebate schedules, and never charge a prepayment penalty.
    
- **Refinancing of large installment (Sec. 348.123)** – If one scheduled payment is more than twice the average prior installments (ignoring down payment), the buyer can refinance just that “balloon” payment when due without cost, amortized in equal payments of at least the prior average, at a rate no higher than originally contracted. This does _not_ apply to certain leases or commercial-use vehicles. A DMS should detect any balloon >2× average and enforce this special right, computing new schedule and capping rate as prescribed.
    

## D. Disclosures, Notices and Recordkeeping

- **Contract copies (Sec. 348.110–.112)** – The seller must give the buyer a copy of the fully executed contract (or mail it to buyer’s address). Until this copy is given, a buyer who has no vehicle delivery may _rescind_ the deal and get all payments and trade-ins returned. The buyer’s signed acknowledgment (included in the contract) of receiving the copy – placed above their signature and in bold 10pt – is conclusive proof that the buyer got the contract and that no required blank was left. The DMS should track contract delivery (signed receipt or mail date), and enforce rescission rights if not delivered timely.
    
- **Payment statements (Sec. 348.405)** – Upon written request, the holder must give the buyer (or designee) a schedule of payments made and payoff amount. One statement is free every 6 months; additional are at most $1 each. The DMS should generate periodic statements and track request frequency and fees.
    
- **Payment receipts (Sec. 348.406)** – A written receipt is required for every cash payment by the buyer. A DMS must print receipts for all cash transactions (with date, amount, payor).
    
- **Outstanding balance/payoff (Sec. 348.408)** – If the holder gives the buyer (or an authorized designee) a payoff quote, the holder is bound by it for a reasonable time. If the buyer tenders that amount, the holder must accept it as full payoff and release the lien on the vehicle within 10 days. The seller must also pay off any trade-in loan not later than the 25th day after delivery of both new and trade-in vehicles (and proper documents). A DMS should record payoff quotes with expiration, adjust ledgers on payoff, trigger lien release, and calculate deadlines for trade-in payoff.
    
- **Buyer's remedies for misinformation** – If a holder refuses to honor a correct payoff (e.g. demands more than the quoted amount), the buyer can recover triple the difference plus interest, attorney fees, and costs. The system should ensure that payoff quotes are accurate and binding to avoid such penalties.
    
- **Document retention and reporting** – While not explicitly requiring new fields in the DMS, the statute implies that a DMS must store all executed contracts, amendments, notices (insurance statements, equity disclosures, etc.), payment records, and acknowledgments as compliance proof. It must also be able to produce these records if audited by the Finance Commission.
    

## E. Insurance and Protection (Subchapter C)

- **Property insurance (Sec. 348.201)** – The holder may require the buyer to carry insurance on the vehicle and accessories that covers the holder’s interest. Premiums must be reasonable relative to the loan amount/term and risk. A DMS should note whether insurance is required and allow financing of its premium as a separate line item.
    
- **Credit life/accident insurance (Sec. 348.202–.203)** – The holder may also require (or buyer may choose) credit life or accident insurance. Coverage amounts cannot exceed the loan balance (with an annual cap on scheduled indemnities). Premiums and agent fees can be added as separate charges. The DMS must limit coverage to these statutory maxima and allow financing the premiums.
    
- **Insurance disclosures (Sec. 348.204–.207)** – If insurance is required, the holder must give a written statement that insurance is required and that the buyer _may_ use their own existing policy or one from an authorized insurer. If optional liability insurance isn’t included in the contract, a conspicuous notice must say _“liability insurance coverage for bodily injury/property damage to others is NOT included.”_. The buyer can also procure required insurance themselves within 10 days; if the buyer fails to provide proof, the holder may buy substitute insurance (fair coverage) at legal rates and add its premium to the loan balance (the system should generate the notice of substitute insurance to the buyer).
    
- **Other coverages (Sec. 348.208)** – The contract may also include optional coverages (as separate charges) that are **typically available to the public**: collision/liability insurance, mechanical breakdown, theft protection, diminished value coverage, warranties or service contracts, ID theft protection, GAP (debt cancellation) if sold, and even trade-in credit agreements. These must be on insurer‑approved forms. In a DMS, each such product should be selectable, with price, term, and disclosures handled per law.
    
- **Debt cancellation (GAP) (Sec. 348.124)** – A seller may offer (but _not_ require) the buyer to buy a debt cancellation agreement (often called GAP waiver). This is not insurance, and its charge must be reasonable. The seller must provide a separate notice that the buyer is not required to take it. The DMS should track whether GAP is offered, whether accepted, and include required disclosures.
    
- **Trade-in credit agreements (Sec. 348.125)** – At signing, the seller may offer a _trade-in credit agreement_ (a future credit toward another vehicle when the buyer’s car is traded in). If so, the buyer gets a copy and a written notice that: (1) it’s optional; (2) they can cancel within 30 days for a full refund; (3) they can cancel later for pro rata refund minus a cancellation fee ≤ $50. The price must be ≤ 5% of the vehicle’s cash price (excluding taxes/fees). The credit provided must be commensurate with the fee charged. A DMS should record the sale of any such agreement, enforce the 5% cap, and allow a refund calculation.
    
- **Auto club memberships (Sec. 348.414)** – Similarly, a seller may offer an auto club membership. The buyer must be told it’s optional and refundable within 30 days. If services are already included by the manufacturer, the dealer must inform the buyer. The fee must be reasonable. A DMS can treat this like any optional product with a cancellation window.
    

## F. Assignment and Payment Duties (Subchapter D & E)

- **Assignment of contracts (Sec. 348.301–.303)** – Any person may buy (acquire) contracts or balances; there is _no duty_ to disclose assignment details or terms. A buyer need not be notified of assignment; if the buyer pays the last holder they know, that payment binds all later holders. In practice, the DMS should allow recording assignments but needn’t notify the buyer, and any payment received from the buyer should clear the account (no matter which holder is current).
    
- **Seller advances and incentives (Sec. 348.403–.404)** – A dealer cannot give cash back to the buyer in the transaction unless expressly allowed. However, the dealer may advance money to pay off a trade-in’s existing loan (or a lease/other auto loan); such advances can be included in the new contract as an itemized charge. The DMS should allow a “payoff” field for trade-in financing, and include it in disclosure (the code requires any such advance to be shown in any manner TILA/Reg Z allows).
    
- **Payment obligations** – Upon payoff of a trade-in’s lien, the seller must submit full payment to that lienholder within 25 days of sale. The system should trigger that payment and record the discharge.
    
- **Receipts and documentation** – As noted, receipts for cash payments (Sec. 348.406) and notices of any personal property retained on repossession (Sec. 348.407) are required. If a repossession yields non-attached items (like custom stereo, rims), the holder must notify the buyer (by mail/15th day) and allow him to claim them by day 30. If unclaimed, the holder may sell them per law. The DMS should log any such notices and track disposition dates.
    
- **Liens and repossession (Sec. 348.408–.411)** – Besides lien release on payoff (above), contracts may _not_ include power-of-attorney to confess judgment or wage assignments. Repossession must comply with Business & Commerce Code (no breach of peace) and _may not_ authorize a repossession agent as the buyer’s attorney-in-fact. Contracts also cannot waive the buyer’s legal rights or defenses against holder misconduct. The DMS should ensure no unlawful clauses are stored or used in templates.
    
- **Equity transfers (Sec. 348.413)** – A buyer may transfer his equity in the vehicle (sell out of the contract) with the holder’s consent. The holder may charge up to $25 for this equity transfer. The DMS should allow a $25 transfer fee and update the contract to substitute the new buyer if approved.
    

## G. Licensing and Administration (Subchapter F)

- **Holder licensing (Sec. 348.501)** – Only an authorized lender, credit union, or a person licensed under Chapter 348 may be a holder of these contracts. While license details are mostly administrative, a DMS should record whether a seller/holder is licensed. It should also enforce that only loans financed by licensed entities are processed.
    

## Data/Fields a DMS Must Manage

Based on the above, a Dealer Management System should be capable of tracking/managing:

- **Contract details:** Buyer/seller names and addresses; vehicle (make/model, VIN, year); cash price; trade-in details (value, loan payoff balance); down payment amounts; itemized charges (each fee or accessory/service financed); interest rate or finance charge method; payment schedule (dates/amounts) and maturity date.
    
- **Optional products:** Whether GAP/debt-cancellation, trade-in credit agreement, auto club, warranties, etc., were offered and sold; their charges; cancellation rights and deadlines.
    
- **Disclosure forms:** Completed trade-in equity disclosure form fields (see Sec. 348.0091) and conditional-delivery terms if used.
    
- **Insurance elections:** Whether vehicle insurance or credit-life/health insurance was required or elected; premiums; whether buyer elected to supply own insurance.
    
- **Documentary fee:** If charged, the amount and the required disclosure notice text.
    
- **Contract amendments:** Records of any deferments or extensions (new payment dates and recalculated balance/interest), with written confirmations.
    
- **Payments and payoffs:** All payments (date, amount, type), including special entries for payoff of contract or trade-in. The DMS should compute payoff amounts including prepayment refunds. Track issuance of payoff quotes and their expiration. Release lien on payoff date.
    
- **Statements and receipts:** Logging of any statement requests, issuance dates, and applicable fees. Issuance of receipts for cash payments.
    
- **Notices/acknowledgments:** Evidence of delivering contract copies, acknowledgments signed, insurance notices given (required by Sec. 348.204–.207), and any refunds/cancellations (e.g. auto club, GAP) exercised.
    
- **Compliance validation:** Ensuring no prohibited contract terms (power-of-attorney to confess judgment, wage assignments, etc.), that pre-signed blanks are not used, and all required boldface statements are present.
    
- **Regulatory tracking:** License status of the seller/holder, and notification deadlines (e.g. 25-day trade-in payoff rule).