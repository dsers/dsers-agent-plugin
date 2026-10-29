---
name: product-publishing
description: "Publishes products from a dropshipping seller's DSers Import List to their connected stores: picks products and stores, checks each product is ready, sets up a Shopify shipping profile when the store has none, chooses shipping per product and store, sets whether products go live or stay as drafts, visibility in the online store, stock and price sync, and other publish settings, then publishes and reports the result for every product and store. Use when the user wants to publish, push, list, or upload products to Shopify or another store, or asks why a product did not publish. Not for editing products before publishing (listing-editing) or changing products already in the store (profit-and-pricing, supplier-mapping)."
license: MIT
compatibility: "Requires the DSers connector (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection."
metadata:
  author: DSers
  version: "0.1.0"
---

# Product publishing

Take finished products from the Import List to the store, with the settings the seller expects, and confirm every one arrived.

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
| Products | `DSers:list_pre_publish_products`, `DSers:get_pre_publish_product` | Import List products and their details |
| Stores | `DSers:list_stores` | Target stores, platform, status, languages, Shopify shipping profiles, publish defaults |
| Shopify shipping | `DSers:list_shopify_shipping_zones`, `DSers:create_shopify_shipping_profile` | Create a shipping profile when the store has none |
| Shipping | `DSers:get_push_shipping_recommendation` | Shipping method choices for each product and store |
| Organize | `DSers:get_pre_publish_product_organization`, `DSers:update_pre_publish_product_organization` | Optional: collections, tags, category before publishing |
| Publish | `DSers:publish_products_to_stores` | Publish up to 100 product-store pairs in one task |
| Result | `DSers:get_job_status` | Outcome for each product and store |

## Workflow

```
Publishing
- [ ] 1. Choose products and stores
- [ ] 2. Check products are ready
- [ ] 3. Shopify shipping profile
- [ ] 4. Shipping method per product and store
- [ ] 5. Publish settings
- [ ] 6. Preview and get agreement
- [ ] 7. Publish and report every pair
```

1. **Choose.** Products from `DSers:list_pre_publish_products`, by name or filter; stores from `DSers:list_stores`. Leave out stores whose status is not available and say why. All products in one publish go to the same stores; split the request when products go to different stores.
2. **Ready?** For each product, check with `DSers:get_pre_publish_product`: a cleaned-up title (no supplier keyword stuffing), a price on every variant, the variants the seller wants, and stock. List the problems and offer to fix them with the listing-editing skill first. Offer a review of collections, tags, and category; skip it if the user does not want it.
3. **Shopify shipping profile.** Each Shopify store needs one of its shipping profiles and a location from `DSers:list_stores`. If there are several, ask which one. If the store has none, offer to create one: get the store's countries and currency with `DSers:list_shopify_shipping_zones`, agree the countries, rate name, and flat rate with the user, create it with `DSers:create_shopify_shipping_profile`, then read `DSers:list_stores` again for the new IDs.
4. **Shipping method.** Call `DSers:get_push_shipping_recommendation` once for all the product-store pairs. For each pair with options, show them and let the user pick one or keep the default. Use a choice only for the pair it was made for; a pair with no options publishes with the default.
5. **Publish settings.** Read the store's defaults from `DSers:list_stores` and work out the effective value of each setting, from [references/publish-settings.md](references/publish-settings.md). Do not ask about each one; show them in the preview, and point out the ones that matter most if the user has not said: live or draft, visible in the online store, and automatic stock and price updates.
6. **Preview.** One table: product, store, platform, live or draft, visible or not, shipping profile and location (Shopify), shipping method options, and the main settings in plain words. Below it, list the product problems from step 2 and every choice the user can make. Get agreement, or the changes the user wants.
7. **Publish.** Call `DSers:publish_products_to_stores` once for the agreed pairs. If DSers blocks it before starting, nothing was published: explain each reason and fix it. Otherwise follow the task with `DSers:get_job_status` until it ends, and report each pair: published, failed with the reason, or unknown. Never publish the same pair again because its status is unclear; check the store's products first.

## After publishing

- Published products appear in the store with the chosen status. Products published as drafts need the seller to review and activate them in the store admin.
- Changes to published products are made with other skills: prices with profit-and-pricing, supplier and stock with supplier-mapping.
- To take a product down, the seller unpublishes it in DSers or the store; offer the DSers page from `DSers:find_feature_entry`.

## Guardrails

- Publishing can make products visible to buyers at once. Never publish a pair the user has not seen in the preview.
- Never treat a store as Shopify by its name or domain; use the platform `DSers:list_stores` returns.
- Use shipping profile and location IDs from the same store and the same profile.

## References

- [references/publish-settings.md](references/publish-settings.md): each publish setting in plain words, and sensible defaults.
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
