# Automating tax calculation from Amazon VAT reports: a system design for VAT service providers

> Originally published on the FXTech blog: https://fxweb.dev/blog/amazon-vat-report-tax-engine/

For firms that provide cross-border VAT services, the most labor-intensive step usually isn't registration. It's calculating tax every filing period. Sellers send over their Amazon VAT Transactions Report, and accountants sort it in Excel by country, rate and deductible item, then copy the numbers into each country's return. With more countries and more sellers, both the error rate and the turnaround time go up.

We built an online platform for a cross-border tax compliance provider covering VAT registration and agent transfer, tax calculation, filing and payment, EPR and authorized representative services. This article covers only the tax calculation part: how the system turns an Amazon report into numbers ready to file. It should be useful for VAT service providers and tax agents who want to turn their service into a product.

## The input: how the Amazon report is structured

The Amazon VAT Transactions Report has one row per transaction. Each row includes the transaction type, who collects the tax (Amazon or the seller), whether the buyer gave a valid VAT number, the ship-from and ship-to countries, the tax rate, gross and net amounts, currency and other fields.

Calculating tax comes down to deciding, for each row, "in which country, as what kind of supply and at what rate is this sale reported", then totaling by country and filing period.

## Core design: classify first, then calculate

Instead of writing rules country by country, we first put every transaction into one of 7 sales categories. The classification combines three fields:

- Tax collector: Amazon collects (as the deemed supplier) or the seller self-reports
- Buyer VAT number: whether the buyer is a business with a valid VAT number
- Ship-from / ship-to country: domestic sale, intra-EU cross-border sale or export outside the EU

Combined, these put each transaction into a category such as domestic sale, intra-EU distance sale, supply to a VAT-registered business, export or marketplace-collected. Each category has a fixed treatment: some are charged output tax at the local rate, some are reported at zero rate and listed separately, and for marketplace-collected sales the seller pays no further tax but still has to report them.

The benefit is that country differences collapse into one table of "how each country treats each category", instead of being scattered across hundreds of if-else branches. Adding a country means filling in that table.

The system has built-in cross-border rules for all 27 EU member states. Tax calculation is live for the UK and 8 EU countries: Germany, France, Italy, Spain, Poland, the Czech Republic, the Netherlands and Austria.

## Input tax: C79, C88 and PVA

UK import VAT is the part most often missed, so the system handles three kinds of documents separately:

- C79: the import VAT certificate issued by HMRC, which is the basis for reclaiming import VAT. The system collects C79 amounts per filing period and counts them as input tax
- C88: the import declaration, used to reconcile import value and tax, and part of the supporting evidence
- PVA (Postponed VAT Accounting): the seller pays no VAT at import and instead reports the same amount as both output and input tax on the return. The system puts PVA amounts in both boxes; the net effect is zero, but it must be reported

None of these are in the Amazon report, so sellers upload them separately. In the filing workspace they are separate items with reminders when something is missing.

## The output: UK VAT Return boxes

The UK VAT Return has fixed boxes (Box 1 to Box 9) for output tax, EU acquisitions tax, input tax, net tax, total sales and purchases, and intra-EU supplies and acquisitions. Once transactions are sorted into the 7 categories and C79 / C88 / PVA are processed, every box can be derived directly, without an accountant reconciling spreadsheets by hand.

For the 8 EU countries, the system outputs totals that match the structure of each country's return.

## Currencies and exchange rates

The currency of a transaction in the Amazon report isn't always the filing currency. The system fetches exchange rates daily and converts each transaction at the rate for its date. The rate source and conversion date are stored with each calculation so reviewers can trace them.

## Workflow: tax calculation inside the filing workspace

Tax calculation isn't a standalone tool; it's one step in the filing workflow. The workspace moves through these states: awaiting report upload → awaiting tax confirmation → awaiting submission → being filed by the accounting firm → awaiting payment → filed.

- Seller side: upload the Amazon report and deduction documents, see the calculated tax, confirm and submit
- Operations side: review calculations and filings, manage payment proofs and receipts, with role permissions and audit logs

With tax calculation inside the workflow, sellers see "how much is due this period and why" instead of an Excel file they have to interpret themselves.

## Engineering safeguards

Mistakes in tax data are expensive, so we put several safeguards in place:

- 20 end-to-end automated tests covering the main calculation paths
- A database schema comparison before each release so migrations don't drop fields
- Calculation results keep the mapping from original rows to totals, so a reviewer can trace any total back to individual transactions

The platform went through 300+ iterations in about 4 months, with about 44 business pages, 77 API endpoints and 19 data models. The stack is React 19 + TypeScript + Koa + MySQL, with Alibaba Cloud for OCR and object storage.

## Advice for VAT service providers

If you're thinking about turning manual tax calculation into a system, start here:

1. Write your tax rules as a "category × country" table first. It takes no code, but it exposes the gray areas in your rules
2. Model deduction documents separately from marketplace reports; they come from different sources on different cycles
3. Make every result traceable to individual rows, so reviewers are willing to sign off
4. Start with the UK and one or two EU countries, get the workflow running, then add countries

---

FXTech is an e-commerce technology agency and Shopify Partner based in Shenzhen, China. We build VAT compliance systems for VAT service providers, tax agents and cross-border e-commerce teams (we don't file taxes ourselves), and also build Shopify stores and headless DTC storefronts and optimize web performance. Case study: https://fxweb.dev/cases/#vat-compliance-saas . VAT solutions: https://fxweb.dev/vat/ .
