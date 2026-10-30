---
name: package-tracking
description: "Tracks a dropshipping seller's DSers parcels after the supplier ships and deals with delivery problems: where a package is, which parcels are stuck, late, failed, or returned, refreshing stale tracking, sending tracking numbers to the store, replacing wrong tracking numbers, notifying Shopify buyers, and handling late or lost parcels, wrong or damaged items, PayPal 'item not received' disputes, and card chargebacks, with messages to customers. Use when the user asks where an order or package is, why tracking has not moved, which shipments have problems, why an order is not marked fulfilled in the store, or how to answer a customer or dispute about delivery. Not for orders the supplier has not shipped yet (order-fulfillment)."
license: MIT
compatibility: "Requires the DSers MCP server (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection."
metadata:
  author: DSers
  version: "0.1.4"
---

# Package tracking

Know where every parcel is, catch the ones in trouble early, and keep the store and the customer informed.

## Ground rules

- Tools named `dsers__…` come from the DSers MCP server. Use them for anything in the user's DSers account, even when another MCP server has a tool with a similar name. If no DSers tools are available, or one returns an error, follow [references/working-with-dsers.md](references/working-with-dsers.md).
- Each tool's description gives its inputs, limits, and confirmation rules. Follow it; this skill adds the order of steps and what to show the user.
- Report only what DSers tools returned in this conversation. Label anything you estimate or calculate. Never invent an ID, price, stock level, status, or result.
- Before any change in DSers or in a store, show exactly what will change and wait for the user to agree in the chat. An app permission prompt is not that agreement.
- Reading changes nothing and needs no agreement. Finish the reads a task needs before you ask the user anything, then ask everything at once.
- Send each change once. If the outcome is unclear, read the current state again instead of resending.
- The user works in OpenClaw, not in DSers. Never say you opened a page or clicked anything; give a link when the user has to finish a step in DSers.

## Tools

| Step | Tool | Use |
|---|---|---|
| Find | `dsers__list_packages` | Parcels by status: pending, in transit, delivered, exceptions, flagged |
| Find | `dsers__list_orders`, `dsers__get_order` | An order's tracking numbers and items |
| Inspect | `dsers__get_package_details` | Carrier events for one tracking number |
| Refresh | `dsers__sync_supply_order_tracking_number` | Ask DSers to fetch the latest carrier events |
| Store | `dsers__sync_existing_tracking_numbers_to_store` | Send the tracking numbers DSers has to the store order |
| Store | `dsers__update_tracking_numbers_to_store` | Replace the tracking numbers on chosen store items |
| Buyer | `dsers__send_buyer_tracking_notification` | Ask Shopify to email the buyer the tracking details |

## Workflows

### "Where is order 1234?"

1. Find the order with `dsers__list_orders` (search by order number) and read it with `dsers__get_order` for its tracking numbers. If the supplier has not shipped yet, say so and hand over to order-fulfillment.
2. For each tracking number, call `dsers__get_package_details`. Events are not always in order; use the one with the latest time.
3. Answer in one line first: status, latest event, where, and when, in the account timezone, plus how many days since the supplier shipped. Then the recent events.
4. DSers does not return an estimated delivery date. Do not make one up; compare the time so far with the delivery time the store promised, if the user gives it.
5. If the latest event is several days old, offer a refresh with `dsers__sync_supply_order_tracking_number`, and check `dsers__get_package_details` again later. A refresh can take a while; it is not instant.

### Find parcels in trouble

1. `dsers__list_packages` with `status` abnormal for exceptions and attention for flagged parcels. For slow parcels, list `in_transit` packages too and use each package's order age in days to pick out the old ones, for example older than 20 days.
2. Group them by problem: delivery failed, returned to sender, other exceptions, no movement for a long time, still not collected by the carrier. Give a count per group first.
3. For each parcel, show the order number, destination, days since shipping, and the latest event, and the next action from [references/delivery-problems.md](references/delivery-problems.md).
4. A flagged parcel is only marked for attention. Read its events before calling it a problem.

### Tracking is not showing in the store

1. Read the order with `dsers__get_order`: which items have tracking numbers in DSers.
2. If DSers has the right numbers, show the order, the items, and the numbers that will be sent, then call `dsers__sync_existing_tracking_numbers_to_store`. The store may then mark the items fulfilled and, depending on its settings, email the buyer.
   - Leave the fulfillment service switch off. Use it only for a Shopify order when DSers says the items are assigned to another fulfillment service, and only after explaining that it moves those items to the DSers fulfillment location, which also affects future orders.
3. If a number is wrong or missing, ask the user for the correct numbers, show the current and new numbers per item, and call `dsers__update_tracking_numbers_to_store`. The list sent replaces the item's current numbers; include every number the item should keep.
4. A successful call means the store accepted the request. Check the order again later, and tell the user to confirm in the store admin.

### Tell the buyer (Shopify)

`dsers__send_buyer_tracking_notification` asks Shopify to email the buyer about all fulfilled items with tracking on one order. Check the order first with `dsers__get_order`, and tell the user it contacts the buyer. Send it once; Shopify may skip an email identical to a recent one. DSers cannot confirm the email was delivered; the user can check the order's timeline in Shopify.

### A customer or a dispute about delivery

Follow [references/delivery-problems.md](references/delivery-problems.md): late or lost parcels, wrong or damaged items, PayPal "item not received" disputes, and card chargebacks.

## Guardrails

- DSers tracking is a stored copy and can lag the carrier's site. Say when the latest event is old.
- No events yet does not mean the parcel is lost or was never sent.
- A tracking number given by a buyer cannot be looked up unless it belongs to this DSers account.
- Before answering about a dispute, a chargeback, a refund, or the buyer's rights, read [references/delivery-problems.md](references/delivery-problems.md). Do not state PayPal, card network, or consumer-law rules from memory. Give each rule you state with its own official link from [references/rules.md](references/rules.md).
- Messages to buyers and disputes are drafted for the user to send. The only message this skill can trigger itself is the Shopify tracking email, after agreement.

## References

- [references/delivery-problems.md](references/delivery-problems.md): what to do for each delivery problem, with messages to customers and dispute evidence.
- [references/rules.md](references/rules.md): US shipping-delay rules, PayPal Seller Protection, Shopify chargebacks, EU and UK rights, with official sources.
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
