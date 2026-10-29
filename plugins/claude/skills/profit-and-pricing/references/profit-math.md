# Profit math

## Contents

- Profit and cash are different questions
- Formulas
- Where money usually leaks
- Report format
- Setting a price

## Profit and cash are different questions

"I have sales but no money" has two possible answers. Work out both.

- **Profit**: what is left from the orders after every cost.
- **Cash timing**: profit that has not reached the bank yet, or money spent before it comes in. Check for payouts still pending; reserves or holds on the payment account, which are common on new accounts (see [rules.md](rules.md)); supplier orders paid as soon as customers order while payouts arrive days later; ad platforms billing on their own schedule; and growth, which ties up more cash in supplier payments each week.

## Formulas

DSers gives revenue and supplier cost per order. Everything else comes from the user. Show each formula with the user's numbers, and round only the final results.

- Supplier cost (from DSers) = product cost + supplier shipping, as DSers recorded it. Add duties and import fees the seller pays per parcel, if any; ask whether supplier prices include them.
- Payment fee = amount charged × rate + fixed fee.
- Ad cost per order = ad spend billed in the period ÷ orders in the period. Use what the ad platform billed, not the revenue it reports.
- Profit per order = revenue − supplier cost − duties − payment fee − ad cost per order − other per-order costs.
- Refund cost: a refunded order usually still costs its payment fee, because Shopify Payments and PayPal do not return fees on refunds (see [rules.md](rules.md)), and it costs the product and shipping if the item had already shipped. A chargeback also costs a fee.
- Profit for the period = sum of profit per order − refund and chargeback costs − fixed costs.
- Margin = profit ÷ revenue.
- Break-even ad cost per order = revenue − supplier cost − duties − payment fee − other per-order costs.
- Break-even ROAS = revenue ÷ break-even ad cost per order.

Example with an assumed payment rate of 2.9% + $0.30, to be replaced by the user's own: revenue $29.99, supplier cost $12.70, payment fee $1.17, ads $900 ÷ 120 orders = $7.50. Profit per order = 29.99 − 12.70 − 1.17 − 7.50 = $8.62, before refunds and fixed costs. Break-even ad cost per order = $16.12.

## Where money usually leaks

Go through this list and say which items apply, with the amount where the numbers allow:

- The supplier raised the price after the product was priced, or some variants cost more than the one used for pricing. `DSers:list_supplier_product_change_notifications` shows cost changes from the last 30 days.
- The store offers free shipping but the price does not cover shipping.
- Duties or import fees on parcels to the US, which since August 2025 can apply to every parcel (see [rules.md](rules.md)), are not in the cost.
- Fees kept on refunded orders, and chargeback fees.
- Shopify's extra transaction fee on third-party payment gateways (see [rules.md](rules.md)).
- Discount codes and free-shipping thresholds that bring the average order below the list price. DSers revenue is what the customer actually paid, so compare it with the list price.
- Currency conversion, when the store, payout, and supplier currencies differ. Ask the user for their provider's conversion terms; do not state a rate.
- Ad spend counted from the platform's reported results instead of what was billed.
- Apps and subscriptions that add up.
- Orders reshipped or refunded because of supplier problems.
- Orders DSers flags with cost higher than revenue.

## Report format

1. A one-line answer first: making money or not, and roughly how much, with the main assumption.
2. A table per product: orders, revenue, supplier cost, fees, ad cost, refunds, profit per order, and margin. Mark products that lose money and products below the user's target margin.
3. For each problem product: the price that reaches the target margin, and the highest ad cost per order at today's price. If the needed price is unrealistic, say so; the other options are a cheaper supplier or shipping method, a different product, or stopping ads for it.
4. The cash-timing items that apply, separate from profit.
5. What to keep doing: record the cost per item in the store so the store's profit reports work (see [rules.md](rules.md)), and recheck supplier costs regularly.

## Setting a price

- Price for a target margin = (supplier cost + duties + ad cost per order + fixed payment fee + other per-order costs) ÷ (1 − target margin − payment fee rate).
- Markup = (price − supplier cost) ÷ supplier cost. Margin = profit ÷ price. Users often mix them up; say which one you mean.
- After applying a price ending such as .99, check the margin again.
- Compare-at ("was") prices must be real former prices. In the US, the FTC guide expects a price the item was openly offered at for a reasonably substantial period; in the EU, an announced price reduction must show the lowest price of the previous 30 days (see [rules.md](rules.md)).
