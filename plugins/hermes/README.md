# DSers — Dropshipping Plugin for Hermes

Run your [DSers](https://www.dsers.com/) dropshipping operations from Hermes
Agent. Built for dropshippers using AliExpress and Shopify, this plugin brings
product sourcing, supplier mapping, order fulfillment, and shipment tracking
into your agent conversation.

Connect your DSers account, ask for the outcome you need, and let Hermes use
the available DSers actions within your account permissions and approvals.
The service connection uses DSers's authenticated remote tools.

**Status:** tested with Hermes v0.22.5. Package validation and Plugin Doctor
pass. Browser OAuth and authenticated discovery succeeded with 60 tools;
the maintainer also confirmed a successful read-only business query using
DeepSeek. This repository has not yet been listed in the Hermes Plugin Catalog.
See [validation evidence](docs/validation.md).

[中文接入与上架说明](docs/hermes.zh-CN.md)

## For dropshippers and ecommerce teams

| Task | Example request | Relevant discovered DSers tools |
| --- | --- | --- |
| Store overview | “List my connected DSers stores.” | `list_stores` |
| Product sourcing | “Find AliExpress products matching this keyword and compare shipping to the US.” | `search_supplier_products`, `list_supplier_product_shipping_methods_and_cost` |
| Product preparation | “Show the products in my import list and inspect this product's pricing.” | `list_pre_publish_products`, `get_pre_publish_product` |
| Supplier mapping | “Inspect the supplier mapping for this store product.” | `get_store_product_mapping` |
| Order fulfillment | “Show orders awaiting fulfillment and diagnose this order's blockers.” | `list_orders`, `get_order`, `diagnose_order_issue` |
| Shipment tracking | “Check this package's tracking events.” | `list_packages`, `get_package_details` |

The authenticated server also exposes supported write operations such as
listing publication, pricing changes, supplier-order placement, and tracking
updates. Their availability depends on your plan, store, and permissions;
Hermes approvals still apply. The table illustrates use cases, not a promise
that every tool or account combination has been tested.

## Requirements

- A DSers account with access to the requested operations and relevant stores.
- Hermes Agent; this package was validated on v0.22.5.
- A configured model provider in Hermes to interpret requests and call tools.
  The maintainer's read-only smoke test used DeepSeek.
- Browser sign-in to authorize access to DSers.

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
Hermes discovers the tools available to the authorized account at runtime;
the names in the use-case table are examples from the successful discovery test.

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

## Find and install DSers

Direct repository installation is available now. The Hermes Plugin Catalog
entry has not been submitted or accepted yet, so catalog search is not live.
After acceptance and catalog publication, users can search for `dsers` or the
business terms included in its catalog description, such as `dropshipping`,
`AliExpress`, `Shopify`, `fulfillment`, and `tracking`.

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

## Validate and publish

With Hermes installed:

```sh
hermes plugins validate /absolute/path/to/dsers-agent-plugin/plugins/hermes --install-deps
hermes plugins doctor /absolute/path/to/dsers-agent-plugin/plugins/hermes --ci
```

Static validation is separate from OAuth and authenticated tool testing.
Complete login, discover the real tools, and verify one authorized read-only
request before submitting. Do not use a write operation as a smoke test.

Publication requires a public commit containing this package, a full
40-character SHA, and a catalog PR to `NousResearch/hermes-agent`. Set
`subdir: plugins/hermes` in the catalog entry. The
maintainers review the pinned commit. See the
[Hermes catalog submission guide](https://hermes-agent.nousresearch.com/docs/developer-guide/plugins/catalog-submission)
and [local publication checklist](docs/hermes.zh-CN.md).
