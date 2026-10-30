---
name: profit-and-pricing
description: "Checks whether a dropshipping seller is making money and sets prices in DSers: profit per order and per product from the seller's DSers orders after payment fees, ads, refunds, and other costs; where the money leaks; explaining, testing, and changing a store's pricing rule (cost ranges, multipliers, profit margins, shipping, price endings, exchange rate); applying the rule to every product in a store; changing one live product's price; and reacting when supplier costs rise. Use when the user asks if they are profitable, why sales do not reach the bank, which products lose money, what to charge, how their pricing rule works, or wants prices changed. Not for prices of products still in the Import List before publishing (listing-editing)."
license: MIT
compatibility: "Requires the DSers MCP server (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection. Some rule types need a plan that includes them."
metadata:
  author: DSers
  version: "0.1.1"
---

# Profit and pricing

Find out what each order and product really earns, then set prices that keep it that way.

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
| Orders | `mcp__dsers__list_orders` | Revenue, supplier cost, profit, and margin per order |
| Rule | `mcp__dsers__get_pricing_rule` | The store's current pricing rule, price endings, and exchange rate |
| Test | `mcp__dsers__simulate_pricing_rule` | Prices the rule gives a store product, for a destination and cost |
| Change rule | `mcp__dsers__update_pricing_rule` | Save a new rule, price endings, or exchange rate for one store |
| Apply rule | `mcp__dsers__update_store_product_price_in_bulk`, `mcp__dsers__get_job_status` | Reprice every eligible product in a store with its rule |
| One product | `mcp__dsers__list_managed_store_products`, `mcp__dsers__get_managed_store_product`, `mcp__dsers__update_managed_store_product` | Find a live product and change its variants' prices |
| Cost changes | `mcp__dsers__list_supplier_product_change_notifications` | Supplier price rises in the last 30 days |
| Context | `mcp__dsers__list_stores`, `mcp__dsers__get_account_profile` | Store IDs and currencies; account timezone for date ranges |

## Workflows

### Profit check: "Am I making money?"

```
Profit check
- [ ] 1. Agree the period and stores
- [ ] 2. Pull orders and DSers profit
- [ ] 3. Collect the costs DSers does not have
- [ ] 4. Calculate real profit per order and per product
- [ ] 5. Report, with fixes
```

1. Agree the date range, for example the last 30 days, in the account timezone, and which stores.
2. Page through `mcp__dsers__list_orders` for that range until there are no more pages, and say if you stopped early. Each order has revenue (what the customer paid, in the store's currency), supplier cost, and profit. DSers profit is revenue minus supplier cost only. Keep amounts in different currencies apart unless DSers returned a converted amount. Orders with no cost yet, such as unmapped or unplaced ones, have no profit; count them separately.
3. Ask for what DSers does not know, most useful first: ad spend for the period as billed, the payment provider and the rate the seller actually pays, refunds and chargebacks, and monthly fixed costs. Work with what the user has and label every assumption.
4. Calculate with [references/profit-math.md](references/profit-math.md). Group by product using each order's line items; read an order with `mcp__dsers__get_order` when the list does not name its products. Every order that loses money needs its product named.
5. Report as that file describes: the answer in one line, a table per product, the leaks that apply, and fixes. For a product that loses money, give the price that reaches the user's target margin and the highest ad cost per order today's price can carry.

### Explain the current rule

`mcp__dsers__get_pricing_rule` for the store. Say which rule is active and describe it in plain words, then work two or three example costs through it, showing the selling price, compare-at price, and margin. [references/pricing-rules.md](references/pricing-rules.md) explains each rule type and field. Report only the active rule as current.

### Change the rule

1. Read the current rule with `mcp__dsers__get_pricing_rule` immediately before.
2. Design the change with the user. Default to the rule type the store already uses. For each cost range, check that the price covers landed cost, the payment fee, and the ad cost per order, with [references/profit-math.md](references/profit-math.md).
3. Preview: the current and new rule side by side, and a table of example costs with old price, new price, profit, and margin, calculated from the two rules and labeled as calculated. Apply the store's price endings (for example .99) to every example price before you show it, so the prices match what buyers would see; do not show prices without the ending. Include exchange-rate changes if any.
4. After agreement, save with `mcp__dsers__update_pricing_rule`.
5. Say clearly: saving a rule does not change prices of products already in the store. Offer to apply it (next workflow) or to test it on chosen products first with `mcp__dsers__simulate_pricing_rule`.

If DSers says the plan does not include a rule type, tell the user what DSers returned. Do not rebuild that rule some other way.

### Apply the rule to every product in a store

`mcp__dsers__update_store_product_price_in_bulk` reprices every eligible product in the selected stores with each store's current rule. Buyers see the new prices. Before agreeing, test a few typical products with `mcp__dsers__simulate_pricing_rule` and show old and new prices. Then start the task, follow it with `mcp__dsers__get_job_status`, and report the result per store.

### Change one live product's price

1. Find the product with `mcp__dsers__list_managed_store_products` and read its variants with `mcp__dsers__get_managed_store_product`.
2. Show each variant's current and new price, and compare-at price if it changes.
3. Ask what should happen to automatic price updates for this product: keep them, so a later rule or cost update may overwrite this price, or turn them off for it. Explain the choice; do not pick it for the user.
4. After agreement, save with `mcp__dsers__update_managed_store_product`, then compare the updated variants with what was asked.

For products still in the Import List, use the listing-editing skill.

### Supplier costs went up

1. `mcp__dsers__list_supplier_product_change_notifications` with change type cost changed, for the stores the user cares about.
2. For each affected store product, show the old and new cost and the margin at today's price.
3. Offer the fix that fits: let the pricing rule recalculate (apply the rule, above), change the product's price, or find a cheaper supplier with the supplier-mapping skill.

## Guardrails

- Never state fee rates, plan prices, tax or duty rates from memory. Use the user's numbers, or an example rate labeled as an example and where to find their own.
- Margin and markup are different. Say which one a number is.
- Compare-at ("was") prices must be real former prices; see [references/rules.md](references/rules.md). Point it out when a rule sets compare-at prices as a multiple of the price.
- Pricing is the user's decision. Recommend with numbers; do not change a price the user has not agreed to.

## References

- [references/profit-math.md](references/profit-math.md): formulas, where money leaks, cash timing, and the report format.
- [references/pricing-rules.md](references/pricing-rules.md): DSers rule types and fields in plain words, and how to design a rule.
- [references/rules.md](references/rules.md): fees kept on refunds, chargebacks, payout holds, and compare-at price rules, with official sources.
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
