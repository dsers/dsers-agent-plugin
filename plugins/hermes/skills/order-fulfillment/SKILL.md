---
name: order-fulfillment
description: "Moves a dropshipping seller's DSers orders from paid by the customer to placed and paid with the supplier: shows how many orders are waiting, explains why an order cannot be placed (shipping method, address, unmapped or out-of-stock items), fixes the address, shipping method, or supplier on an order, places orders with suppliers, creates payment links for orders awaiting payment, cancels eligible supplier orders, and refreshes supplier order details. Use when the user asks about new, stuck, failed, or unpaid orders, wants to place or pay for orders, asks why an order is not going through, or has an item that went out of stock after the customer paid. Not for tracking parcels after they ship (package-tracking) or changing a product's supplier for all future orders (supplier-mapping)."
license: MIT
compatibility: "Requires the DSers MCP server (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection."
metadata:
  author: DSers
  version: "0.1.1"
---

# Order fulfillment

Get orders from "the customer paid" to "the supplier has the order and it is paid for", and clear whatever blocks them on the way.

## Ground rules

- Tools named `mcp__dsers__…` come from the DSers MCP server. Use them for anything in the user's DSers account, even when another MCP server has a tool with a similar name. If no DSers tools are available, or one returns an error, follow [references/working-with-dsers.md](references/working-with-dsers.md).
- Each tool's description gives its inputs, limits, and confirmation rules. Follow it; this skill adds the order of steps and what to show the user.
- Report only what DSers tools returned in this conversation. Label anything you estimate or calculate. Never invent an ID, price, stock level, status, or result.
- Before any change in DSers or in a store, show exactly what will change and wait for the user to agree in the chat. An app permission prompt is not that agreement.
- Reading changes nothing and needs no agreement. Finish the reads a task needs before you ask the user anything, then ask everything at once.
- Send each change once. If the outcome is unclear, read the current state again instead of resending.
- The user works in Hermes, not in DSers. Never say you opened a page or clicked anything; give a link when the user has to finish a step in DSers.

## Tools

| Step | Tool | Use |
|---|---|---|
| Find | `mcp__dsers__list_orders` | Counts by tab, order lists, order IDs, revenue, cost, and profit per order |
| Inspect | `mcp__dsers__get_order` | One order in full: items, supplier rows, placement errors, address |
| Diagnose | `mcp__dsers__diagnose_order_issue` | What blocks placement, and which items can be placed |
| Fix | `mcp__dsers__validate_order_address`, `mcp__dsers__update_order_information` | Check and correct the address, notes, message to the supplier, or shipping method |
| Fix | `mcp__dsers__list_supplier_product_shipping_methods_and_cost` | Shipping options and costs to the order's country |
| Fix | `mcp__dsers__update_order_supplier` | Fulfill one order item with another supplier product or variant |
| Place | `mcp__dsers__place_orders_to_suppliers` | Send orders to the supplier |
| Pay | `mcp__dsers__create_order_payment_link` | Checkout link for orders awaiting payment |
| Follow up | `mcp__dsers__get_job_status`, `mcp__dsers__update_supplier_order` | Placement results; refresh supplier order details |
| Cancel | `mcp__dsers__cancel_supplier_order` | Cancel an eligible DSers Agent or 1688 Dropshipping supplier order |
| Records | `mcp__dsers__get_order_paymentlink_and_billing` | Payment and billing records for orders |
| Context | `mcp__dsers__list_stores` | Stores, supplier platforms, supplier accounts |

## How orders move

| Tab | Meaning | Next step |
|---|---|---|
| awaiting_order | The customer paid; not yet placed with the supplier | Diagnose, fix, place |
| awaiting_payment | Placed with the supplier; the seller has not paid the supplier | Payment link |
| awaiting_shipment | Paid; the supplier has not shipped | Wait, or refresh supplier details if stale |
| awaiting_fulfillment, fulfilled | Shipped; tracking is waiting for or has reached the store | package-tracking |
| failed, cancelled | Placement failed, or the order was cancelled | Inspect, fix, place again |

An order can sit in several tabs when its items are at different steps, so tab counts overlap: never add them up for a total. `pending` is a separate tab from `awaiting_order`; report both.

## Workflows

### Overview: "How many orders do I have to deal with?"

Call `mcp__dsers__list_orders` with `only_count` true, for all stores unless the user names one. Report the total and each tab's count, starting with the tabs that need the seller: awaiting_order, awaiting_payment, failed. Offer the next step for the largest group.

### Place new orders

Copy this checklist into your reply and keep it updated:

```
Placing orders
- [ ] 1. List awaiting orders
- [ ] 2. Diagnose each supplier group
- [ ] 3. Fix or set aside blocked orders
- [ ] 4. Preview and get agreement
- [ ] 5. Place, then check every order
- [ ] 6. Offer payment
```

