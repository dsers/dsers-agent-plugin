# DSers — Dropshipping Plugin for Claude

Run your [DSers](https://www.dsers.com/) dropshipping operations from Claude.
The plugin connects Claude to the DSers MCP server and adds skills for the
daily work of a dropshipping store: finding products and suppliers, preparing
and publishing listings, checking profit and prices, keeping suppliers linked,
processing orders, and tracking parcels.

Before Claude changes anything in DSers or in your store, it shows exactly
what will change and waits for you to agree in the chat.

## Skills

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

Claude picks the skill that fits your request. You can also choose one from
the `/` menu.

## Example requests

- "Place today's orders."
- "Find a supplier for this phone stand that ships to the US in under 10 days."
- "Clean up the titles of the products in my Import List."
- "Am I making money this month?"
- "Which of my products have no supplier?"
- "Why can't I place order 1052?"
- "Which parcels are stuck?"

## Requirements

- A DSers account. [Sign up](https://accounts.dsers.com/accounts/register?utm_source=claude_plugin&utm_medium=ai_assistant&utm_campaign=dsers_plugin&utm_content=readme)
- A DSers plan that includes MCP Connection. See the
  [DSers pricing page](https://www.dsers.com/pricing).
- At least one store connected in DSers, for tasks that touch your store.

## Install

### Claude Code

```text
/plugin marketplace add dsers/dsers-agent-plugin
/plugin install dsers@dsers
```

Then run `/mcp`, select the DSers server, and sign in to DSers in the browser
window that opens.

### Claude on the web, desktop, and Cowork

Once DSers is listed in the Claude directory, add it from
**Customize > Plugins**, then connect DSers from the plugin's
**Connectors** tab and sign in. On Team and Enterprise plans, ask an owner if
the plugin is not available to you.

You never paste a password or API key into Claude. DSers uses OAuth sign-in.

Confirm the connection with a read-only request:

> Show my DSers plan and list my connected stores. Do not make any changes.

## Troubleshooting

- **No DSers tools:** in Claude Code, run `/mcp` and check that the DSers
  server is connected; sign in if it asks. In the Claude apps, check that
  DSers is connected under **Customize > Connectors**.
- **Plan or permission errors:** Claude reports the message DSers returns.
  If it says your plan does not include MCP Connection, see the
  [DSers pricing page](https://www.dsers.com/pricing).
- **Wrong DSers account:** disconnect DSers, sign out of DSers in the
  browser, then connect again and sign in with the account you want.

## Remove

- **Claude Code:** `/plugin uninstall dsers@dsers`.
- **Claude apps:** remove the plugin from **Customize > Plugins** and
  disconnect DSers under **Customize > Connectors**.

## Data, network, and approvals

- The plugin contains skills and a connection to `https://ai.dsers.com/mcp`.
  It has no scripts, hooks, agents, or background processes.
- When DSers is connected, Claude sends requests to DSers under your DSers
  sign-in, and DSers processes that data under its
  [privacy policy](https://www.dsers.com/privacy).
- DSers tools include write operations, such as publishing products,
  changing prices, and placing supplier orders. The skills ask for your
  agreement in the chat before each change, and Claude's own tool permission
  prompts still apply.

## Support

- DSers account and service questions:
  [DSers Help Center](https://help.dsers.com/) or `support@dserspro.com`.
- Plugin issues:
  [GitHub Issues](https://github.com/dsers/dsers-agent-plugin/issues).

## License

[MIT](LICENSE)
