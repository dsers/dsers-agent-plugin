# DSers pricing rules

A pricing rule belongs to one store and sets the selling price, and optionally the compare-at price, of products from their supplier cost. It applies when products are imported, and to existing products only when the seller applies it to the store (`mcp__dsers__update_store_product_price_in_bulk`) or when automatic price updates recalculate a product.

## Contents

- Rule types
- Basic rule
- Advanced rule
- Price endings and exchange rate
- Showing example prices
- Designing a rule

## Rule types

`mcp__dsers__get_pricing_rule` returns the active `mode` and every saved rule body. Only the body that matches the active mode is in use; describe the others as saved but inactive, if at all.

| Mode | In plain words | Can be changed here |
|---|---|---|
| none | No rule; prices are set by hand | Yes |
| basic | Multiply the cost by a number, or add an amount, per cost range | Yes |
| advanced, fixed formula | Profit percentage, fixed and minimum profit, a percentage for fees, shipping, and tax, per cost range | Yes, if the plan includes it |
| advanced, custom formula | A formula the seller wrote in DSers | No; describe it, and the seller edits it in DSers |
| ai_custom | An AI Custom rule saved in DSers | No; describe it, and the seller edits it in DSers |

## Basic rule

- **Cost ranges**: supplier cost bands in USD, such as 0–9.99 and 10–29.99. Costs outside every range use the "rest" formula.
- **Formula per range**: cost × a number (for example × 2.5), or cost + an amount (for example + 8).
- **Cost basis**: product cost only, or product cost plus shipping.
- **Compare-at price**: optional, with its own basis and its own multiply or add.

Example: with "cost × 2.5" on product cost, a product costing $6.00 sells for $15.00.

## Advanced rule

Each cost range has:

- **Profit %**: the profit percentage the seller wants.
- **Fixed profit**: an amount added per item, in the store currency.
- **Minimum profit**: DSers keeps the profit at or above this amount.
- **Breakeven %**: a percentage of the price that covers payment and transaction fees.
- **Shipping**: a fixed shipping cost, or supplier shipping rates to a chosen destination country.
- **Tax or import charges**: a fixed amount, or the platform's default calculation where one exists.
- **Compare-at price**: optional, as a multiple of or an amount added to the selling price.

Do not state DSers's exact Advanced formula from memory. To show real numbers, use the method below.

## Price endings and exchange rate

- **Price endings** ("cents"): for example, every selling price ends in .99, and every compare-at price in .00. Selling and compare-at endings are switched on and off separately. An assigned ending replaces the cents of each price: with .99, $6.00 becomes $6.99 and $29.97 becomes $29.99. Check the margin after the ending is applied.
- **Exchange rate**: when the store sells in another currency, DSers converts with its default rate, or with a custom rate the seller sets.

## Showing example prices

- **For the store's saved rule**: `mcp__dsers__simulate_pricing_rule` returns the prices DSers itself would compute for a store product. To show prices at other costs, run it on any mapped product in that store with cost overrides for its variants, and label the costs as examples.
- **For a proposed rule that is not saved yet**: calculate with the rule's inputs, show the arithmetic, and label the prices as approximate. After the user saves the rule, check a few products with `mcp__dsers__simulate_pricing_rule` before applying it to the whole store.

## Designing a rule

1. Collect the inputs: typical supplier costs, shipping per order, the payment fee rate, the ad cost per order, and the target margin.
2. Use cost ranges so cheap products get a higher multiplier and expensive products a lower one. Three or four ranges are usually enough.
3. For each range, check the price at the low and high end of the range: profit after fees and ads must be positive, and should meet the target.
4. Include shipping in the cost basis when the store offers free shipping.
5. Add a price ending, and check the margin again.
6. Set compare-at prices only if the seller will actually have sold at those prices; see [rules.md](rules.md).

Present the rule as a short table, one row per range, then a table of five or six example costs with price, profit, and margin.
