# DSers

Connect your favorite AI client to DSers and move from product discovery to listings, store operations, orders, fulfillment, and delivery tracking — all in one conversation.

This is the DSers plugin for OpenClaw. It connects OpenClaw to [DSers MCP](#dsers-mcp) and adds skills for the daily work of a dropship store: finding products and suppliers, cleaning up and publishing listings, checking profit and pricing rules, keeping supplier mapping working, placing supplier orders, and tracking parcels.

Before anything changes in DSers or in your store, OpenClaw shows what will change and waits for you to agree in the chat.

## What's included

- **DSers MCP**: a connection to `https://ai.dsers.com/mcp`. You sign in to DSers with OAuth.
- **Skills**:

| Skill | What it helps with |
| --- | --- |
| account-and-stores | Getting started, connected stores and supplier accounts, reconnecting a store, plan, AI Credits, invoices |
| product-sourcing | Product ideas, supplier search by keyword or image, comparing cost, stock, and shipping, adding products to the Import List |
| listing-editing | Cleaning up imported titles, descriptions, variants, and images, prices before publishing, collections and tags |
| product-publishing | Publishing Import List products to your stores with the shipping and publish settings you choose |
| profit-and-pricing | Profit per order and product after fees and ads, pricing rules, store-wide and single-product price changes |
| supplier-mapping | Linking store products to suppliers, backup suppliers, suppliers by country, bundles, AI Mapping, supplier changes |
| order-fulfillment | Why orders are stuck, fixing addresses and shipping methods, placing orders with suppliers, payment links |
| package-tracking | Parcel status, delivery exceptions, sending tracking to your store, buyer notifications, delivery disputes |

OpenClaw picks the skill that fits your request.

## Try it

- "Place today's orders."
- "Find a supplier for this phone stand that ships to the US in under 10 days."
- "Clean up the titles of the products in my Import List."
- "Am I making money this month?"
- "Which of my products have no supplier?"
- "Why can't I place order 1052?"
- "Which parcels are stuck?"

## Install

```bash
openclaw plugins install clawhub:@dsers/dsers-dropshipping-automation
```

If the Gateway is already running, restart it so it loads the plugin:

```bash
openclaw gateway restart
```

Then connect DSers: open Settings → Plugins → DSers, select **Connect** next to dsers under Accounts, and sign in to DSers in the browser window that opens. Only a Gateway administrator can do this, and the sign-in is shared by everyone who uses this Gateway.

You never paste a password or API key into OpenClaw. Check the connection with a read-only request:

> Show my DSers plan and list my connected stores. Do not make any changes.

## Requirements

- OpenClaw 2026.10.9 or later.
- A DSers account. [Sign up](https://accounts.dsers.com/accounts/register?utm_source=openclaw_plugin&utm_medium=ai_assistant&utm_campaign=dsers_plugin&utm_content=readme)
- A DSers Advanced, Pro, or Enterprise plan. These plans include MCP Connection; Basic does not ([DSers MCP documentation](https://www.dsers.com/en/mcp-server), [DSers pricing page](https://www.dsers.com/pricing)).
- At least one Shopify or Wix store connected in DSers.

## Stores and suppliers

From the [DSers integrations page](https://www.dsers.com/en/integration):

- **Sales channels**: DSers supports Shopify, Wix, WooCommerce, eBay, TikTok Shop, Amazon, Walmart, Big Cartel, Jumpseller, Cartpanda, Square, Ecwid, BigCommerce, Squarespace, SHOPLINE, LPQV, Magento, Genstore.ai, and CSV workflows. Check the channel list inside DSers for the latest availability.
- **Sourcing**: DSers supports sourcing workflows for AliExpress, Alibaba.com, 1688, and the DSers Verified Agent Network.

DSers MCP needs at least one Shopify or Wix store connected in DSers ([DSers MCP documentation](https://www.dsers.com/en/mcp-server)).

## About DSers

DSers is an ecommerce platform founded in 2018 that helps sellers spend less time on repetitive operations and more time building their business — supporting dropshipping and wholesale through connected sourcing, automation, and fulfillment workflows. As an official Alibaba Group partner, DSers supports 10M+ dropshippers, with 100M+ SKUs in the product pool and 300M+ orders processed. DSers supports sourcing across AliExpress, 1688.com, Alibaba.com, and the DSers Agent Network, giving sellers transparent options as their sourcing needs evolve.

Technology should understand more, automate more, and stay dependable—so people can focus on judgment, creativity, relationships, and growth. [More about DSers](https://www.dsers.com/en/about-us)

## DSers MCP

DSers MCP is the official MCP server from DSers for AI-powered dropshipping automation. Connect your favorite AI client to DSers and move from product discovery to listings, store operations, orders, fulfillment, and delivery tracking — all in one conversation.

What you can do with DSers MCP:

- **Discover products and logistics**: Search by keyword or image, inspect supplier products, compare logistics by country, and monitor supplier-product changes.
- **Build and optimize import drafts**: Create import-list products, then edit content, images, options, variants, prices, organization, and categories before launch.
- **Configure stores and publish**: Connect or reauthorize stores, create Shopify shipping profiles, resolve logistics choices, and publish through tracked jobs.
- **Operate catalogs and mappings**: Inspect live products, update price or stock, import store products, and manage Basic, Advanced, or AI-assisted supplier mappings.
- **Run orders and supplier fulfillment**: Validate addresses, update order details, and place or cancel supplier orders.
- **Control tracking and buyer updates**: Inspect package events, refresh supplier tracking, synchronize or replace store tracking numbers, and send buyer notifications.

DSers MCP works alongside the DSers dashboard. [DSers MCP documentation](https://www.dsers.com/en/mcp-server)

## Data, network, and approvals

- The plugin contains skills and a connection to `https://ai.dsers.com/mcp`. It has no code, scripts, hooks, or background processes.
- When DSers is connected, OpenClaw sends requests to DSers under the DSers account that signed in, and DSers processes that data under its [privacy policy](https://www.dsers.com/en/privacy).
- DSers tools include write operations: placing supplier orders, which spends money through your DSers account; changing prices; and publishing listings. The skills ask for your agreement in the chat before each change, and OpenClaw's own tool approval settings still apply.

## Troubleshooting

- **No DSers tools**: open Settings → Plugins → DSers. Check that the plugin is enabled and that dsers under Accounts is connected; connect again if it asks.
- **Plan or permission errors**: OpenClaw reports the message DSers returns. If it says your plan does not include MCP Connection, see the [DSers pricing page](https://www.dsers.com/pricing).

## Remove

```bash
openclaw plugins uninstall dsers
```

Or select **Uninstall** in Settings → Plugins → DSers.

## Support

DSers account, plan, and DSers MCP questions: [DSers Help Center](https://help.dsers.com/) or support@dserspro.com

Plugin packaging or documentation issues: [GitHub Issues](https://github.com/dsers/dsers-agent-plugin/issues)

## License

[MIT](LICENSE)
