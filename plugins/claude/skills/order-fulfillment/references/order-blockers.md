# Placement blockers and fixes

`DSers:diagnose_order_issue` reports, for each order, whether it can be placed, each blocker or warning, and often a suggested fix. Use its suggested fix when there is one. This table says what each common result means for the seller and which step fixes it. After any fix, diagnose the order again before placing it.

| What the diagnosis reports | What it means | Fix |
|---|---|---|
| No shipping country (`SHIPPING_COUNTRY_REQUIRED`) | The saved address has no country | Ask the user for the country, then save it with `DSers:update_order_information` |
| Address field errors | A field is missing, too long, or in the wrong format for the supplier | See "Address" below |
| Shipping method required (`SHIPPING_METHOD_REQUIRED`) | The supplier ships to this country, but no method is chosen | See "Shipping method" below |
| No shipping method available (`SHIPPING_METHOD_UNAVAILABLE`) | The supplier product cannot ship to this address | Use another supplier product or variant for this order with `DSers:update_order_supplier`, or, if nothing ships there, cancel and refund the customer (see [order-problems.md](order-problems.md)) |
| Item has no supplier (Unmapped, supplier ID 0) | The store product is not linked to a supplier | Link it with the supplier-mapping skill, which also fixes future orders, then list the order again |
| Variant out of stock or unavailable | The mapped supplier variant cannot be bought now | See [order-problems.md](order-problems.md) |
| Item has no quote (`UNQUOTED_ORDER`) | The item has no supplier price yet | Read the order; the item usually needs a supplier product or variant chosen with `DSers:update_order_supplier` |
| Zero quantity to fulfil (`ZERO_FULFILLABLE_QUANTITY`) | Nothing is left to order for that item, for example after a refund or a partial fulfilment in the store | Leave the item out; check the store order if the user did not expect this |
| Message to supplier too long (`SUPPLIER_MESSAGE_TOO_LONG`) | The note to the supplier is over 1,000 characters | Shorten it with the user and save it with `DSers:update_order_information` |
| Cost higher than revenue (`ORDER_COST_EXCEEDS_REVENUE`) | A warning, not a blocker: supplier cost plus shipping is more than the customer paid | Show it in the preview; the user decides whether to place the order anyway. If it repeats, suggest the profit-and-pricing skill |
| Checks listed as missing, or a result marked unknown or incomplete | DSers could not finish every check | The order is not confirmed ready. Say which check is missing; the user can place it in DSers after reviewing it there |

## Address

1. Run `DSers:validate_order_address` for the order and its supplier platform.
2. List each field with an error and why, in plain words, for example "phone number: must contain only digits".
3. Ask the user for the corrected values. Never guess an address, phone number, tax ID, or name, and never "fix" one by reformatting it beyond what the rule asks.
4. Show the old and new values, get agreement, and save them with `DSers:update_order_information`. Send only the fields that change.
5. Diagnose the order again.

Some countries need extra fields for customs, such as a tax ID or a birth date for Brazil. The validation result names them; the customer usually has to supply them.

A passing check means the fields meet the supplier's format rules. It does not confirm the address is deliverable.

## Shipping method

1. From `DSers:get_order`, take the item's supplier product and variant, and the destination.
2. Get the options with `DSers:list_supplier_product_shipping_methods_and_cost` for that product, variant, quantity, and destination.
3. Show the options: carrier and method, cost and currency, delivery time, and whether tracking is included. Recommend one: usually the cheapest method with tracking that arrives within the delivery time the store promised.
4. After the user chooses, save it with `DSers:update_order_information`, passing the quote's own service, carrier, cost, and currency for that supplier row.
5. Diagnose the order again.

To change the method for many orders, save it order by order after one agreement on the table of changes. To make a method the default for future orders, the user sets it in DSers shipping settings; offer the link from `DSers:find_feature_entry`.
