# Shopify or Medusa? A framework for choosing your DTC e-commerce stack

> Originally published on the FXTech blog: https://fxweb.dev/blog/shopify-vs-medusa/

"We're building our own online store. Should we use Shopify or build it ourselves?" This is the question we get asked most. The usual answers are "Shopify is easier" or "open source gives you freedom". Both are true, and neither is enough to make the decision.

This article gives you a framework you can apply directly. We build both Shopify stores and headless storefronts on MedusaJS, and we've hit the pitfalls on both sides, so we try not to favor either.

## What the two routes actually are

**Shopify / Shopify Plus**: a SaaS e-commerce platform. The storefront, checkout, payments, inventory and orders are all hosted by the platform. You customize the storefront with themes (Liquid) and extend functionality with apps and APIs. Shopify Plus is stronger on multiple stores, checkout customization and automation, and is aimed at larger brands.

**Headless / composable storefront (MedusaJS as the example)**: the shopping front end is decoupled from the core commerce services (products, orders, payments, membership and so on). The back end is built on an open-source commerce framework with custom APIs, a database and object storage; the front end is developed separately and can be designed and iterated freely. You own the code, data and hosting, and there's no platform commission. The cost is that you (or your agency) are responsible for deployment and operations.

## Six questions to decide

### 1. How well does your business process fit the platform?

Ask yourself: can Shopify's standard flow cover 80% or more of how you take orders, fulfill them and handle after-sales?

If it can, go with Shopify. The remaining 20% can usually be covered with theme customization and custom apps. We built a full Shopify site for an energy storage brand whose products ranged from EV chargers sold directly at retail to containerized storage systems that need an inquiry. Retail products are bought directly, large storage products go through downloads and inquiries, and the whole information architecture of 4 product lines and 11 subcategories lives in Shopify. There was no need to build a custom stack.

If your core process is different from standard e-commerce, such as custom quoting, multi-party fulfillment or complex membership tiers, the platform will keep holding you back. That's when to consider headless.

### 2. How deep does your customization really need to go?

Many teams overestimate how much customization they need. You can do more at the Shopify theme level than you might think. For a smart fitness equipment brand, we built a complete loyalty program inside the Shopify theme: member center, points for tasks, points history, coupon redemption and linking to the brand's app account. All 6 sub-pages are built on custom sections and Alpine.js with no third-party loyalty app, and they're live on the US and EU stores.

The test isn't "do we want customization" but "does the customization touch checkout and the core data model". For storefront experience and surrounding features, Shopify is enough. To change checkout logic or the data model itself, Shopify Plus solves part of it; beyond that is headless territory.

### 3. Data ownership and long-term cost

On Shopify, your data lives on the platform. You can export it through the API, but your business logic is tied to the platform, and platform fees and app subscriptions grow with GMV.

A headless storefront has no platform commission and your data sits in your own database, but you carry the cost of servers, operations and ongoing development. That's not a one-off investment; it's a long-term need for engineering capacity.

A simple rule: if you don't have, and don't plan to have, a technical team (in-house or a long-term agency), don't go headless.

### 4. Multiple markets and localization

Both routes support multiple languages, currencies and markets.

Shopify Markets lets you configure currency, domain and pricing per country. For the energy brand we set up two markets, the US (USD) and India (INR), with India on its own domain and hreflang configured. These are standard platform features.

A headless setup gives you more freedom over localized content, tax rules, shipping strategy and regional marketing. It suits brands whose markets differ a lot and need deep regional operations.

### 5. Integration with internal systems

At a certain size, the store has to connect to your ERP, CRM and inventory systems: update stock in real time to avoid overselling, sync customer data to the CRM and send orders back to the warehouse.

On the Shopify route, this is done with custom apps built on the API that sync orders, inventory, products and customer data both ways with your internal systems, with encrypted communication and role-based permissions. Surrounding systems (product center, order center, shipping management, reporting, ad cost analysis, supply chain management) can be built outside the store and exchange data with Shopify through the API.

On the headless route, these integrations are part of your own back end: payments can integrate Stripe, PayPal and local payment methods with order validation and fraud checks; logistics can connect to carriers, warehouse systems and shipping rules with shipment status sync and tracking. You get more freedom, but every integration is code you have to build and maintain.

### 6. Time to launch and where your team is today

Shopify can go from zero to live quickly with almost no operations work. Headless takes longer from design through development to deployment, and after launch someone has to own access control, backups, log monitoring and staged rollouts.

If your team is just starting and needs to validate the market, choose Shopify. If you already have steady sales, know what you need and are ready to invest long term, then consider headless.

## A decision table

| Your situation | Leans toward |
|---|---|
| Standard e-commerce flow, want to launch fast | Shopify |
| Need custom storefront experience, loyalty, marketing modules | Shopify + theme / app customization |
| Multiple stores, checkout customization, automation | Shopify Plus |
| Business process differs a lot from standard e-commerce | Headless |
| Sensitive to data ownership and platform fees, have a tech team | Headless |
| No tech team | Shopify |

## Common misconceptions

- **"Shopify is slow, so we need our own stack"**: most Shopify performance problems come from theme code, too many third-party app scripts and unoptimized images. All of these can be fixed at the theme level without switching platforms
- **"Open source is free"**: the framework is free; the engineering isn't
- **"Start on Shopify and migrate later"**: you can, but migration cost grows with your data volume and customization depth, so think through your direction for the next two to three years when you first choose

## Our advice

Understand your business stage and team first, then pick the solution. Don't pick the technology first and look for reasons afterwards. If you're unsure, write down your order flow, your current systems and your goals for the next two years, then go through the six questions above one by one. The answer usually becomes clear.

One more thing that often gets overlooked: launch isn't the finish line. Whichever route you take, the store will need troubleshooting, new features and performance work after launch. Decide up front who will own that, whether it's an in-house team, a long-term agency or the platform's support. That answer often decides which route to take.

---

FXTech is an e-commerce technology agency and Shopify Partner based in Shenzhen, China. We build Shopify / Shopify Plus stores and headless DTC storefronts on open-source frameworks such as MedusaJS, optimize web performance and build cross-border VAT compliance systems. Shopify services: https://fxweb.dev/shopify/ . Headless storefronts: https://fxweb.dev/ecommerce/ . Case studies from this article: https://fxweb.dev/cases/#shopify-loyalty-program and https://fxweb.dev/cases/#energy-storage-shopify-store .
