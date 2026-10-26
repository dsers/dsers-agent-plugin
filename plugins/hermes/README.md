# DSers Plugin for Hermes

A portable Agent Plugins v1 package that connects an agent to the
[DSers MCP server](https://ai.dsers.com/mcp).
This package targets Hermes Agent and lives in `plugins/hermes/` within the
[DSers Agent Plugins repository](https://github.com/dsers/dsers-agent-plugin).

**Status:** initial package. The Hermes upstream parser and admission validator
pass, and public OAuth discovery has been checked. Authenticated discovery and
a read-only tool call still need an end-to-end test. This repository has not
yet been listed in the Hermes Plugin Catalog. See [validation evidence](docs/validation.md).

[中文接入与上架说明](docs/hermes.zh-CN.md)

## Package

```text
plugin.json                       Plugin identity and Hermes call policy
mcp.json                          Remote MCP connection
examples/hermes.config.yaml        Native Hermes OAuth configuration
LICENSE                           MIT license for this package
```

The plugin does not bundle DSers server code or hard-code a tool list. It
exposes all tools the server returns for the authorized account, subject to
the host's settings and approvals. Available features depend on DSers account
permissions and granted OAuth scopes.

## Install in Hermes

Use a current Hermes release with portable Agent Plugins v1 and remote MCP
support. Before publication, validate against the specific Hermes release
that will be declared in the catalog entry.

Once these files are committed and pushed to this repository:

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

Example requests after connection:

- “Show the DSers capabilities available to my account.”
- “Use the available read-only tools to show my DSers stores.”
- “Check this DSers order's status: `<order ID>`.”

These are example intents; the actual server must expose a matching tool.

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
