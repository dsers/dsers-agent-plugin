# Delivery problems

Use this for late, stalled, failed, returned, or lost parcels, wrong or damaged items, and payment disputes about delivery. Laws and platform rules mentioned here are in [rules.md](rules.md) with their official sources; name the country or platform and give the link when you rely on one.

## First, the facts

From DSers: the order date, when the supplier shipped, the tracking events (`DSers:get_package_details`), the destination, and the shipping method. From the user: the delivery time the store promised, how the customer paid, and whether a dispute or chargeback is already open. Do not state an AliExpress buyer protection period or dispute rule from memory; tell the user to read it on the supplier order page.

## By tracking status

| Status | What it usually means | What to do |
|---|---|---|
| Awaiting collection, or no events for several days after shipping | The supplier made a label, but the carrier has not scanned the parcel | Refresh the tracking. If it still has no scans after about a week, ask the supplier whether it really left; consider a dispute with the supplier before its protection period ends |
| In transit, no new event for a long time | Often waiting between countries or at customs | Refresh. Compare the total time with the promise. If the parcel reached the destination country, ask the supplier for the local carrier's tracking number |
| At delivery hub, awaiting delivery | Nearly there | Tell the customer it is close; nothing else is needed |
| Delivery failed | The carrier could not deliver: address problem, nobody home, or a customs or fee issue | Ask the customer to check the address and contact the local carrier. Fix the address with the carrier, not in DSers; the supplier order has already shipped |
| Returned | The parcel is going back to the supplier | Ask the supplier what happens when it arrives; reship or refund the customer, then claim from the supplier |
| Delivered, but the customer says it did not arrive | Wrong door, neighbour, mailroom, or a carrier error | See "Item not received" below |
| Other exception | Damage, loss, or a carrier issue | Read the events; contact the supplier and the carrier |

## Late, stalled, or lost

1. Compare the time so far with the delivery time the store promised and the shipping method's normal time.
2. Ask the supplier for an update. If the parcel was handed to a local carrier in the destination country, ask for that carrier's tracking number.
3. Watch the supplier order's buyer protection period. Open a dispute with the supplier before it ends if the parcel still has not arrived; do not let it run out while waiting.
4. Tell the customer what is known and when the next update will come. For a US customer past the promised date, the FTC delay rule applies: ask for their consent to the delay, and refund if they do not agree.
5. If the parcel is lost, reship or refund, then claim from the supplier.

## Wrong, damaged, or not as described

1. Ask the customer for photos of the item, the packaging, and the shipping label.
2. Agree on a fix with the customer: a replacement, a partial refund if they keep the item, or a full refund. Compare the cost of return shipping with the item's cost before asking for the item back.
3. Use the photos to open a dispute with the supplier within its protection period.
4. If the same product keeps arriving wrong or damaged, suggest changing supplier with the supplier-mapping skill.

## PayPal "item not received", but tracking shows delivered

1. Check that the delivery address in the tracking matches the address on the PayPal Transaction Details page. [PayPal Seller Protection](https://www.paypal.com/us/cshelp/article/what-is-paypal-seller-protection-help156) requires shipping to that address and valid proof of shipment or delivery; give the user that link and tell them to read the full requirements before assuming they are covered.
2. If the tracking only covers the journey to the destination country, get the local carrier's number that shows delivery.
3. Reply in the dispute with the carrier, tracking number, delivery date, and delivery address, and attach the tracking page. Ask the customer politely to check with neighbours, household members, and the building's mailroom, and offer to open a trace with the carrier.
4. If the addresses do not match or there is no delivery scan, the evidence is weak; refunding may cost less than losing the case.

## Card chargeback through Shopify Payments

- The seller responds on the order page in the Shopify admin before the deadline shown there. Shopify's guide: [chargebacks](https://help.shopify.com/en/manual/payments/chargebacks).
- Evidence: the order details, the product page as the customer saw it, the policy the customer agreed to, tracking showing delivery to the order address, and every message with the customer.
- A refund cannot be issued while the chargeback is open. If the customer should get the money back, accept the chargeback, or ask the customer to withdraw it and then refund.
- If the bank has only sent an inquiry, a full refund prevents a chargeback; a partial refund does not.

## Messages to customers

Write the message for the user to send: one sentence on where the parcel is, a real date or the options, and what the seller will do next and when. Do not blame the customer, invent reasons, or promise a delivery date the carrier has not given.

> Hi [name], thanks for your patience. Your order [number] is with [carrier]; the latest update, on [date], shows it [status in plain words]. I've asked for an update and will write to you again by [date]. If it hasn't arrived by [date], I'll [reship it / refund you in full].
