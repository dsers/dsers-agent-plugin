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

The check count and isolation verdict above describe the initial upstream
static validation. The upstream SHA is a development snapshot, not a declared
minimum supported release. Released-CLI and live connection results follow.

## Released Hermes CLI and connection testing

Environment: macOS arm64, Hermes v0.22.5, release commit
`bf0203a794a0` (short SHA reported by the CLI), Python 3.14.9.
The installed plugin source was commit
`194c1dd9d1633e12e8859e2f626d2a2b7617b913` of this repository.

- `hermes plugins validate <repository>/plugins/hermes`: passed, including
  the security scan (`safe`).
- `hermes plugins doctor <repository>/plugins/hermes --ci`: passed runtime
  discovery, manifest parsing, import, and registration checks. It reported
  zero native tool/hook registrations; remote MCP tools are discovered by
  the MCP client rather than registered as native Python tools.
- GitHub subdirectory installation: the maintainer supplied the successful
  install output for `dsers/dsers-agent-plugin/plugins/hermes`.
- Browser OAuth: supplied output reported `Authenticated — 60 tool(s) available`.
- `hermes mcp test dsers`: supplied output reported a successful HTTP
  connection and 60 discovered tools.
- Read-only business query: after configuring DeepSeek, the maintainer
  confirmed that the requested read-only test completed successfully. This
  outcome is maintainer-reported; no business response data is stored here.

The tool count records this test account at this time and is not a fixed
capability promise. This evidence does not establish that all 60 tools, write
approvals, token refresh, or every account configuration have been tested.

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

This initial discovery check read only public metadata. Subsequent browser
authorization and authenticated testing are recorded in the preceding section.

## Required before catalog submission

Commit and publish these verification notes, then pin the resulting full
40-character commit SHA in the catalog entry with `subdir: plugins/hermes`.
Use `requires_hermes: ">=0.22.5"` as the tested release floor; earlier releases
have not been validated. Re-run package validation at the final pinned tree.
Write approval behavior and token refresh remain outside the completed test
scope and must not be described as verified in the submission.
