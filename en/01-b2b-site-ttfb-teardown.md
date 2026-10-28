# How we cut a B2B site's TTFB from 17.96s to 0.94s without a rebuild

> Originally published on the FXTech blog: https://fxweb.dev/blog/b2b-site-ttfb-teardown/

When we took on performance work for an LED lighting manufacturer's export website, the homepage took more than ten seconds to load on our first visit. The network wasn't the problem. The site was: the web server was in Germany, the database was in Hong Kong, the homepage banner was a video of almost 40MB, and images had no responsive variants. On mobile, first paint took tens of seconds, and some admin queries timed out with 502 errors.

Problems like these are common on PHP export sites, and most of them don't require rebuilding the site. Below is the full order in which we diagnosed and fixed them, so you can check your own site step by step.

## What we measure

We optimize around Core Web Vitals, Google's set of metrics for real user experience: LCP (Largest Contentful Paint, when the main content appears), INP (Interaction to Next Paint, how fast the page responds to a click) and CLS (Cumulative Layout Shift, whether the page jumps around). Add TTFB (Time to First Byte) and these four metrics usually tell you whether the problem is on the server, in asset size or in front-end code. They affect bounce and conversion rates, and they are one of the signals search engines use for ranking.

For tooling, Lighthouse and PageSpeed Insights give lab data, while the Chrome UX Report and real user monitoring (RUM) show what happens in production. Lab data locates the problem; field data confirms the business impact. You need both.

## Step 1: measure before you touch anything

Don't change things on instinct. We ran Lighthouse three times each on the homepage and a product page and took the median as the baseline:

- Homepage, desktop: score 56, LCP 6.5s
- Product page, desktop: score 44, CLS 0.287
- Homepage TTFB: 17.96s
- Homepage image download on mobile: 22.7MB
- Homepage video size: 38.8MB

With these numbers, every later step can be judged on whether it's worth doing and whether it worked.

## Step 2: high TTFB means look at the server path first

A TTFB of almost 18 seconds means the problem happens before the browser receives the first byte. It has nothing to do with front-end assets. We traced the request path and found the site running on a server in Germany while its MariaDB database sat in Hong Kong. Every page request queried the database across continents, and a single page can run dozens of queries, so the latency stacked up.

The fix was simple: move the database onto the German server itself and remove the intercontinental round trips. After this step, homepage TTFB dropped from 17.96s to 0.94s, ten admin requests went from 208.7s in total to 12.1s, and the query that used to time out with a 502 now returns in 1.43s.

Lesson: if your TTFB is above one second, first check whether the database and application server are in the same data center, then look for slow queries. Many export sites were built in stages, and the server moved while the database stayed behind.

## Step 3: the video was the largest single asset

The homepage banner video was encoded in HEVC and weighed 38.8MB. HEVC has patchy browser support, and here it didn't even make the file smaller. We transcoded it to H.264 at 720p, added a poster image and lazy loading, and the mobile video size dropped to 2.73MB.

Three points matter:

1. Choose the resolution for the size it's actually displayed at; a banner doesn't need 4K
2. Always set a poster image, otherwise LCP waits for the first video frame
3. Only load the video when it's needed; use the poster as the above-the-fold placeholder

## Step 4: responsive images instead of one image for every screen

The homepage downloaded 22.7MB of images on mobile because phones were fetching the full-size desktop originals. The fix:

- Resize images on the fly with an image CDN, outputting different sizes by request parameter
- Use srcset so the browser picks an image by screen width
- Serve WebP
- Add width / height placeholders so the page doesn't jump when images load (the main source of CLS)
- Load images only when they enter the viewport

Afterwards, mobile homepage image download fell from 22.7MB to 241KiB, a 99% reduction. Product page CLS went from 0.287 to 0.008, mostly thanks to the width / height placeholders.

## Step 5: JavaScript, caching and fonts

What remained was standard front-end cleanup:

- Merged 17 JS files into 4 and added defer so they don't block rendering
- Versioned static assets by file modification time with long CDN caching, so files cache for a long time but users never get stale ones after an update
- Restored font-display: swap so text shows in a system font first and swaps to the custom font when it loads, instead of leaving users staring at blank space
- Enabled Brotli compression on the server

We also scanned all 187 pages for JavaScript errors on desktop and mobile. Performance work touches a lot of code, and an error scan is the safety net against making the site faster and breaking features.

## Results

Lighthouse desktop (median of three runs):

- Homepage 56 → 88, LCP 6.5s → 1.5s
- Product page 44 → 91, CLS 0.287 → 0.008

We didn't rebuild the site at all. The stack is still PHP + ThinkPHP + MariaDB + Nginx; we just fixed each layer's problems one by one.

## After the work: preventing regressions

The most common ending for performance work is that the site is slow again three months later: someone uploads a 5MB original image, or a developer adds another third-party script. So at handover we keep the baseline report and the post-optimization report together, and we recommend adding a Lighthouse run to the release process to compare before and after each deploy. Check field data weekly so that when a metric slips, you can find the change that caused it.

Don't judge the result by lab scores alone, either. Look at LCP, INP, CLS and TTFB before and after alongside your conversion funnel to confirm the speed-up actually changed something for the business.

## The same method works for Shopify stores

None of this is specific to self-hosted PHP sites. Performance problems on Shopify stores usually come from theme code, too many third-party app scripts and unoptimized images, and all of these can be fixed at the theme level without switching platforms. The order is the same: measure a baseline, go after the largest assets, then deal with scripts and layout shift.

## A checklist to audit your own site

If you want to check your site yourself, go in this order:

1. Run PageSpeed Insights or Lighthouse three times and note TTFB, LCP, CLS and total download size
2. TTFB above one second: check database location, slow queries and server configuration
3. Look at the largest resources in the waterfall: usually videos and original images
4. Look at CLS: find images without dimensions, late-loading fonts and ad slots
5. Look at JavaScript: render-blocking scripts, and whether they can be merged and deferred
6. Finally, set up caching and compression

Measure again after each step and confirm you're moving in the right direction before starting the next one.

---

FXTech is an e-commerce technology agency and Shopify Partner based in Shenzhen, China. We build Shopify / Shopify Plus stores and headless DTC storefronts, optimize web performance (Core Web Vitals) and build cross-border VAT compliance systems. Case study: https://fxweb.dev/cases/#b2b-site-performance . Web performance service: https://fxweb.dev/wpo/ .
