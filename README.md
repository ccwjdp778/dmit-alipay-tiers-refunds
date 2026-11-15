# VPS with Alipay: Does DMIT Accept It, Which Tier Do You Actually Need, and What Happens If You Want a Refund? (Lite vs Pro, Checkout, Cancellation)

Yes. DMIT accepts Alipay at checkout, alongside UnionPay, credit cards and PayPal, and its client area and order pages are available in Chinese, so an order can be completed without an international card. Two things matter more than the payment method: DMIT sells premium China-optimized routing (CN2 GIA, China Unicom 9929, China Mobile CMI) rather than cheap compute, and once a server is provisioned you should assume the money is gone.

What follows covers how the Lite and Pro lines differ, what the Alipay checkout actually looks like, the cancellation mechanics that catch people out, and when a cheaper Alipay-friendly host is the better call.

> **Short answer for most readers:** start on a **Lite plan billed monthly**, not annually. It is the cheapest way to test whether DMIT's routing helps your traffic, and a monthly cycle is the only exit that doesn't involve writing off a prepaid year. Third-party reviews from late 2025 put entry Lite instances at roughly $6–7/month; treat that as a ballpark, since DMIT prices differ by location and get adjusted.
> 👉 [Check current DMIT plans and Alipay checkout](https://www.dmit.io/aff.php?aff=9081)

## The full DMIT lineup, and what separates the tiers

DMIT sells two product families, and the difference between them is network path, not CPU count. **Lite** runs on standard transit and is the budget entry point. **Pro (Premium)** puts you on the optimized routes: CN2 GIA to mainland China plus Unicom and Mobile optimized transit, sold from Los Angeles, Hong Kong and Tokyo. Hardware is the same story across the board: AMD EPYC processors, enterprise SSD in RAID-10, no heavy overselling.

| Tier | Network | Location | Who it fits | Price |
| --- | --- | --- | --- | --- |
| Lite | Standard transit | Varies by stock | Testing, low-traffic sites, anything not latency-sensitive | Reported entry around $6–7/mo; confirm live |
| Pro | CN2 GIA + Unicom/Mobile optimized | Los Angeles | Mainland-facing sites, APIs, remote desktop | Reported from ~$39.90/quarter; confirm live |
| Pro | CN2 GIA + optimized Asia transit | Hong Kong | Lowest latency to southern China | Varies by location; confirm live |
| Pro | CN2 GIA + optimized Asia transit | Tokyo | Japan-facing and Asia-Pacific traffic | Varies by location; confirm live |

👉 [See the live price for each tier and location](https://www.dmit.io/aff.php?aff=9081)

Two practical notes on that table. Pro low-configuration plans go out of stock regularly, because the optimized bandwidth is the scarce part, so the plan you want may simply not be orderable today. And bandwidth allowances are finite. Premium routes are the expensive part of the bill, and traffic beyond the plan allowance is billed by usage, which is why static files and backups belong on cheap object storage rather than on the DMIT box.

## Alipay checkout, step by step

DMIT runs its billing on WHMCS, the standard hosting panel, so the flow follows the usual pattern rather than a custom one. Create an account in the client area with an email address. Pick location, tier and billing cycle. Choose the OS template and any add-ons (extra IPv4 or IPv6 addresses, a DDoS-filtered IP). Then check out, select Alipay as the payment method, and scan the QR code in the Alipay app. Once payment clears, the invoice is marked paid and the instance is provisioned automatically with the system and location you selected.

Two things to confirm on the invoice screen before you scan. First, whether the total differs from the plan price. Some hosts add a percentage for Alipay specifically; Infrawire, for example, discloses a 3% surcharge on Alipay while card, PayPal, crypto and bank transfer stay fee-free. DMIT does not publish an equivalent surcharge that I can verify, so read the line items rather than assuming either way. Second, whether any promotion you're using survives the payment switch. At least one provider restricts bonus-credit style promo codes to card and PayPal, and Alipay orders get rejected. If you're applying a code, apply it and confirm the discounted total still shows with Alipay selected.

Renewals arrive as invoices before the due date and can be settled with Alipay or another method, so you aren't locked into the same channel forever.

## Cancellation is easy. Getting money back is not.

This is the part that decides whether you should buy annually, and it's worth reading before the first payment.

DMIT's terms treat promo, sale and special plans as non-refundable in any form. Regular full-price plans can be requested for refund only within a narrow window and under conditions (IP not blocked by the firewall, no abuse record). Once a service has been provisioned and delivered, the default position is no refund, which is normal for this segment: the resources were allocated the moment the VM booted.

Cancellation itself is a separate action from a refund, and it's done manually: log into the client area, go to Services, open the instance, and choose Request Cancellation. You then pick **Immediate** or **End of Billing Period**. Immediate terminates the server within hours and wipes the data with no grace period. End of Billing Period lets the instance run until the paid term expires and then shuts it down. Neither option returns a prorated amount for unused days, so if you've already paid, there's no financial reason to terminate early.

Two consequences follow. Your IP goes back into the pool on termination, including any premium-route address, and it cannot be reserved. If domains point at that IP, update DNS before or immediately after. And if you're cancelling because speeds dropped, open a ticket first: routing problems on optimized paths are sometimes temporary or fixable on the provider's side, and that's cheaper than migrating.

The obvious conclusion: if you're unsure about DMIT, buy monthly. The per-month rate is higher than the annual equivalent, but the downside is capped at one month instead of twelve.

👉 [Start with a monthly plan and test the route yourself](https://www.dmit.io/aff.php?aff=9081)

## What the Pro premium actually buys you

DMIT's pricing makes sense only if latency to mainland China is a business metric for you. A WooCommerce store with Chinese buyers, a payment callback endpoint, a jump box a distributed team uses all day, a small voice or video signaling server: these are cases where packet loss at 9pm costs more than the hosting difference. The Lite line exists for everything else.

Skip Pro if your traffic comes mostly from Europe or North America, if you're serving large files or video, or if you're still learning Linux. DMIT is unmanaged. Support covers the hypervisor, power, panel and network; nobody is going to configure your web server or tune MySQL for you.

One configuration detail worth acting on after provisioning: on a 1 vCPU / 1 GB Pro instance running WordPress with MySQL, create a 1 GB swap file and set `swappiness` to 10. It won't make the box faster, but it keeps MySQL from being killed when memory spikes.

## Three Alipay-friendly alternatives, if premium routing is wasted on you

All figures below are from each provider's own published pricing or plan pages, checked recently. No links, because these are for comparison only.

| Provider | Entry price | Billing | Alipay notes | Refund |
| --- | --- | --- | --- | --- |
| LightNode | $7.71/mo (1 vCPU, 2 GB, 50 GB NVMe, 1 TB) | Hourly | Alipay, PayPal, card, Google Pay; no card required | Not stated |
| Infrawire | €8.30/mo (2 cores, 4 GB, 40 GB NVMe, unlimited traffic) | Monthly, no commitment | Alipay adds a disclosed 3% fee; other methods free | 7-day money-back |
| LisaHost | from ¥199/yr (1 core, 1 GB, 10 GB) | Mostly annual | Alipay default, WeChat Pay and card also accepted | 48 hours, ticket required |

The billing granularity matters more than the sticker price. LightNode's hourly billing means a wrong plan can be torn down and redeployed the same day. LisaHost's annual-first model with Chinese-language panels and a 48-hour refund window suits people who want a Chinese interface and a cheap year rather than optimized routing. Infrawire's 3% Alipay fee is the clearest example of a cost that never appears in a headline price.

## Who should not buy DMIT

Anyone whose main constraint is budget. DMIT doesn't compete on price and has no throwaway tier for learning commands. Anyone who needs a managed service, a Chinese control panel as the deciding factor, or a comfortable refund window should look at the alternatives above instead. And anyone who wants a specific Pro configuration today may find it out of stock; that's a supply characteristic of optimized bandwidth, not a temporary glitch.

## How to decide

If you only want to see whether an optimized route improves your connection, take the cheapest Lite plan monthly and measure before committing. If you're running a production site whose visitors are mostly in mainland China, go Pro in Los Angeles or Hong Kong, monthly for the first cycle, and only move to quarterly or annual billing once you've confirmed the route holds up at peak hours. If you're running several services on one box, size memory and bandwidth first, because overage on premium transit is the line item that surprises people.

Before you pay, confirm two things on the invoice: the final total with Alipay selected, and the refund terms attached to the specific plan in your cart. Sale plans are the ones with no way back.

👉 [Compare DMIT's Lite and Pro plans on the official order page](https://www.dmit.io/aff.php?aff=9081)
