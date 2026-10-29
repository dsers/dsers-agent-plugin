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

## Start with Hermes

The [DSers dropshipping plugin for Hermes](plugins/hermes/README.md) connects
your DSers account to Hermes Agent. Setup requires a DSers Advanced, Pro, or
Enterprise subscription and the one-time OAuth connection configuration in
the linked guide. Installing and enabling the plugin alone does not complete
the connection. After setup and authorization, try:

> Show my connected DSers stores and summarize the orders awaiting fulfillment.

```sh
hermes plugins install dsers/dsers-agent-plugin/plugins/hermes --no-enable
```

Continue with the [OAuth setup instructions](plugins/hermes/README.md#install-in-hermes).

## Start with Claude

The [DSers dropshipping plugin for Claude](plugins/claude/README.md) connects
your DSers account to Claude and adds skills for sourcing, listings, pricing,
supplier mapping, orders, and tracking. In Claude Code:

```text
/plugin marketplace add dsers/dsers-agent-plugin
/plugin install dsers@dsers
```

Then run `/mcp` and sign in to DSers. See the
[plugin README](plugins/claude/README.md#install) for the Claude apps.

## Plugins

| Platform | Package | Compatibility |
| --- | --- | --- |
| Hermes Agent | [plugins/hermes](plugins/hermes/README.md) | Hermes v0.22.5; CLI and Desktop agent integration |
| Claude | [plugins/claude](plugins/claude/README.md) | Claude Code, Claude apps, and Cowork |

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
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    ├── hermes/
    │   ├── plugin.json
    │   ├── mcp.json
    │   ├── examples/
    │   ├── README.md
    │   └── LICENSE
    └── claude/
        ├── .claude-plugin/
        ├── .mcp.json
        ├── skills/
        ├── README.md
        └── LICENSE
```

For Hermes installation and usage, see the
[plugin README](plugins/hermes/README.md).

Install each plugin from its own subdirectory. The repository root is an
index, not an installable plugin. For Claude Code, `.claude-plugin/marketplace.json`
lists the Claude plugin so it can be installed from this repository.

The packages connect to the existing DSers MCP service at
`https://ai.dsers.com/mcp` using OAuth 2.1. This repository does not include
the MCP server implementation or user credentials.

MCP provides the current service connection. Plugin packages can also contain
workflow instructions and other components supported by their host.

## Find DSers

- [DSers dropshipping platform](https://www.dsers.com/)
- [DSers plugin for Hermes and setup guide](plugins/hermes/README.md)
- [DSers plugin for Claude and setup guide](plugins/claude/README.md)
- [DSers service capabilities](https://www.dsers.com/en/mcp-server)

## License

[MIT](LICENSE) covers the repository's files. Each distributable plugin
includes a copy so the license is retained when installed on its own. Use of
the DSers service remains subject to its service terms and account permissions.
