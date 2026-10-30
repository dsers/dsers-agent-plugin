# Publish settings

`dsers__list_stores` returns each store's publish defaults, but DSers does not apply them by itself. A setting the publish call leaves out gets a fixed value, not the store's default: active, hidden from the online store, no other sales channels, automatic stock and price updates off, the store's pricing rule not applied, and not taxable. Only the language and the Shopify shipping profile follow the store. So send every setting in the call, using the store's default unless the user chose otherwise, and show the value of each one in the preview, in these words.

| Setting | In plain words | Suggest |
|---|---|---|
| Product status: active or draft | Active products can be sold; drafts wait in the store admin for review | Draft for a first batch the seller wants to check in the store; active when they want the products on sale |
| Visible in the online store | Whether the product appears on the store's website | On when the products should be on sale; off keeps them hidden from the website even when active |
| Other sales channels | Also offer the product on channels connected to the store, such as Shop, Google & YouTube, TikTok, or Facebook & Instagram. A channel the store does not have is ignored | Only the channels the seller uses |
| Keep selling when out of stock | Customers can still buy a variant with no stock | Off for dropshipping, so the store does not take orders the supplier cannot fill |
| Automatic stock updates | DSers updates store stock from supplier stock | On, so sold-out supplier variants stop selling |
| Apply the store's pricing rule (per product and store) | Prices the product with the store's pricing rule instead of its Import List price | The store's default from `dsers__list_stores` |
| Automatic price updates | DSers updates store prices when supplier costs change, using the pricing rule | On when the store uses a pricing rule and prices should follow costs; off when prices are set by hand |
| Allow manual price updates | Allows manual price changes after publishing (the tool's default is on) | Keep the default (on) unless the seller asks |
| Description | Push the supplier's overview, its specifications, or both | Both, after the description has been cleaned up |
| Images | Only the images selected in the Import List, or every available image | Selected only, after unwanted images were removed |
| Taxable | Whether the store charges tax on the product | Ask the seller; it depends on where they sell and their tax setup |
| Language (per store) | The language of the published text; the store's choices come from `dsers__list_stores` | The store's language |
| Shopify shipping profile and location | Which shipping profile and location the product uses in Shopify | The store's general profile, unless the seller uses a specific one |

If a setting's effect is not clear from this table or the tool's description, show its value and say you are not certain what it changes, rather than guessing.
