# Order problems after the customer paid

Use this when an item is out of stock or unavailable after the customer paid, when a supplier cancels, or when a customer wants to cancel or return. Laws and platform rules mentioned here are in [rules.md](rules.md) with their official sources; name the country or platform and give the link when you rely on one.

## Out of stock, or cancelled by the supplier

1. **Get the facts** from `DSers:get_order`: the item, the supplier and variant, the destination, the order date, and whether the supplier order was placed or paid. Ask the user what delivery time the store promised.
2. **Find a replacement for this order**, cheapest first in effort:
   - another variant of the same supplier product, if it is the same item (`DSers:get_supplier_product`);
   - the backup supplier, if the product has one (`DSers:get_managed_store_product` shows the main and backup suppliers);
   - the same item from another supplier (`DSers:search_supplier_products`, by keyword or by a public image link).
   For each option, show the cost, the shipping cost and time to the customer (`DSers:list_supplier_product_shipping_methods_and_cost`), stock, and how it differs from what the customer ordered. Paying a little more to keep the order can cost less than a refund, a dispute, or a lost customer; show the numbers.
3. **Switch this order** with `DSers:update_order_supplier`. Show the old and new supplier, product, variant, price, shipping, and the change in cost. Ask the user whether a later change to the product's supplier link may replace this manual choice, and pass their answer. This changes this order only; the buyer's ordered variant and the product's supplier link stay the same.
4. **Fix future orders** if the problem will repeat. With the supplier-mapping skill, change the product's supplier, add a backup supplier, or set the variant's stock to zero so the store stops selling it.
5. **Tell the customer** before they ask, using the message pattern below. Tell them only if what they will receive differs, or if it will arrive later than promised.
6. **If nothing works**, cancel and refund the customer in full promptly in the store, and say why. If the supplier cancelled after being paid, check that the supplier refunded the seller.

If delivery will be late and the customer is in the US, the [FTC Mail, Internet, or Telephone Order Merchandise Rule](https://www.ftc.gov/business-guidance/resources/business-guide-ftcs-mail-internet-or-telephone-order-merchandise-rule) requires the seller to ask the customer to agree to the delay, and to refund without being asked if they do not agree. In any country, offer the options: wait until a stated date, a similar item, or a full refund.

## The customer wants to cancel or return

- **Not yet placed with the supplier**: refund in the store and leave the order unplaced.
- **Placed but not shipped**: try to cancel the supplier order. DSers Agent and 1688 Dropshipping orders that are awaiting payment or shipment can be cancelled with `DSers:cancel_supplier_order`; for other suppliers, the user cancels with the supplier. Then refund the customer.
- **Shipped or delivered**: apply the store's policy and the customer's legal rights. Customers in the EU ([Consumer Rights Directive](https://eur-lex.europa.eu/eli/dir/2011/83/oj)) and UK ([GOV.UK](https://www.gov.uk/online-and-distance-selling-for-businesses)) can withdraw within 14 days of delivery without giving a reason, and in the EU the seller must refund within 14 days of being told, including standard delivery costs. A store policy cannot remove these rights. Compare the cost of return shipping with the item's cost before asking for the item back.

Refunds happen in the store, not in DSers. A refunded order usually still costs the payment fee, because Shopify Payments and PayPal do not return fees on refunds.

## Messages to customers

Write the message for the user to send. One sentence on what happened, a real date or the options, and what the seller will do next and when. Never say an order has shipped or is being processed when it has not. Do not blame the customer, invent reasons, or promise what the seller cannot control.

> Hi [name], I'm sorry: the [item] you ordered on [date] is out of stock with our supplier. I can send [alternative] so it arrives by [date], you can wait until [date] when it is back in stock, or I can refund you in full today. Which would you prefer?
