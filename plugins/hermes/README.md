# DSers — Dropshipping Plugin for Hermes

Run your [DSers](https://www.dsers.com/) dropshipping operations from Hermes
Agent. Built for dropshippers using multiple sourcing platforms and sales
channels, this plugin helps you find suppliers and products, manage stores,
fulfill orders, and track shipments in your agent conversation.

Connect your DSers account, ask for the outcome you need, and let Hermes use
the available DSers actions within your account permissions and approvals.
The service connection uses DSers's authenticated remote tools, and the
plugin's skills give Hermes DSers's steps for the daily work of a dropship
store. The skills tell Hermes to show what will change and wait for your
agreement in the chat before any change in DSers or your store, and Hermes
asks for approval before it calls a DSers tool that is not read-only.

Setup has two parts: install and enable this plugin, then configure and
authorize the DSers OAuth connection as described in
[Install in Hermes](#install-in-hermes). Installing and enabling the plugin
alone does not complete the connection.

## Multiple sourcing platforms and sales channels

DSers's [integration ecosystem](https://www.dsers.com/integration) includes:

- Sourcing platforms: AliExpress, Alibaba.com, 1688, and the DSers Verified
  Agent Network (DVAN).
- Sales channels: Shopify, WooCommerce, Wix, eBay, TikTok Shop, Amazon, Walmart,
  BigCommerce, Squarespace, Square, Ecwid, Magento, SHOPLINE, Jumpseller,
  Big Cartel, Cartpanda, Genstore, and LPQV.

These are DSers platform connections. The plugin's available actions depend
on the tools and permissions exposed for your account and connected channels.
DSers also offers a CSV workflow for storefronts without a direct integration.

## For dropshippers and ecommerce teams

| Task | Example request | Relevant discovered DSers tools |
| --- | --- | --- |
| Store overview | “List my connected DSers stores.” | `list_stores` |
| Supplier discovery and product sourcing | “Find supplier products matching this keyword, compare the available suppliers and shipping to the US, and show which sourcing platforms are available to my account.” | `search_supplier_products`, `list_supplier_product_shipping_methods_and_cost`, `list_stores` |
| Product preparation | “Show the products in my import list and inspect this product's pricing.” | `list_pre_publish_products`, `get_pre_publish_product` |
| Supplier mapping | “Inspect the supplier mapping for this store product.” | `get_store_product_mapping` |
| Order fulfillment | “Show orders awaiting fulfillment and diagnose this order's blockers.” | `list_orders`, `get_order`, `diagnose_order_issue` |
| Shipment tracking | “Check this package's tracking events.” | `list_packages`, `get_package_details` |

The authenticated server also exposes supported write operations such as
listing publication, pricing changes, supplier-order placement, and tracking
updates. Their availability depends on your plan, store, and permissions;
Hermes approvals still apply.

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

While the plugin is enabled, Hermes offers these skills to the agent and loads
the one that fits your request.

## Requirements

- A DSers account with access to the requested operations and relevant stores.
  [Sign up](https://accounts.dsers.com/accounts/register?utm_source=hermes_plugin&utm_medium=ai_assistant&utm_campaign=dsers_plugin&utm_content=readme)
- A DSers Advanced, Pro, or Enterprise subscription is required to use the
  connected MCP service. The Basic plan does not include MCP access.
- At least one Shopify or Wix store connected in DSers.
- Hermes Agent; this package was validated on v0.22.5.
- A configured model provider in Hermes to interpret requests and call tools.
- Browser sign-in to authorize access to DSers.

## CLI and Desktop

Use the same plugin from the Hermes CLI or from a chat in Hermes Desktop.
When both connect to the same backend and profile, they use that profile's
plugin installation, DSers connection settings, and saved OAuth authorization.
If Desktop connects to a remote backend, install and authorize the plugin on
that backend in the profile you select.

In Desktop, the plugin provides DSers tools to the agent's chat. It does not
add a separate DSers pane. See the
[Hermes Desktop guide](https://hermes-agent.nousresearch.com/docs/user-guide/desktop)
for profile and backend selection.

## Package

```text
plugin.json                       Plugin identity and Hermes call policy
mcp.json                          Remote MCP connection
skills/                           Eight DSers workflow skills
examples/hermes.config.yaml        Native Hermes OAuth configuration
LICENSE                           MIT license for this package
```

This is a portable Agent Plugins v1 package in `plugins/hermes/` of the
[DSers Agent Plugins repository](https://github.com/dsers/dsers-agent-plugin).
MCP supplies the service connection and the skills supply DSers's workflows;
plugin metadata, documentation, and host policy also belong to the plugin
package. The skills name DSers tools as Hermes registers them
(`mcp__dsers__<tool>`), so keep the server name `dsers` in the OAuth
configuration below.

The plugin does not bundle DSers server code or maintain a fixed tool list.
Hermes discovers the tools available to the authorized account at runtime.
The remote service and its tool set can change independently of the installed
plugin version; the plugin version identifies the connection package.

## Install in Hermes

Tested release: Hermes v0.22.5. Use a release with portable Agent Plugins v1
and remote MCP support. Earlier releases have not been validated by this project.

Install directly from this repository:

```sh
hermes plugins install dsers/dsers-agent-plugin/plugins/hermes --no-enable
```

Enable OAuth in the **active profile's** `config.yaml`. For the default profile
this is normally `~/.hermes/config.yaml`. Merge the following with your
existing `mcp_servers` map; preserve other configuration:

```yaml
mcp_servers:
  dsers:
    url: https://ai.dsers.com/mcp
    auth: oauth
    trust: untrusted
    strict_redirect_headers: true
```

On the validated Hermes v0.22.5 release, this step is required: the portable
remote MCP adapter does not accept an `auth` field in `mcp.json`.
A same-name native server overrides the portable entry, including its call
policy. Keep the settings above.

```sh
hermes plugins enable dsers
hermes mcp login dsers
hermes mcp test dsers
```

Complete sign-in and consent in the browser. The server advertises OAuth 2.1,
PKCE S256, dynamic client registration, and refresh tokens. No API key or
manually embedded bearer token is needed. Restart the Hermes session after
setup so the MCP tools are available in the session.

Start a new conversation with `hermes chat` and try the read-only store overview
in the table above. If Hermes has no model configured, run `hermes model` first.

Successful authorization confirms sign-in, but does not guarantee access to
every business operation. Each call depends on the plan's MCP eligibility,
account and store permissions, tools exposed by the service, any applicable
usage quota, and the host's approval policy. Tool discovery alone does not
establish that an operation is permitted or that quota remains available.
Confirm setup with a read-only request:

> Show my DSers plan and list my connected stores. Do not make any changes.

If access is denied, check the account's subscription and permissions. A Basic
account cannot use the MCP service even if sign-in or tool discovery succeeds.

## Troubleshooting

- **No DSers tools in chat:** check that the connection was configured in the
  same profile and backend as the chat, run `hermes mcp test dsers`, and restart
  the session after setup. A model provider must also be configured.
- **Authorization expired or the wrong account is connected:** sign out of
  DSers in the browser if switching accounts, then run `hermes mcp login dsers`
  and authorize the intended account. This replaces the saved MCP login for
  `dsers` in the active profile. Restart affected sessions afterward.
- **Plan or operation restriction:** confirm Advanced, Pro, or Enterprise
  access and the required store permissions. Available tools do not guarantee
  permission to execute every operation. See the
  [DSers service reference](https://www.dsers.com/en/mcp-server) for current
  access requirements and usage limits.
- **Usage limit reached:** check the service's returned error and any reset
  information. See the [official DSers MCP guide](https://www.dsers.com/en/mcp-server)
  for current metering rules and plan limits.

For DSers account or service assistance, use the
[DSers Help Center](https://help.dsers.com/) or email `support@dserspro.com`.
Report plugin packaging or documentation issues through
[GitHub Issues](https://github.com/dsers/dsers-agent-plugin/issues).

## Remove the plugin and connection

The manually configured `mcp_servers.dsers` connection is independent of the
plugin. Disabling or removing the plugin alone leaves that connection and its
cached MCP OAuth credentials in place, so DSers tools may remain available.

To remove both from the active profile, run:

```sh
hermes plugins remove dsers
hermes mcp remove dsers
```

The second command removes the native connection and clears its cached MCP
OAuth credentials. Restart affected sessions or backends afterward. Local
credential removal does not itself revoke the grant on the DSers service.

## Links

[DSers product website](https://www.dsers.com/) ·
[DSers service capability reference](https://www.dsers.com/en/mcp-server) ·
[Plugin source](https://github.com/dsers/dsers-agent-plugin/tree/main/plugins/hermes)

## Data, network, and approvals

- The host connects to `https://ai.dsers.com` for MCP and OAuth. Tool arguments
  and results are exchanged with DSers and processed by the agent host.
- Hermes manages OAuth credentials in the active profile. This package does
  not read credential stores or implement token persistence or refresh.
- DSers tools can include write operations. `trust: untrusted` asks for host
  approval for tools that are not annotated as read-only; tools with missing
  annotations may also prompt. No tool filter is applied by this package.
- The package contains connection manifests, skills written as Markdown
  instructions, and documentation. It runs no bundled code: no server, scripts,
  shell hooks, background process, or separate telemetry collector.
- MIT covers this repository's files. Use of the DSers service remains
  subject to DSers service terms and the user's account permissions.
