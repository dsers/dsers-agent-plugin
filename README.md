# DSers Agent Plugins

DSers plugins for agent platforms, maintained in one repository. Each plugin
has its own directory, manifests, installation instructions, and
validation record.

## Plugins

| Platform | Package | Status |
| --- | --- | --- |
| Hermes Agent | [plugins/hermes](plugins/hermes/README.md) | Initial package; static validation passes; OAuth end-to-end testing and catalog submission pending |

Only the Hermes package exists today. Future platform integrations belong in
separate directories under `plugins/` and require their own validation.

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

## License

[MIT](LICENSE) covers the repository's files. Each distributable plugin
includes a copy so the license is retained when installed on its own. Use of
the DSers service remains subject to its service terms and account permissions.
