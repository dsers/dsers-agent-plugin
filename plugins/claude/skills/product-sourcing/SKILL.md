---
name: product-sourcing
description: "Finds products and suppliers for a dropshipping seller with live DSers supplier data across the supplier platforms connected to DSers, such as AliExpress, 1688, Alibaba.com, and the DSers Verified Agent Network: product ideas from curated collections, search by keyword or image link with filters like ship-from country, delivery time, and price, supplier comparison on cost, stock, shipping cost and time to the seller's market, ratings, and landed cost and margin, then adding the chosen products to the DSers Import List. Use when the user asks what to sell, wants a supplier for a product or an image, wants to compare suppliers or shipping options, or wants products imported into DSers. Not for replacing the supplier of a product already in the store (supplier-mapping) or editing imported products (listing-editing)."
license: MIT
compatibility: "Requires the DSers connector (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection."
metadata:
  author: DSers
  version: "0.1.0"
---

# Product sourcing

From "what should I sell?" to a shortlist of suppliers with real costs, and the winners saved to the Import List.

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
| Ideas | `DSers:list_featured_product_collections` | Curated themes when the user has no product in mind |
| Filters | `DSers:list_supplier_search_filters` | Turn "ships from the US", "under $10", a category, or a carrier into search values |
| Search | `DSers:search_supplier_products` | Supplier products by keyword or public image link |
| Detail | `DSers:get_supplier_product` | Variants, stock, prices, availability for a destination |
| Shipping | `DSers:list_supplier_product_shipping_methods_and_cost` | Shipping methods, cost, and time to a country |
| Countries | `DSers:list_supported_countries`, `DSers:list_platform_shipping_methods` | Country codes; carriers a supplier platform offers to a country |
| Import | `DSers:create_pre_publish_products` | Add 1–10 chosen products to the Import List |
| Context | `DSers:list_stores` | Connected supplier platforms and accounts |

## Before searching

Ask only for what changes the results, and use defaults for the rest:

- **Market**: the country most customers are in. Default to the store's country from `DSers:list_stores`, and say so.
- **Product**: keywords, or a public image link. An image pasted into the chat cannot be sent to DSers; ask for a link to it, or describe it and search by keywords.
- **Limits**: target cost or selling price, delivery time, ship-from country, if the user has them.

## Workflows

### "What should I sell?"

1. `DSers:list_featured_product_collections`. These are curated themes, not a sales ranking; present them that way.
2. When the user picks a theme or a direction, search it (next workflow). A list of themes alone does not answer the question; end with real products.

### Find and compare suppliers

```
Sourcing
- [ ] 1. Translate conditions into filters
- [ ] 2. Search
- [ ] 3. Shortlist 3–5 and read details
- [ ] 4. Get shipping to the market
- [ ] 5. Compare and recommend
- [ ] 6. Offer to import
```

1. If the user gave conditions such as ship-from, delivery time, or a category, get their values from `DSers:list_supplier_search_filters`.
2. `DSers:search_supplier_products`. Results echo which filters took effect; say which ones did not apply. If one supplier platform fails and others answer, show what came back and name the gap.
3. Pick 3 to 5 candidates that match the product and limits. Read each with `DSers:get_supplier_product` for the market country: variants, stock per variant, price, and whether it can ship there.
4. `DSers:list_supplier_product_shipping_methods_and_cost` for each to the market country, for the variant the seller would sell most.
5. Compare with [references/comparing-suppliers.md](references/comparing-suppliers.md) and recommend one, with a backup.
6. Offer to add the chosen products to the Import List (next workflow).

### Add products to the Import List

The Import List is where products wait in DSers to be edited and priced before they are published to a store.

1. Show the products to add, with supplier and price. Mention the language and destination country DSers will use for them: adding a language other than English changes the account's import languages, and each language counts toward the plan's language limit.
2. After agreement, call `DSers:create_pre_publish_products` with up to 10 products.
3. Report each product as added or failed, with the reason. A product that already has an Import List record fails; that is not an error to retry.
4. Next steps: clean up titles and variants with the listing-editing skill, then publish with the product-publishing skill. If a store has a pricing rule, DSers uses it to price products for that store.

## Guardrails

- Compare only values DSers returned, in one currency. Mark missing values as unknown; do not fill them from memory or the web.
- A supplier quote does not include shipping. Never present it as landed cost.
- Stock of zero on a variant means that variant is out of stock, not that the product is gone. "Not available" for a country means it cannot ship there; try another destination before ruling it out.
- Point out products that need extra care before selling: children's products, batteries, liquids, powders, magnets, and branded goods; see [references/rules.md](references/rules.md).

## References

- [references/comparing-suppliers.md](references/comparing-suppliers.md): what to compare, warning signs, and the comparison table.
- [references/rules.md](references/rules.md): product safety and shipping restrictions, duties on US parcels, with official sources.
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