1. **List.** `mcp__dsers__list_orders` with `status` awaiting_order. Group the orders by supplier platform and store, because placement and diagnosis work one supplier platform at a time.
2. **Diagnose.** For each group, call `mcp__dsers__diagnose_order_issue` with that `supplier_platform_id` and `status` awaiting_order. An order is ready only when the result says placement is allowed and the check is complete; anything unknown or incomplete is not ready.
3. **Fix.** For each blocked order, use the matching fix in [references/order-blockers.md](references/order-blockers.md), then diagnose again. If the user does not want to fix an order now, leave it out.
4. **Preview.** Show one table of the ready orders: order number, store, items and quantities, supplier, shipping method, supplier cost, order revenue, and profit, each with its currency. Flag orders whose cost is higher than the revenue. Say that placing creates real supplier orders, and that paying the supplier is the next step. If the supplier platform has several supplier accounts, ask which one to use.
5. **Place.** Call `mcp__dsers__place_orders_to_suppliers` with the exact order IDs in `selected` mode: up to 20 orders per call, all from one supplier platform and one store platform (such as Shopify). Never use filters to stand in for orders the user named.
   - **AliExpress**: the call returns a DSers page with the selected orders instead of placing them. Give the link and tell the user to finish placing and paying there.
   - **Other suppliers**: the call starts a task. Follow it with `mcp__dsers__get_job_status`, then check each order with `mcp__dsers__list_orders`. Report placed, failed (with the reason), and unknown orders separately. Never place an order again because its status is unclear.
6. **Pay.** Offer payment for the orders now awaiting payment (next workflow).

### Pay for orders

1. `mcp__dsers__list_orders` with `status` awaiting_payment, or the orders just placed.
2. Show the orders, their supplier costs, and the total per currency, and get agreement.
3. `mcp__dsers__create_order_payment_link` with the exact order IDs in `selected` mode. Give each returned link. Opening a link does not charge anyone; the user pays on the checkout page.
4. Payment status shows up later in `mcp__dsers__list_orders` and in `mcp__dsers__get_order_paymentlink_and_billing`. Do not say an order is paid until DSers shows it.

### Explain a failed, cancelled, or stuck order

1. Find the order with `mcp__dsers__list_orders` (search by the store order number), then read it with `mcp__dsers__get_order`, using the supplier and tab from the same list row.
2. Explain in plain words what happened, from the placement errors and supplier rows. If the order needs a shipping method, get the options as in [references/order-blockers.md](references/order-blockers.md) "Shipping method" and show them with a recommendation in this same reply; reading them needs no agreement.
3. If it can be placed again, diagnose it with the order's tab (failed or cancelled), fix the blocker, and place it as above.
4. If supplier details look out of date, offer `mcp__dsers__update_supplier_order` for that store, follow the task, then read the order again.

### An item is out of stock after the customer paid

Follow [references/order-problems.md](references/order-problems.md): find a replacement for this order, decide with the user, tell the customer, and stop the next order hitting the same problem.

### Cancel a supplier order

`mcp__dsers__cancel_supplier_order` cancels only DSers Agent and 1688 Dropshipping orders that are awaiting payment or awaiting shipment. Check the order's current tab with `mcp__dsers__get_order` first, confirm, cancel, and later confirm with `mcp__dsers__get_order` that it shows as cancelled. For other suppliers, tell the user to cancel with the supplier, for example on the AliExpress order page. Cancelling a supplier order does not refund the customer; say so.

## Guardrails

- Placing orders spends the seller's money. Never place an order the user has not seen in the preview.
- Never guess an address, phone number, or tax ID. Ask the user for the corrected value.
- When a fix needs the user to choose, such as a shipping method or a replacement supplier, read the choices first and show them with a recommendation in the same reply.
- When choosing a shipping method for an order, pass the quote's own cost and currency; leaving the cost out records it as zero.
- Show an order's address and contact details only when the task needs them.
- An order with items from several suppliers is placed per supplier. Say which items each step covers.
- Before answering about a customer cancellation, refund, or return, read [references/order-problems.md](references/order-problems.md). Do not state consumer-law or payment-platform rules from memory. Give each rule you state with its own official link from [references/rules.md](references/rules.md).

## References

- [references/order-blockers.md](references/order-blockers.md): what each placement blocker means and how to fix it.
- [references/order-problems.md](references/order-problems.md): out-of-stock and supplier cancellations after payment, customer cancellations and returns, and messages to customers.
- [references/rules.md](references/rules.md): consumer and payment rules with official sources (US shipping delays, EU and UK cancellations, Shopify and PayPal refunds).
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
