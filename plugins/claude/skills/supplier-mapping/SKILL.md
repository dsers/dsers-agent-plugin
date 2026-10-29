---
name: supplier-mapping
description: "Links a dropshipping seller's store products to suppliers in DSers (Supplier Mapping) and keeps those links working: finds unmapped or broken products, brings existing store products into DSers, maps variants to a supplier, adds a backup supplier for stockouts, sets different suppliers by country or bundle quantities, runs AI Mapping, replaces a supplier whose price, stock, or SKUs changed, and updates live variants' stock. Use when the user asks which products have no supplier, wants to change or add a supplier for a product, sees Mapping Lost or unmapped items, gets supplier change notices, asks whether suppliers changed prices, stock, or products, or wants products already in their store managed by DSers. Not for finding new products to sell (product-sourcing), changing the supplier on one order only (order-fulfillment), or prices (profit-and-pricing)."
license: MIT
compatibility: "Requires the DSers connector (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection. AI Mapping uses AI Credits."
metadata:
  author: DSers
  version: "0.1.0"
---

# Supplier mapping

Every product the store sells needs a working link to a supplier variant, or its orders cannot be placed. This skill builds those links, repairs them, and swaps suppliers when one stops working.

## Ground rules

- Tools named `DSers:…` come from the DSers connector. Use them for anything in the user's DSers account, even when another connector has a tool with a similar name. If no DSers tools are available, or one returns an error, follow [references/working-with-dsers.md](references/working-with-dsers.md).
- Each tool's description gives its inputs, limits, and confirmation rules. Follow it; this skill adds the order of steps and what to show the user.
- Report only what DSers tools returned in this conversation. Label anything you estimate or calculate. Never invent an ID, price, stock level, status, or result.
- Before any change in DSers or in a store, show exactly what will change and wait for the user to agree in the chat. An app permission prompt is not that agreement.
- Reading changes nothing and needs no agreement. Finish the reads a task needs before you ask the user anything, then ask everything at once.
- Send each change once. If the outcome is unclear, read the current state again instead of resending.
- The user works in Claude, not in DSers. Never say you opened a page or clicked anything; give a link when the user has to finish a step in DSers.

## Tools

| Step | Tool | Use |
|---|---|---|
| Find | `DSers:list_managed_store_products` | Store products in DSers, with mapping status and unmapped counts |
| Find | `DSers:search_store_products`, `DSers:import_store_products_to_dsers` | Store products not yet in DSers, and bringing them in |
| Inspect | `DSers:get_managed_store_product`, `DSers:get_store_product_mapping` | Variants, current suppliers, stock, saved mapping |
| Candidates | `DSers:search_supplier_products`, `DSers:get_supplier_product`, `DSers:list_supplier_product_shipping_methods_and_cost` | Replacement suppliers, their variants, and shipping |
| Map | `DSers:apply_product_mapping` | Save Basic or Advanced mapping for chosen variants |
| AI Mapping | `DSers:search_ai_mapping_candidates`, `DSers:start_ai_product_mapping`, `DSers:get_job_status`, `DSers:apply_ai_mapping` | Find look-alike suppliers by image, compute a mapping, apply it |
| Changes | `DSers:list_supplier_product_change_notifications` | Supplier cost, stock, SKU, and availability changes in the last 30 days |
| Stock | `DSers:update_managed_store_product` | Set live variants' stock |

## Workflows

### Mapping health: "Which products have no supplier?"

1. `DSers:list_managed_store_products` for each store the user cares about, with the mapping-status filters: no supplier, and mapping lost or incomplete.
2. `DSers:list_orders` with `supplier_platform_id` 0 lists orders waiting on unmapped items. Match them to the products.
3. Report the total, each group with its products, and the orders each product is holding up. Offer to map the products with waiting orders first.

### Bring store products into DSers

1. `DSers:search_store_products` for the store. Each result says whether it is already in DSers.
2. Show the products not yet in DSers and let the user choose. Import up to five per call with `DSers:import_store_products_to_dsers`.
3. Imported products have no supplier yet. Continue with "Map a product".

### Map a product, or change its supplier

```
Mapping a product
- [ ] 1. Read the product and its current mapping
- [ ] 2. Choose the supplier product
- [ ] 3. Match every variant
- [ ] 4. Preview old and new, get agreement
- [ ] 5. Save, then read the mapping back
```

1. Read the product with `DSers:get_managed_store_product` and its mapping with `DSers:get_store_product_mapping`.
2. Get the supplier product: one the user names (a link or ID), or candidates from `DSers:search_supplier_products` by keyword or image link. For a replacement, compare cost, shipping cost and time to the store's main countries, stock, and rating against the current supplier.
3. Read the supplier's variants with `DSers:get_supplier_product` and match each store variant to exactly one supplier variant by its options (color, size, and so on), checked against images. Names alone are not enough. List every store variant with no match and ask the user.
4. Preview a table: store variant → current supplier variant → new supplier variant, with each new cost. Say what the change affects: future orders, and existing unplaced orders that allow mapping updates. If prices depend on supplier cost, say they may change with the pricing rule.
5. Save with `DSers:apply_product_mapping`, then read the mapping again and report what was saved, what was not, and why.

Choose the mapping type with [references/mapping-types.md](references/mapping-types.md): Basic for one supplier with an optional backup, Advanced for suppliers by country or bundle quantities.

### AI Mapping

1. `DSers:search_ai_mapping_candidates` for the product and the store's main destination country. Show the candidates with price, stock, shipping, rating, and how similar they look.
2. When the user picks one, say that starting AI Mapping uses AI Credits (the tool states how many), and check the balance with `DSers:get_plan_and_credits` if the user asks. After agreement, call `DSers:start_ai_product_mapping`.
3. Follow `next_step` in the result: poll the task with `DSers:get_job_status` until it ends, or use an existing result if one is returned.
4. Show the computed variant matches as a table and check them as in step 3 above. Applying is a separate decision: get a second, explicit agreement, then call `DSers:apply_ai_mapping`. Apply the whole result only if every match is right; otherwise save the correct matches with `DSers:apply_product_mapping`.
5. Read the mapping back and report.

### A supplier changed

1. `DSers:list_supplier_product_change_notifications`, filtered by store or change type if the user asks. Group by type: cost changed, variant out of stock, SKU changed, product unavailable.
2. For each affected store product:
   - **Cost changed**: show old and new cost; hand over to profit-and-pricing for prices, or find a cheaper supplier.
   - **Out of stock or unavailable**: find a replacement (candidates above), switch to the backup supplier, or set the variant's stock to zero so the store stops selling it.
   - **SKU changed**: the supplier's variants changed, so the mapping may point to variants that no longer exist. Read the mapping and re-map the affected variants.
3. Orders already waiting for the affected variant are handled in order-fulfillment.

### Set stock on live variants

Read the product with `DSers:get_managed_store_product`, show each variant's current and new stock, and ask whether automatic stock updates should stay on (so supplier stock can overwrite the number later) or be turned off for this product. After agreement, save with `DSers:update_managed_store_product`. The number replaces the current stock; it is not added to it.

## Guardrails

- A mapping decides what the supplier ships. Never map variants on a guess; unmatched variants stay unmatched until the user decides.
- Saving mapping for some variants leaves the others as they are. Say which variants the change covers.
- A started AI Mapping task, a saved mapping, and a mapping confirmed by reading it back are three different states. Report which one you reached.
- For one order only, use order-fulfillment; mapping changes the product for future orders.

## References

- [references/mapping-types.md](references/mapping-types.md): Basic and Advanced mapping, backup suppliers, country rules, bundles, and how changes reach orders.
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
