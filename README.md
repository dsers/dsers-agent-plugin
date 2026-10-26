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
your DSers account to Hermes Agent. After installing and authorizing it, try:

> Show my connected DSers stores and summarize the orders awaiting fulfillment.

```sh
hermes plugins install dsers/dsers-agent-plugin/plugins/hermes --no-enable
```

Continue with the [OAuth setup instructions](plugins/hermes/README.md#install-in-hermes).

## Plugins

| Platform | Package | Compatibility |
| --- | --- | --- |
| Hermes Agent | [plugins/hermes](plugins/hermes/README.md) | Hermes v0.22.5; CLI and Desktop agent integration |

Hermes is the currently available integration.

## What you can do

| Dropshipping task | DSers capabilities available through account-authorized tools |
| --- | --- |
| Supplier discovery and product sourcing | Find suppliers and products, compare sourcing options, and inspect available shipping methods |
| Listing preparation | Prepare import-list products and manage product content and pricing |
| Supplier mapping | Inspect product-to-supplier mappings and manage supported mapping operations |
| Order fulfillment | Review orders, diagnose blockers, and perform explicitly authorized supplier-order actions |
| Shipment tracking | Inspect packages and tracking events and perform supported tracking updates |

Availability depends on the DSers plan, account permissions, connected store,
and exposed tools.

## Repository structure

```text
dsers-agent-plugin/
├── README.md
├── LICENSE
└── plugins/
    └── hermes/
        ├── plugin.json
        ├── mcp.json
        ├── examples/
        ├── README.md
        └── LICENSE
```

For Hermes installation and usage, see the
[plugin README](plugins/hermes/README.md).

Install each plugin from its own subdirectory. The repository root is an
index, not an installable plugin.

The packages connect to the existing DSers MCP service at
`https://ai.dsers.com/mcp` using OAuth 2.1. This repository does not include
the MCP server implementation or user credentials.

MCP provides the current service connection. Plugin packages can also contain
workflow instructions and other components supported by their host.

## Find DSers

- [DSers dropshipping platform](https://www.dsers.com/)
- [DSers plugin for Hermes and setup guide](plugins/hermes/README.md)
- [DSers service capabilities](https://www.dsers.com/en/mcp-server)

## License

[MIT](LICENSE) covers the repository's files. Each distributable plugin
includes a copy so the license is retained when installed on its own. Use of
the DSers service remains subject to its service terms and account permissions.
