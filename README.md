# BandwagonHost data centers: the full list of locations, how CN2 GIA routes actually differ, and which plan to buy in which机房-less plain English

BandwagonHost (搬瓦工) runs one of the more confusing location lineups in the VPS world. Six separate facilities in Los Angeles alone, two in Hong Kong, two in Amsterdam, and a pile of cryptic codes like `USCA_9` and `EUNL_9` in the control panel. Pick the wrong one and you either overpay for a premium route you don't need, or wonder why your "fast" VPS crawls every evening.

This guide covers all 19 data centers currently operated by BandwagonHost (IT7 Networks), what the route names actually mean, how to test latency before spending a cent, and which plans — from the $49.99/year entry KVM to the Hong Kong CN2 GIA flagships — are available in which location.

## The complete list: all 19 BandwagonHost data centers

The codes below are what you'll see in KiwiVM, the in-house control panel. Route names in parentheses are how most people refer to them.

| # | Code | Location | Route / Network | Typical port |
| --- | --- | --- | --- | --- |
| 1 | HKHK_3 | Hong Kong (HK85 facility) | CMI (China Mobile International) | 1 Gbps |
| 2 | HKHK_8 | Hong Kong | CN2 GIA | 1 Gbps |
| 3 | JPOS_1 | Osaka, Japan | Softbank | 10 Gbps |
| 4 | JPTYO_8 | Tokyo, Japan | CN2 GIA | 1–1.2 Gbps |
| 5 | USCA_2 | Los Angeles (DC2) | QNET, standard | 1 Gbps |
| 6 | USCA_3 | Los Angeles (DC3) | CN2 (regular) | 1 Gbps |
| 7 | USCA_4 | Los Angeles (DC4) | MCOM, standard | 1 Gbps |
| 8 | USCA_6 | Los Angeles (DC6) | CN2 GIA-E | 2.5 Gbps |
| 9 | USCA_8 | Los Angeles (DC8) | ZNET, standard | 1 Gbps |
| 10 | USCA_9 | Los Angeles (DC9) | CN2 GIA / CMIN2 / Unicom Premium | 2.5 Gbps |
| 11 | USNY_2 | New York | Standard | 1 Gbps |
| 12 | USNJ | New Jersey | Standard | 1 Gbps |
| 13 | USCA_FMT | Fremont, CA | Standard | 1 Gbps |
| 14 | USCA_FMT8 | Fremont-8, CA | Standard | 1 Gbps |
| 15 | CABC_1 | Vancouver, Canada | Standard | 1 Gbps |
| 16 | CABC_6 | Vancouver, Canada | CN2 GIA | 2.5 Gbps |
| 17 | EUNL_3 | Amsterdam, Netherlands | Standard | 1 Gbps |
| 18 | EUNL_9 | Amsterdam, Netherlands | China Unicom AS9929 | 2.5 Gbps |
| 19 | AUSYD_1 | Sydney, Australia | Standard | 1 Gbps |

A newer Manhattan facility, `USNY_6` (CoreSite NY1 at 32 Avenue of the Americas, with DE-CIX, Cloudflare, Google and Facebook peering), has also been rolling out across plan order pages, so the count keeps inching upward. The bandwagonhost.com order pages are the source of truth for which rooms are open on which plan at any given moment.

## What the route names actually mean

This is where most confusion lives, so it's worth a minute. BandwagonHost's own CN2 GIA explainer is blunt about the situation: regular routes into mainland China get congested during peak hours, and packet loss on cheap transit can hit 30% or more in the evening. Whether that matters depends entirely on who your VPS is talking to.

**CN2 GIA (AS4809)** is China Telecom's top-tier commercial backbone. It's priority-routed and avoids the public peering points where congestion happens. It's also genuinely expensive to operate — BandwagonHost notes transit costs on this network can run as high as $120 per megabit, which explains why CN2 GIA plans cost several times more than identical hardware on standard routes. Two honest downsides the company itself lists: limited capacity, and no DDoS tolerance (attacks get null-routed rather than absorbed).

**CN2 GIA-E** is the extended variant. The practical difference: E-plans can roam across a much wider set of data centers — the current e-commerce lineup covers locations from Los Angeles to Amsterdam — and the port speeds run higher (2.5 Gbps and up, versus 1 Gbps on classic CN2 GIA plans).

