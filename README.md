# Proxies for Web Scraping: Which Type Do You Need, What Does $1.40–$8/GB Actually Buy, and When Is a Scraping API the Better Deal?

Most scraping projects fail on one of two walls: per-IP rate limits, or content that changes by country. Residential proxies cost roughly **$1.40 to $8 per GB** depending on provider and committed volume, but the headline rate is usually a volume-tier price that small accounts never reach. If you'd rather not run rotation, retries, user-agent management, and ban detection yourself, ScraperAPI bundles all of that into a single endpoint and charges per credit instead — plans start at **$49/month for 100,000 credits**, with a free tier of 1,000 credits per month (5,000 for your first seven days) and a 7-day refund window.

The rest of this article covers the current plan lineup, how the credit multiplier changes what you actually get, which proxy type fits which target, and the pricing math most comparison posts skip.

## Quick answer before you read the tables

> If you have working scraper code and mostly plain-HTML targets, ScraperAPI's **Hobby plan at $49/month (100,000 credits)** covers about 100,000 standard page requests, or 10,000 pages that need JavaScript rendering or premium residential IPs. If you need country-level targeting beyond the US and EU, the entry point is **Business at $299/month**. 👉 [Start with the Hobby plan](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## ScraperAPI plans and prices (all tiers, current list prices)

Every plan includes JavaScript rendering, premium residential and mobile IPs, anti-bot bypass, rotating proxy pools, CAPTCHA handling, custom headers, automatic retries, unlimited bandwidth, and a 99.9% uptime guarantee. What separates the tiers is credit volume, concurrency, geotargeting scope, and whether pay-as-you-go overage is available.

