# DSers Agent Plugins

[DSers](https://www.dsers.com/) plugins help dropshippers run their ecommerce
operations from AI agents: find suppliers and products, manage stores,
prepare order fulfillment, and follow shipment tracking.

Built for dropshippers using multiple sourcing platforms and sales channels,
these plugins bring supplier discovery, product sourcing, and store operations
into an agent conversation. Each operation follows the connected DSers
account's permissions and the host's approvals.

## Sourcing platforms and sales channels

DSers connects both sides of a dropshipping business:

| Role | Platforms listed by DSers |
| --- | --- |
| Sourcing platforms | AliExpress, Alibaba.com, 1688, DSers Verified Agent Network (DVAN) |
| Sales channels | Shopify, WooCommerce, Wix, eBay, TikTok Shop, Amazon, Walmart, BigCommerce, Squarespace, Square, Ecwid, Magento, SHOPLINE, Jumpseller, Big Cartel, Cartpanda, Genstore, LPQV |

The [DSers integrations directory](https://www.dsers.com/integration) describes
the available connections. CSV is also available as a file-based DSers
workflow for other storefronts. The platform list describes the DSers
ecosystem; actions available through each plugin depend on the connected
account, channel, and exposed tools.

## Plugins

Each plugin connects your DSers account to DSers MCP and includes eight DSers
skills for the daily work of a dropship store: product sourcing, listing
editing, publishing, profit and pricing, supplier mapping, order fulfillment,
package tracking, and account and store setup. The skills are the same
workflows in every plugin, written for each host's tool names and setup.

| Platform | Package | Install | Compatibility |
| --- | --- | --- | --- |
| Hermes Agent | [plugins/hermes](plugins/hermes/README.md) | `hermes plugins install dsers/dsers-agent-plugin/plugins/hermes --no-enable`, then the [OAuth setup](plugins/hermes/README.md#install-in-hermes) | Hermes v0.22.5; CLI and Desktop |
| OpenClaw | [plugins/openclaw](plugins/openclaw/README.md) | `openclaw plugins install clawhub:@dsers/dsers`, then Connect in Settings → Plugins → DSers | OpenClaw 2026.10.9 or later |
| Claude | [dsers/claude-dsers-plugin](https://github.com/dsers/claude-dsers-plugin) | Claude Code: `/plugin marketplace add dsers/claude-dsers-plugin`, then `/plugin install dsers-dropshipping-automation@claude-dsers-plugin` | Claude Code |

Every plugin requires a DSers Advanced, Pro, or Enterprise subscription.
Installing a plugin alone does not complete the connection; each guide covers
the DSers sign-in. After setup and authorization, try:

> Show my connected DSers stores and summarize the orders awaiting fulfillment.

## What you can do

| Dropshipping task | DSers capabilities available through account-authorized tools |
| --- | --- |
| Supplier discovery and product sourcing | Find suppliers and products, compare sourcing options, and inspect available shipping methods |
| Listing preparation | Prepare import-list products and manage product content and pricing |
| Supplier mapping | Inspect product-to-supplier mappings and manage supported mapping operations |
| Order fulfillment | Review orders, diagnose blockers, and perform explicitly authorized supplier-order actions |
| Shipment tracking | Inspect packages and tracking events and perform supported tracking updates |

Availability depends on the plan's MCP eligibility, account and store
permissions, tools exposed by the service, any applicable usage quota, and
the host's approval policy. Successful authorization does not guarantee that
a requested operation can be called. See the
[official DSers MCP guide](https://www.dsers.com/en/mcp-server) for current
access requirements and usage rules.

## Repository structure

```text
dsers-agent-plugin/
├── README.md
├── LICENSE
├── assets/
│   └── hermes-catalog.png
└── plugins/
    ├── hermes/
    │   ├── plugin.json
    │   ├── mcp.json
    │   ├── skills/
    │   ├── examples/
    │   ├── README.md
    │   └── LICENSE
    └── openclaw/
        ├── .claude-plugin/plugin.json
        ├── .mcp.json
        ├── openclaw.plugin.json
        ├── package.json
        ├── skills/
        ├── assets/icon.png
        ├── README.md
        └── LICENSE
```

Each plugin's README covers installation and usage. `assets/` holds images
for plugin directory listings; they are not part of any plugin package.

Install each plugin from its own subdirectory. The repository root is an
index, not an installable plugin.

The packages connect to the existing DSers MCP service at
`https://ai.dsers.com/mcp` using OAuth 2.1. This repository does not include
the MCP server implementation or user credentials.

MCP provides the service connection. The skills carry DSers workflows as
Markdown instructions; the packages run no bundled code.

## Find DSers

- [DSers dropshipping platform](https://www.dsers.com/)
- [DSers plugin for Hermes and setup guide](plugins/hermes/README.md)
- [DSers plugin for OpenClaw and setup guide](plugins/openclaw/README.md)
- [DSers service capabilities](https://www.dsers.com/en/mcp-server)

## License

[MIT](LICENSE) covers the repository's files. Each distributable plugin
includes a copy so the license is retained when installed on its own. Use of
the DSers service remains subject to its service terms and account permissions.
