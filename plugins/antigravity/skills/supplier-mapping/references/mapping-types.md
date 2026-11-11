# Mapping types

A mapping links each store variant (what the customer buys) to the supplier variant DSers orders for it. A product uses one mapping type at a time; switching type keeps the other type's saved links for later.

## Choosing a type

| The seller wants | Use |
|---|---|
| One supplier for the product | Basic |
| A backup supplier that takes over when the main one is out of stock | Basic, with a backup |
| Different suppliers for different countries, such as a US warehouse for US orders | Advanced, by country |
| Several items, or several units, shipped for one store variant (bundles, buy one get one) | Advanced, with quantities |
| Orders fulfilled by a sourcing agent in the DSers Verified Agent Network | Advanced, with an Agent |

## Basic

- Each store variant links to one supplier variant from the main supplier product, and optionally one from a backup supplier product.
- The whole product can use one main supplier product and one backup supplier product.
- Each link needs the supplier variant's complete set of options. Store options are paired with supplier options by ID (for example store "Color: Navy" with supplier "Color: Dark Blue"). Supplier options the store does not sell, such as "Ships from: China", are fixed to one value.
- Option and value names may differ between the store and the supplier; match by what the variant actually is, using images and attributes, and confirm unclear pairs with the user.

## Advanced

- Links are set per country. "Global" is the fallback for every country without its own link, and stays in place next to country-specific links.
- For one store variant and one country, every link must come from the same supplier platform; it can include several supplier variants or products from that platform.
- Each link has a quantity. Buy one get one: the store variant links to the supplier variant with quantity 2. A bundle: the store variant links to each item in the bundle.
- An Agent link sends that variant's orders to a sourcing agent instead of a specific supplier product, with quantity 1.
- Saving Advanced links replaces only the countries you send for the variants you send.

## How a mapping change reaches orders

- New orders use the new mapping.
- Orders not yet placed with the supplier may also switch, unless someone set their supplier by hand and chose to keep that choice when changing it.
- Orders already placed are not changed.
- Prices change only if the pricing rule recalculates them; a mapping change alone does not reprice the product.

Say these points in the preview, especially when the request started from an order.

## AI Mapping

AI Mapping finds supplier products that look like the store product, then computes variant links for the one the user picks. Starting a computation uses AI Credits; looking at candidates does not. A computed result is a suggestion until the user agrees and it is applied. If a product was already AI-mapped, recomputing it is a separate, confirmed step.
