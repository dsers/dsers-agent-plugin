# DSers Agent Plugins

[DSers](https://www.dsers.com/) plugins help dropshippers run their ecommerce
operations from AI agents: find products, work with suppliers, manage stores,
prepare order fulfillment, and follow shipment tracking.

Built for AliExpress dropshipping and Shopify merchants using DSers, these
integrations bring account-aware actions into an agent conversation. Each
operation follows the connected account's permissions and the host's approvals.

## Start with Hermes

The [DSers dropshipping plugin for Hermes](plugins/hermes/README.md) connects
your DSers account to Hermes Agent. After installing and authorizing it, try:

> Show my connected DSers stores and summarize the orders awaiting fulfillment.

```sh
hermes plugins install dsers/dsers-agent-plugin/plugins/hermes --no-enable
```

Continue with the [OAuth setup instructions](plugins/hermes/README.md#install-in-hermes).

## Plugins

| Platform | Package | Status |
| --- | --- | --- |
| Hermes Agent | [plugins/hermes](plugins/hermes/README.md) | Validated on Hermes v0.22.5; OAuth and tool discovery passed; read-only query confirmed by maintainer; catalog submission pending |

Only the Hermes package exists today. Future platform integrations belong in
separate directories under `plugins/` and require their own validation.

## What you can do

| Dropshipping task | DSers capabilities available through account-authorized tools |
| --- | --- |
| Product sourcing | Search supplier products and compare available shipping options |
| Listing preparation | Prepare import-list products and manage product content and pricing |
| Supplier mapping | Inspect product-to-supplier mappings and manage supported mapping operations |
| Order fulfillment | Review orders, diagnose blockers, and perform explicitly authorized supplier-order actions |
| Shipment tracking | Inspect packages and tracking events and perform supported tracking updates |

Availability depends on the DSers plan, account permissions, connected store,
and exposed tools. The completed smoke test covers a read-only query, not
every operation in the table. See [validation evidence](plugins/hermes/docs/validation.md).

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
        ├── docs/
        ├── README.md
        └── LICENSE
```

For Hermes installation and publication, see the
[plugin README](plugins/hermes/README.md) or
[中文说明](plugins/hermes/docs/hermes.zh-CN.md).

Catalog entries and direct installs must point at the relevant plugin
subdirectory. The repository root is an index, not an installable plugin.

The packages connect to the existing DSers MCP service at
`https://ai.dsers.com/mcp` using OAuth 2.1. This repository does not include
the MCP server implementation or user credentials.

MCP is the current service connection within the plugin. A plugin package can
also include workflow instructions and other host-supported components as it
evolves. This release does not bundle workflow skills; future capabilities will
be documented when released.

## Find DSers

- [DSers dropshipping platform](https://www.dsers.com/)
- [DSers plugin for Hermes and setup guide](plugins/hermes/README.md)
- [DSers service capabilities](https://www.dsers.com/en/mcp-server)
- [中文接入与上架说明](plugins/hermes/docs/hermes.zh-CN.md)
- [Search and listing plan](docs/discovery.zh-CN.md)

Hermes Catalog submission is pending. Until it is accepted, install using the
repository command above. Catalog search by name or dropshipping use case
becomes available only after the entry is published.

## License

[MIT](LICENSE) covers the repository's files. Each distributable plugin
includes a copy so the license is retained when installed on its own. Use of
the DSers service remains subject to its service terms and account permissions.
