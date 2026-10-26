# DSers — Dropshipping Plugin for Hermes

Run your [DSers](https://www.dsers.com/) dropshipping operations from Hermes
Agent. Built for dropshippers using multiple sourcing platforms and sales
channels, this plugin helps you find suppliers and products, manage stores,
fulfill orders, and track shipments in your agent conversation.

Connect your DSers account, ask for the outcome you need, and let Hermes use
the available DSers actions within your account permissions and approvals.
The service connection uses DSers's authenticated remote tools.

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

## Requirements

- A DSers account with access to the requested operations and relevant stores.
- A DSers Advanced, Pro, or Enterprise subscription is required to use the
  connected MCP service. The Basic plan does not include MCP access.
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
examples/hermes.config.yaml        Native Hermes OAuth configuration
LICENSE                           MIT license for this package
```

This is a portable Agent Plugins v1 package in `plugins/hermes/` of the
[DSers Agent Plugins repository](https://github.com/dsers/dsers-agent-plugin).
MCP supplies its current service connection; plugin metadata, documentation,
host policy, and future workflow components belong to the plugin package.
No workflow skills are bundled in this release.

The plugin does not bundle DSers server code or maintain a fixed tool list.
Hermes discovers the tools available to the authorized account at runtime.

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

This step is required: Hermes's portable remote MCP adapter currently does
not accept an `auth` field in `mcp.json`. A same-name native server overrides
the portable entry, including its call policy. Keep the settings above.

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
- The package contains connection manifests and documentation. It launches no bundled server,
  shell hooks, background process, or separate telemetry collector.
- MIT covers this repository's files. Use of the DSers service remains
  subject to DSers service terms and the user's account permissions.