**CTGNet (AS23764)** is China Telecom's newer backbone. BandwagonHost treats it as practically equivalent to CN2 GIA in both price and performance, and the DC9 facility sends China-bound traffic to three carriers at once: CN2 GIA, CMIN2 (China Mobile, AS58807) and China Unicom Premium (AS10099). That multi-carrier setup is why DC9 is usually the recommended room when one Chinese ISP can't be singled out.

**AS9929** is China Unicom's premium backbone — the same idea as CN2 GIA, different carrier. The EUNL_9 Amsterdam room uses it, which is why a European VPS can still have decent China connectivity.

**CMI** is China Mobile's international network. Solid, and usually the lowest-latency option for China Mobile subscribers, though generally not as fast to mainland Telecom lines as CN2 GIA.

**Softbank** powers the Osaka room with a fat 10 Gbps pipe. Good general Asian performance; not specifically optimized for mainland China routing.

**Everything else** — Fremont, New York, New Jersey, Sydney, and the standard LA rooms — runs ordinary commercial transit. Perfectly fine for websites serving North American or European audiences, and noticeably cheaper.

## Which data center fits which use case

Match the room to your traffic, not to the shiniest name.

- **Mainland China audience, budget allows:** USCA_9 (DC9 CN2 GIA) for the best overall capacity and stability, USCA_6 (DC6 CN2 GIA-E) for flexibility across many rooms, or the Hong Kong CN2 GIA (HKHK_8) and Tokyo CN2 GIA (JPTYO_8) rooms when raw latency matters more than price — expect to pay substantially more for the Asian endpoints.
- **China Mobile users specifically:** Hong Kong CMI (HKHK_3) or Tokyo CMI plans tend to have the lowest latency for that network.
- **General Asian traffic without China focus:** Osaka Softbank (JPOS_1) — high bandwidth, reasonable cost.
- **North American audience:** pick geographically. West Coast → LA or Fremont; East Coast → New York or New Jersey.
- **European audience:** Amsterdam. Add the EUNL_9 AS9929 room if you also want a good path back to China.
- **Mixed or unknown audience:** a US West Coast room is a reasonable default, and you can always migrate later (more on that below).

## Test before you buy: Looking Glass and test IPs

BandwagonHost provides a Looking Glass for each data center, so you can check the actual return route from your own connection before committing. Community-maintained speed test pages add per-room download tests and latency checks — for example, the DC9 CN2 GIA test IP is 89.208.246.192 with an associated speedtest host, and DC2 QNET's is 104.225.153.186. Run ping and a download test from your local network during the evening hours; peak-hour numbers are the ones that matter for China routes.

One thing worth knowing upfront: any plan you buy can be moved between data centers later, so the test is about picking the right *starting* room, not making an irreversible bet.

## The plans, tier by tier

BandwagonHost's catalog splits into three always-available product lines, plus a rotating cast of limited edition plans. All plans are KVM, self-managed, run through KiwiVM, and include 1–10 Gbps uplinks, weekly security audits, and OS choices covering AlmaLinux, RockyLinux, CentOS, Debian, Ubuntu, CentOS Stream and Fedora. Setup is instant, there's a 99.9% uptime guarantee, and a 30-day refund policy if the route doesn't work out.

### Basic VPS — the budget line

Available in Fremont, Los Angeles, New York and other standard-route locations. As verified on the official order pages:

| Plan | SSD | RAM | CPU | Transfer | Port | Price | Billing |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Basic 1 | 20 GB | 1 GB | 2x | 1 TB/mo | 1 Gbps | $49.99 | /year |
| Basic 2 | 40 GB | 2 GB | 3x | 2 TB/mo | 1 Gbps | $52.99 | /half year |
| Basic 3 | 80 GB | 4 GB | 4x | 3 TB/mo | 1 Gbps | $19.99 | /month |
| Basic 4 | 160 GB | 8 GB | 5x | 4 TB/mo | 1 Gbps | $39.99 | /month |
| Basic 5 | 320 GB | 16 GB | 6x | 5 TB/mo | 1 Gbps | $79.99 | /month |
| Basic 6 | 480 GB | 24 GB | 7x | 6 TB/mo | 1 Gbps | $119.99 | /month |

