---
name: listing-editing
description: "Turns supplier products in a dropshipping seller's DSers Import List into store-ready listings before they are published: rewrites keyword-stuffed titles and supplier descriptions, renames and removes variants and options, fixes images, SKUs, stock, and package weight, sets prices or one price for all variants, sets collections, product type, tags, vendor, category, and URL handle for a store, and deletes products from the Import List; one product or a batch. Use when the user wants to clean up, rewrite, translate, or edit imported products, set prices before publishing, or tidy the Import List. Not for products already published to a store, whose text is edited in the store admin (prices: profit-and-pricing), or for publishing (product-publishing)."
license: MIT
compatibility: "Requires the DSers connector (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection."
metadata:
  author: DSers
  version: "0.1.0"
---

# Listing editing

Supplier listings are written for wholesale buyers: stuffed titles, notes to resellers, coded variant names. Fix them in the Import List, before customers see them.

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
| Find | `DSers:list_pre_publish_products` | Import List products, filtered by keyword, supplier, cost, publish status |
| Read | `DSers:get_pre_publish_product` | One product: title, description, images, options, variants, prices, and its current version |
| Content | `DSers:update_pre_publish_product_content` | Title, description, images, options, variants, SKUs, stock, package |
| Price | `DSers:update_pre_publish_product_price` | Variant prices and compare-at prices, or one price for all variants |
| Organize | `DSers:get_pre_publish_product_organization`, `DSers:update_pre_publish_product_organization` | Collections, type, tags, vendor, category, URL handle, per store |
| Delete | `DSers:delete_pre_publish_products` | Remove products from the Import List |
| Context | `DSers:list_stores` | Store IDs for prices and organization |

## Workflows

### Clean up one product

```
Cleaning up a product
- [ ] 1. Read the product, with its description
- [ ] 2. Collect facts and flag gaps
- [ ] 3. Draft the new title, description, variants
- [ ] 4. Preview before and after, get agreement
- [ ] 5. Save, then read it back
```

1. `DSers:get_pre_publish_product` with the description included.
2. List the product facts from the supplier text and images. Ask the user about anything a claim would need that the facts do not show.
3. Write the new version with [references/listing-standards.md](references/listing-standards.md). Offer two or three title options with a one-line reason each.
4. Show each changed field before and after. For variants, a table of old name → new name, and any variant or option to delete with what that removes.
5. Read the product again for a fresh version, save with `DSers:update_pre_publish_product_content`, and report the changed fields. Read it again before any further change; the version changes after every save.

### Variants, options, and images

- **Rename**: rename option values (for example "Color: A" → "Navy"); variant titles follow their options.
- **Delete**: deleting an option value deletes every variant that uses it, and deletions cannot be undone. Show the variants that will go, and keep at least one.
- **"Ships from" options**: if the store will sell from one location, remove the other values; if both stay, they need different prices and delivery times.
- **Images**: the image list is replaced as a whole and the first image becomes the main one. Send the full list in the new order, even to add or remove one. Images must be public HTTPS links.
- **Stock, SKU, weight and size**: set per variant, or for the package.

### Prices before publishing

1. Read the product for its current prices and version.
2. Show each variant's current and new price, and compare-at price only if the user asked to change it. Say that changing prices switches off the pricing rule for this product in the Import List.
3. After agreement, save with `DSers:update_pre_publish_product_price`: by variant, or one fixed price for all variants.
4. A compare-at price must stay at or above the price, and must be a real former price; see [references/rules.md](references/rules.md).

For margin and pricing-rule questions, use the profit-and-pricing skill.

### Collections, tags, category, and URL handle

These are set per store. Read the current values and the store's choices with `DSers:get_pre_publish_product_organization` for all selected products at once, let the user choose, then save only the chosen changes with `DSers:update_pre_publish_product_organization`. Creating a new collection or tag value creates it in the store.

### Many products at once

1. List the products with `DSers:list_pre_publish_products` and agree which ones.
2. Draft the changes for all of them and show one table: product, field, before, after. Flag the products that need facts from the user.
3. After agreement, save product by product, re-reading each just before saving it, and report progress as you go: saved, failed with the reason, skipped.
4. Give a final count, and list the failures with what to do.

### Clear out the Import List

List products with `DSers:list_pre_publish_products` using the filters the user describes, show the exact products to delete, and after agreement delete them with `DSers:delete_pre_publish_products`, up to 20 per call. Deleting cannot be undone, and does not touch products already in a store. Check the list again afterwards.

### Products already published

These tools edit only the Import List. For a published product, give the new text ready to paste into the store admin, or use profit-and-pricing for prices and supplier-mapping for stock.

## Guardrails

- Never add a material, measurement, certification, or claim the facts do not support.
- Do not translate on save; DSers saves the text you give it. When the user wants another language, write the translation and save that.
- Show each title and description exactly as it will be saved.

## References

- [references/listing-standards.md](references/listing-standards.md): what to remove from supplier listings, how to write titles, descriptions, variants, and sizes, and what EU and US listings must show.
- [references/rules.md](references/rules.md): Google Merchant Center title and image rules, EU product-safety information, compare-at price rules, with official sources.
- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers, errors, tasks, stores and currencies.
