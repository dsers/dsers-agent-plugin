# Validation evidence

Date: 2026-10-26.

## Hermes package validation

Validated using the unmodified official
[`NousResearch/hermes-agent`](https://github.com/NousResearch/hermes-agent/tree/749cb638ff43f9ce5333c186a1e4f67d473aa4f2)
source at commit `749cb638ff43f9ce5333c186a1e4f67d473aa4f2`.

Called `hermes_cli.agent_plugins.load_agent_plugin()` and
`hermes_cli.plugin_validate.validate_plugin_dir()` against `plugins/hermes/` in
a temporary validation environment with `ruamel.yaml==0.18.16`. No source
patches or credential/configuration overrides were used.

Results:

- Package root: `plugins/hermes/`; repository root is not a plugin package.
- Package: `dsers`, version `0.1.0`.
- MCP servers: `dsers`; one Streamable HTTP server translated.
- Effective portable policy: `trust: untrusted`, `strict_redirect_headers: true`.
- Portable parser diagnostics: none.
- Admission report: `ok: true`; all five reported checks passed.
- Security scan: `safe`.
- Warnings: none.
- Isolation audit: `portable`, `host_ready: true`.

The check count and isolation verdict above describe upstream static validation.
They do not establish an authenticated live connection. The native Hermes CLI
is not installed on this validation machine, so `hermes plugins doctor` and
installation through the CLI were not run. The upstream SHA is a development
snapshot, not a declared minimum supported release.

## Public OAuth discovery

Unauthenticated reads confirmed:

- `https://ai.dsers.com/mcp` returns HTTP 401 with a Bearer challenge linking
  to the protected resource metadata.
- `https://ai.dsers.com/.well-known/oauth-protected-resource` returns JSON
  identifying `https://ai.dsers.com/mcp` and the authorization server
  `https://ai.dsers.com`.
- `https://ai.dsers.com/.well-known/oauth-authorization-server` returns JSON
  declaring authorization code and refresh token grants, PKCE S256, a dynamic
  client registration endpoint, and token endpoint authentication method `none`.

Only public metadata was read. No OAuth registration, user authorization, token
exchange, authenticated `tools/list`, or business tool call was performed.

## Required before catalog submission

Validate and install with the intended released Hermes version, complete
browser authorization with a test DSers account, discover the real tools, and
perform one authorized read-only query. Verify the server's tool annotations
and the host's write approval behavior. Record that release and outcome before
declaring compatibility or submitting a pinned catalog entry.