The $49.99/year plan works out to about $4.17 a month — 1 GB RAM and 20 GB SSD is modest, but as a cheap, real KVM box from a provider founded in 2012, it's the entry point most people start with. If your audience isn't in mainland China, the standard-route Basic line is all you need. 👉 [查看全部 Basic 套餐和机房](https://bit.ly/BandwagonHost)

### E-Commerce (CN2 GIA-E) — the China-optimized middle ground

This is the line that made BandwagonHost's reputation: CN2 GIA / CMIN2 / China Unicom Premium connectivity in most locations, including the LA DC9 room, with 2.5–10 Gbps ports and free migration across the supported room list.

| Plan | SSD | RAM | CPU | Transfer | Port | Price | Billing |
| --- | --- | --- | --- | --- | --- | --- | --- |
| EC 1 GB | 20 GB | 1 GB | 2x | 1 TB/mo | 2.5 Gbps | $49.99 | /quarter ($169.99/year) |
| EC 2 GB | 40 GB | 2 GB | 3x | 2 TB/mo | 2.5 Gbps | $89.99 | /quarter |
| EC 4 GB | 80 GB | 4 GB | 4x | 3 TB/mo | 2.5 Gbps | $56.99 | /month |
| EC 8 GB | 160 GB | 8 GB | 6x | 5 TB/mo | 5 Gbps | $86.99 | /month |
| EC 16 GB | 320 GB | 16 GB | 8x | 8 TB/mo | 5 Gbps | $159.99 | /month |
| EC 32 GB | 640 GB | 32 GB | 10x | 10 TB/mo | 10 Gbps | $289.99 | /month |
| EC 64 GB | 1 TB | 64 GB | 12x | 12 TB/mo | 10 Gbps | $549.99 | /month |
| EC 64 GB+ | 1 TB | 64 GB | 12x | 15 TB/mo | 10 Gbps | $679.00 | /month |
| EC 64 GB Pro | 1 TB | 64 GB | 12x | 20 TB/mo | 10 Gbps | $899.00 | /month |

The entry CN2 GIA-E plan at $49.99/quarter is the most common choice for China-targeted projects: 2 cores, 1 GB RAM, 20 GB SSD, 1 TB of monthly transfer. It costs roughly four times the Basic equivalent — that difference is the price of the CN2 GIA transit. 👉 [查看 CN2 GIA-E 电商套餐](https://bit.ly/BandwagonHost)

### Ultra — Hong Kong and Tokyo CN2 GIA flagships

The Ultra line serves the Hong Kong (HKHK_8, Equinix HK2) and Tokyo (JPTYO_8, Equinix) rooms. BandwagonHost describes it as "absolute best, no-compromise connectivity to China, with lowest possible latency," and the pricing reflects that.

| Plan | SSD | RAM | CPU | Transfer | Port | Price |
| --- | --- | --- | --- | --- | --- | --- |
| Ultra 2 GB | 40 GB | 2 GB | 2x | 500 GB/mo | 1 Gbps | $89.99/mo |
| Ultra 4 GB | 80 GB | 4 GB | 4x | 1 TB/mo | 1 Gbps | $155.99/mo |
| Ultra 8 GB | 160 GB | 8 GB | 6x | 2 TB/mo | 1 Gbps | $299.99/mo |
| Ultra 16 GB | 320 GB | 16 GB | 8x | 4 TB/mo | 1 Gbps | $589.99/mo |
| Ultra 32 GB | 640 GB | 32 GB | 10x | 6 TB/mo | 1 Gbps | $989.99/mo |
| Ultra 64 GB | 1 TB | 64 GB | 12x | 8 TB/mo | 1 Gbps | $1,889.99/mo |

The Tokyo room runs a similar table with a 1.2 Gbps port on the entry plan. For most people this line is overkill — you're paying Hong Kong/Tokyo real-estate prices for sub-50ms latency that the LA DC9 room gets close to at a fraction of the cost. It makes sense when latency to China genuinely translates into money. 👉 [查看香港/东京 Ultra 套餐](https://bit.ly/BandwagonHost)

### Limited edition plans — the quarterly lottery

On top of the regular catalog, BandwagonHost drops small-batch annual plans that sell out and restock irregularly. They're route-locked (you generally can't migrate them between rooms), but the per-year pricing beats the regular catalog by a wide margin, and renewals stay at the original price. A few documented examples from the community tracker, with the officially supported `pid` deeplink structure:

| Plan | Config | Route | Price |
| --- | --- | --- | --- |
| [MINICHICKEN](https://bandwagonhost.com/aff.php?aff=79616&pid=158) | 1 core / 1 GB / 20 GB / 1 TB | HE transit, Fremont | $19/yr |
| [MEGABOX-PRO](https://bandwagonhost.com/aff.php?aff=79616&pid=157) | 2 AMD cores / 2 GB / 40 GB / 2 TB | CN2 GIA + CMIN2 | $49/yr |
| [THE PLAN 2024](https://bandwagonhost.com/aff.php?aff=79616&pid=147) | 2 cores / 2 GB / 40 GB / 1 TB | CN2 GIA-E, 18 data centers | $99/yr |
| [The Tokyo Plan v2](https://bandwagonhost.com/aff.php?aff=79616&pid=163) | 2 AMD cores / 2 GB / 40 GB / 1 TB | CMI Tokyo, 5 Gbps | $99/yr |

Note two things. First, stock is the whole game — a plan listed in a review may be gone by the time you click, and the cart page is the only reliable stock indicator. Second, BandwagonHost's Terms of Service caps limited edition plans at 30% of one CPU core for sustained load (45% for THE PLAN family), so they're built for routing, light sites and dev boxes, not constant CPU work. 👉 [查看当前限量版套餐库存](https://bit.ly/BandwagonHost)

## Free migration between data centers

This feature does more work than it gets credit for. Any VPS on a standard plan can be migrated between its eligible data centers from the KiwiVM panel — no support ticket, no fee, no data loss, and per the official migration-optimization changelog, the transfer footprint between rooms has been getting smaller. It takes minutes. Practical upshot: start in the room your tests favor, and if real-world evening performance disappoints, move. The main exceptions are limited edition plans, which are route-locked by design.

## Promo codes and payment details

BandwagonHost runs persistent circulating coupon codes rather than big one-off sales. Multiple community trackers (bwghost.me, offersloc.com, vpsgo.com and others) list a rotating set of roughly 6.77–6.81% recurring codes — BWHCGLUKKB at 6.77% and BWHNCXNVXV at 6.81% are the most frequently cited, and these apply to renewals too, which is where the real savings accumulate. The catch: these codes expire and get replaced often enough that you should confirm the discount actually applies in the cart before paying. During seasonal events (Chinese New Year, Double 11, Black Friday) larger sitewide discounts appear — the 2026 Chinese New Year promotion, for instance, took 6.77% off sitewide with entry plans dropping to about $46.61/year.

Payment goes through card, PayPal, Alipay or UnionPay, and activation is instant once payment clears.

## FAQ

**How many data centers does BandwagonHost have?** Nineteen as of now, per the community-maintained location tracker: ten in the US (six of them in Los Angeles), two in Hong Kong, two in Japan, two in Vancouver, two in Amsterdam, and one in Sydney — with a new Manhattan facility (USNY_6) recently added to order pages.

**Is migration between data centers really free?** Yes. The official order pages state it directly: "VPS can be migrated between locations anytime free of charge, without data loss." It runs from the KiwiVM panel and takes minutes.

**Which data center is fastest for mainland China?** For Telecom-heavy traffic, DC9 CN2 GIA (USCA_9) is the default recommendation — it sends China-bound traffic over three premium carriers simultaneously. Hong Kong CN2 GIA and Tokyo CN2 GIA are lower-latency but much more expensive. Test from your own connection using the per-room Looking Glass before deciding.

**Can I switch data centers after buying?** On standard plans, yes, freely, from the control panel. Limited edition plans are the exception — they're locked to their assigned route/room.

**What if the route doesn't work for me?** There's a 30-day refund policy on all plans, which is enough time to run evening-hour latency tests and back out if the numbers don't justify the price.

## The bottom line

Nineteen locations sounds like a lot, but the decision tree is short. Figure out where your traffic comes from. If it's mainland China, you're choosing between the CN2 GIA family — LA DC9 at $49.99/quarter as the balanced pick, Hong Kong/Tokyo Ultra when latency is worth $90+/month — and running an evening speed test before committing. If it's not, a $49.99/year Basic plan in the region closest to your users is the sensible answer, and free migration covers you if the first guess is wrong. 👉 [查看 BandwagonHost 全部套餐和机房](https://bit.ly/BandwagonHost)