| Plan | Credits/month & concurrency | Geotargeting | Price (monthly / annual) | Get it |
| --- | --- | --- | --- | --- |
| Free | 1,000 credits, 5 threads | US and EU only | $0 | [Create a free account](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | 100,000 credits, 20 threads | US and EU only | $49 / $44.10 per month | [Get Hobby](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | 1,000,000 credits, 50 threads | US and EU only | $149 / $134.10 per month | [Get Startup](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | 3,000,000 credits, 100 threads | Global, country-level | $299 / $269.10 per month | [Get Business](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | 5,000,000 credits, 200 threads | Global, country-level | $475 / $427.50 per month | [Get Scaling](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | 10,500,000 credits, 300 threads | Global, country-level | $975 / $877.50 per month | [Get Professional](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | 21,500,000 credits, 500 threads | Global, country-level | $1,975 / $1,777.50 per month | [Get Advanced](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | 22,000,000+ credits, 500+ threads | Global, country-level | Custom quote | [Talk to sales](https://www.scraperapi.com/?fp_ref=coupons) |

Annual billing takes exactly 10% off and gives you the full year's credits upfront, usable on any timeline. Unused monthly credits are lost at renewal, not carried forward.

Two restrictions matter more than the credit number on the tin. Pay-as-you-go overage only exists on Scaling and above; on Hobby, Startup, and Business, running out of credits returns a 429 and scraping pauses until renewal unless you enable auto-upgrade. And US/EU-only geotargeting on the two cheapest paid plans is a hard ceiling, not a soft one — if your keyword research needs French, German, or Japanese SERPs, Hobby and Startup can't do it.

**Also worth knowing:** Business, despite costing six times Hobby, does not include pay-as-you-go or priority support. You're paying for credit volume, concurrency, and global geotargeting. Professional adds priority support; Advanced adds priority routing.

## The credit multiplier is where budgets actually break

One credit equals one standard page request. That's the only case where the plan's credit count equals page count. ScraperAPI prices harder domains and parameters at multipliers, and the multiplier is per domain, not per plan — upgrading doesn't change it.

| Request type | Credits |
| --- | --- |
| Standard page | 1 |
| Amazon, Walmart, eBay | 5 |
| Google, Bing | 25 |
| LinkedIn | 30 |
| Cloudflare / DataDome / PerimeterX / Akamai bypass (auto-applied) | +10 |
| `render=true` (JavaScript) | 10 |
| `premium=true` (residential/mobile IPs) | 10 |
| `screenshot=true` | 10 |
| `premium=true` + `render=true` | 25 |
| `ultra_premium=true` | 30 |
| `ultra_premium=true` + `render=true` | 75 |
| `country_code`, `session_number`, `device_type`, `output_format`, `autoparse` | No extra cost |

Run the numbers on the $149 Startup plan and the same 1,000,000 credits buy 1,000,000 plain pages, 100,000 rendered pages, 40,000 Google searches, or roughly 13,000 rendered ultra-premium pages. That's a 75x spread in effective cost per page depending on what you're scraping, and it's the single most common reason people conclude a plan "didn't deliver enough credits."

The anti-bot bypass surcharge deserves a note: it's applied automatically when ScraperAPI activates bypass logic, not when you ask for it. You don't opt in, and you don't get warned mid-run. Check any URL in the dashboard's Domain Cost Estimator before you scale, and set the `max_cost` parameter on every request — the API returns a 403 rather than silently charging past your cap.

Geotargeting itself is free on any plan that supports the country you need. You set it with `country_code=fr`, `country_code=de`, and so on; the flag routes the request through that country's pool, and proxies rotate after each request automatically.

## Datacenter, residential, mobile, ISP: match the type to the target

There's a genuine disagreement among vendors here, and it's worth stating both sides rather than pretending one is settled.

The pro-residential position: protected, anti-bot-heavy sites need residential IPs because they look like real users; datacenter IPs suit high-volume, lenient targets; ISP proxies suit logged-in or long sessions.

The counter-position, argued by Zyte: in the vast majority of cases, datacenter IPs plus solid proxy management produce the best results at the lowest cost, and a well-configured datacenter pool can approach residential performance without the legal baggage. Zyte sells datacenter-heavy infrastructure, so treat that as a commercial position too.

My read: start with datacenter on any target you haven't tested, because it's cheaper and faster, and move to residential only when you see the block pattern. If the site sits behind Cloudflare, DataDome, or PerimeterX and returns CAPTCHAs to clean datacenter IPs, no amount of rotation tuning fixes it — you need residential or mobile.

| Proxy type | Best for | Trade-off |
| --- | --- | --- |
| Datacenter | Lenient targets, high volume, price monitoring | Fast and cheap; easily fingerprinted; limited locations |
| Residential | Protected sites, geo-specific content, ad verification | Best concealment; slower; billed per GB |
| ISP | Logged-in sessions, long-lived sessions needing a stable IP | Between the two on cost and detectability |
| Mobile | Mobile-only content, hardest targets | Scarce, expensive, and consent questions around device owners |

Mobile IPs are the one category where I'd push back on most use cases. They're hard to source, priced well above residential, and for a typical product-page or SERP scrape they buy you nothing that residential doesn't. Protocol choice is simpler: HTTPS for standard scraping, SOCKS5 only when you need non-web traffic or UDP, which rules out most hosted scraping APIs including this one.

## What residential proxies really cost per GB

The $1.40–$8/GB range is a third-party aggregate across providers, and it hides the mechanic that decides your invoice: unit price is a function of committed monthly volume.

Decodo is the cleanest example because its published tiers can be checked against each other. The headline "from $2/GB" only applies at the **1,000GB/month, $2,000/month** commitment level. The entry tier is 3GB at **$3.75/GB**. A tester scraping 20GB a month pays nearly double the advertised rate and never reaches the volume where the discount exists.

The same pattern shows up across the category. For reference, verified list prices from other providers:

- **Bright Data:** residential from $2.50/GB, datacenter from $0.90/IP, ISP from $1.30/IP; 400M+ residential IPs, city/ZIP/ASN targeting.
- **Oxylabs:** residential from $2.50/GB at volume, $6/GB on the entry plan; 175M+ IPs.
- **SOAX:** residential from $2/GB at volume, $90 starter plan, no pay-as-you-go.
- **IPRoyal:** residential from $1.75/GB at volume, $7/GB pay-as-you-go, traffic doesn't expire.
- **Webshare:** residential from $1.40/GB at high volume; 80M+ IPs; self-serve signup.
- **Rayobyte:** residential from $3/GB at volume, $15/GB entry; under 1M residential IPs.
- **Decodo:** 125M+ IPs; residential $3.75/GB at 3GB, $2/GB only at the $2,000/month tier.

Note what the per-GB model does to small projects. At 20GB/month you're looking at $50–$160 depending on provider, and you still own the rotation logic, the retry logic, the user-agent rotation, the ban classification, and the session handling. Credit-based API pricing inverts this: no volume commitment, no per-GB meter, cost scales with request count and difficulty, and the operational work sits with the vendor.

That inversion is why the plan comparison matters more than the per-GB comparison for anyone under roughly a million requests a month.

## Free proxy lists: fine for a smoke test, wrong for anything scheduled

Public proxy lists are not a budget option for production scraping. The specific failure modes:

- **Plaintext exposure.** Most free proxies don't support HTTPS to the proxy itself, so credentials, cookies, and form data travel in the clear. Some strip HTTPS, inject scripts, or log everything passing through. Never send an authenticated request through one.
- **Lifespan measured in hours.** An IP that responds at 9am is frequently dead by lunch. What stays up is slow and shared with thousands of users.
- **Already blocklisted.** The same free IPs are used by everyone, so Cloudflare and DataDome have them catalogued. Yesterday's working proxy is today's 403.
- **Honeypots and botnets.** Some exist to harvest credentials; some residential "free" proxies are compromised consumer devices whose owners never consented.

The numbers back this up. Geonode's own free list advertises 30–50% uptime with 500–3,000ms response times, against 99.9% SLA and ~320ms on paid tiers. Zyte notes public proxies are often malware-infected. Bright Data's FAQ reaches the same conclusion: slow, unstable, frequently logging traffic.

If your budget is genuinely zero, use a real free tier instead of a list: Webshare gives 10 proxies and 1GB/month permanently; HasData offers a 1,000-credit trial before $49/month; ScraperAPI's free plan gives 1,000 credits per month (5,000 in the first week) with 5 concurrent threads. 👉 [Test ScraperAPI free for 7 days at 5,000 credits](https://www.scraperapi.com/?fp_ref=coupons)

## Who should buy a scraping API instead of a proxy pool

The six jobs a proxy pool imposes on you are: identifying ban types (CAPTCHA, redirect, block, ghosting), retrying failures on a new IP, managing user agents, maintaining sticky sessions where needed, randomizing delays and throttling, and restricting the pool by geography per site. Managing 5–10 proxies by hand is fine. Managing thousands is a system.

ScraperAPI's pitch is that all six are handled and you send requests to one endpoint. For teams without a dedicated infrastructure engineer, that's the real comparison — not $/GB against $/credit, but engineering hours against subscription cost.

**ScraperAPI fits when:** your code already works and you need a reliable fetch layer; you need country-level content and don't want to maintain geo-pools; you want predictable per-request billing instead of a GB meter; you're scraping SERPs, marketplaces, or real estate listings at moderate volume.

**It doesn't fit when:** you need a long-lived fixed residential IP for logged-in account work (session support exists, but this isn't a sticky-session proxy product); you need SOCKS5 or non-HTTP traffic; you need city or ZIP-level targeting, which ScraperAPI doesn't offer at any tier; or you're moving tens of terabytes of raw HTML where per-GB residential pricing from a specialist would undercut per-credit costs.

One more constraint: anything above 3 million API credits per month at the top of the standard range means a sales conversation, and Enterprise pricing is quote-only.

## FAQ

**What's the actual difference between residential and datacenter proxies?**
Datacenter IPs come from cloud and hosting ranges — fast, cheap, and easy for anti-bot systems to identify as non-residential. Residential IPs route through consumer connections, so they look like ordinary visitors and survive detection longer, at higher cost and lower speed.

**Is $1.40/GB a real residential price?**
It's a real volume-tier price at Webshare, not an entry price. Expect $3–$7/GB at small volumes from most providers. Anyone quoting a sub-$2 figure without naming the commitment is quoting a tier you probably won't reach.

**Can I just use free proxies?**
For a one-off test of whether your scraper handles a proxy connection, yes. For anything scheduled or anything touching a login, no — the security and reliability failures are documented, not hypothetical.

**How does geotargeting work in ScraperAPI?**
Add `country_code=xx` to the request (for example `country_code=fr`), and the request routes through that country's pool with automatic rotation. It costs no extra credits. Country-level targeting requires the Business plan or above; Hobby and Startup are limited to US and EU.

**A few thousand requests a month — proxy or API?**
API. At that volume you'd pay $50–$150 for a residential plan and still write the rotation and retry logic. ScraperAPI's Hobby plan at $49 covers 100,000 standard requests with that logic included.

## Before you check out

Match the plan to the multiplier, not the credit headline. If your targets are plain HTML, Hobby's 100,000 credits go a long way. If most of your pages need rendering or premium IPs, divide by 10 or 25 and size accordingly — a 10,000-page rendered job needs Startup, not Hobby. If you need non-US/EU geotargeting, budget for Business at $299 regardless of volume.

Two things to confirm before paying: whether the free allocation covers a realistic test of your hardest target (1,000 monthly credits won't, but the 5,000-credit first week might), and how overage behaves on your tier — Hobby, Startup, and Business pause at 429 unless auto-upgrade is on, so a mid-month stop is the default, not a surprise bill. The 7-day refund policy covers you if the success rate on your specific targets doesn't hold up.

👉 [Compare all ScraperAPI plans and start with 5,000 free credits](https://www.scraperapi.com/pricing/?fp_ref=coupons)
