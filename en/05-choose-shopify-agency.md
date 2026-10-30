# How to evaluate a Shopify development agency: a checklist for brands

> Originally published on the FXTech blog: https://fxweb.dev/blog/choose-shopify-agency/

Search for "Shopify development agency" and you get directories and ads. Very few of them explain how to tell whether an agency can actually do the technical work.

We are a Shopify and e-commerce development agency ourselves. This article is written from the brand's side: what to decide before you send a brief, what to check in a portfolio, which technical questions to ask, how to compare quotes and which warning signs to watch for. Use the checklist on anyone, including us.

## First, decide which kind of work you are buying

"Shopify store build" covers several very different jobs, and each fits a different kind of agency:

| What you want | The actual work | Who fits |
|---|---|---|
| Set up a theme, add products, design pages | Theme editor setup, content entry, app installs | Store setup / marketing agencies, design studios |
| Pages and interactions built to your brand design | Liquid development on a theme, custom sections | A team with front-end developers |
| Store connected to ERP, CRM or inventory | Shopify Admin API, webhooks, custom apps | A team with back-end and integration experience |
| A business process far from standard e-commerce | Shopify Plus checkout customization, or a headless storefront | A team that can design the architecture and run it long term |
| A live store that is slow or converts poorly | Performance audit, theme code and third-party script cleanup | A team that has done performance work and shows the numbers |

Many projects fail not because the agency didn't try, but because the brand hired the wrong type: a development budget spent on a store-setup team, or an integration team asked to build a design-heavy brand site.

A simple test: write the brief on one page, split into "pages and interactions", "data and integrations" and "maintenance after launch". If you can't write anything in the second and third parts, you probably only need one of the first two types.

## Check the portfolio, not the screenshots

Portfolio screenshots always look good, but they don't show code quality. Once you have the live links, spend 10 minutes on each:

1. **Open the live site on your phone.** Mobile layout, fonts and tap targets reveal more care than desktop does.
2. **Run PageSpeed Insights on mobile.** You don't need a perfect score, but if LCP is far above 4 seconds or the main thread is full of third-party scripts, the team doesn't care much about performance. On one export site we later optimized, the homepage TTFB was 17.96 seconds, and every cause could have been avoided at delivery.
3. **Count the app scripts in the page source.** A page loading a dozen app scripts usually means features were installed, not built.
4. **Ask which parts they built.** On the same site, one agency may have only uploaded products while another wrote the whole theme. Ask them to point to their work.

For an energy storage brand we built a full Shopify site on the official Dawn theme: 4 product lines and 11 subcategories, retail products bought directly, large storage systems handled through downloads and inquiries, and Shopify Markets set up for the US and India. For a case like this, ask the agency: why Dawn rather than a paid theme? How is hreflang handled across markets? Where does the inquiry form data go? Teams that really did the work can answer in detail.

## Eight technical questions to ask

You don't need to read code. Listen for specific answers:

1. **Whose name is the store under?** It should be yours. The agency should request collaborator access to your store through the Shopify Partner dashboard, which you approve with the permissions you choose, or build on a development store and transfer ownership to you. A store registered on the agency's account and credit card is the most common source of trouble later.
2. **How is theme code managed and delivered?** A good answer mentions a Git repository, local development with Shopify CLI, and building on an unpublished theme for preview, not editing the live theme directly. You should receive the full theme code at handover.
3. **Which theme is it based on, and what about future theme updates?** A heavily customized official theme can't simply follow new upstream versions. That's a normal trade-off; what matters is whether it's explained up front.
4. **What will be an app and what will be custom code?** Mature apps for reviews or email marketing don't need rebuilding. But when the core experience depends on stacking several apps, fees and page weight add up. For a smart fitness equipment brand, the loyalty program had to link to the brand's own app accounts, which third-party loyalty apps couldn't do, so we built the member center, task-based points and coupon redemption (6 sub-pages) inside the theme, with no loyalty app subscription. The agency should give reasons for calls like this.
5. **How does sync with ERP or inventory work?** Listen for webhooks, API rate limits, retries and reconciliation. "We can integrate it" with no details usually means they haven't.
6. **How are languages and markets handled?** Shopify Markets, separate domains or subfolders, hreflang, currencies and price rules. Teams that explain this clearly have usually hit the problems.
7. **Is there a performance target, and how is it accepted?** Ask for mobile Lighthouse / PageSpeed targets and how they'll re-test after launch.
8. **Who maintains the store after launch, and how is it billed?** Bug fixes, small changes and adapting to Shopify platform changes all need an owner. If this isn't agreed, it's where disputes start.

## Comparing quotes

"Build a Shopify store" can be quoted at prices ten times apart because the work is completely different. When comparing:

- **Align scope before price.** Have every agency quote line by line against the same brief, with pages, custom modules, integrations, markets and maintenance listed separately.
- **There are three common pricing models**: fixed price per project (for well-defined builds), time and materials (for custom work and integrations where requirements will move), and a monthly retainer (for ongoing work after launch). None is always better; match it to how settled your requirements are.
- **Be wary of package prices given before reading your brief.** Packages fit standard setup work, not custom development.
- **Ask what's excluded.** Paid themes, app subscriptions, Shopify Plus fees, translation, product photo work and data migration are usually not in a development quote.

## Warning signs

- The store has to be registered on the agency's account, or you don't get admin access
- No theme code handed over, or the code only lives in the live store with no version control
- Every feature is solved by installing an app, and they can't say what could be built instead
- Portfolio cases are screenshots only, with no live links
- "Shopify Partner" presented as a credential. Joining the Shopify Partner program is free and open to any developer; it means they can work through the Partner dashboard, not that they're certified. Agencies listed in the Shopify Partner Directory are vetted and the bar is higher
- Promises of specific sales or conversion gains. Engineering improves speed and experience; sales depend on product and marketing

## Working with an agency in China

Agencies in China (including Shenzhen, where we are) usually cost less and suit cross-border sellers and brands whose teams are in China. Agencies in your target market know local design habits and compliance better. If your team is overseas and you're considering a China-based agency, check whether they've built sites for your market and can work directly in English with your marketing, design or engineering team, with enough overlap in working hours.

## Start with a small project

If you're still unsure, don't sign for the whole site first. Give a small but complete task: one custom section, a performance audit, or a prototype of an app integration. One or two weeks of real work tells you more than any portfolio or call: how communication goes, how code is delivered and how problems are handled.

---

FXTech is an e-commerce technology agency and Shopify Partner based in Shenzhen, China. We build Shopify / Shopify Plus themes and apps and headless DTC storefronts, optimize web performance and build cross-border VAT compliance systems. Shopify services: https://fxweb.dev/shopify/ . Headless storefronts: https://fxweb.dev/ecommerce/ . Case studies from this article: https://fxweb.dev/cases/#energy-storage-shopify-store and https://fxweb.dev/cases/#shopify-loyalty-program .
