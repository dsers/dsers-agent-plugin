# Hermes 接入与上架

## 当前结构

这是一个提供 MCP 连接的 plugin，采用 Hermes 支持的
Agent Plugins v1 格式，完整包位于仓库的 `plugins/hermes/` 目录。
该目录中的 `plugin.json` 放插件信息，`mcp.json` 放公开服务地址。

仓库根目录是多个插件的索引，不能直接当作 Hermes 插件安装。
未来可在同一仓库的 `plugins/` 下增加其他平台的插件目录。
其他平台的兼容性需分别验证，不能仅凭这个格式声称全部支持。

不需要公开 MCP 服务端源码。这里公开的是插件信息和连接配置；用户必须
登录自己的 DSers 账号，最终可用工具由服务端权限和授权范围决定。
插件没有配置工具白名单或黑名单。

## 连接 OAuth

Hermes 当前的通用 `mcp.json` 远程连接只接受 `type`、`url` 和可选的
`headers`，不能在这里写 `auth: oauth`。需要把
[配置示例](../examples/hermes.config.yaml) 合并到用户当前 Hermes profile
的 `config.yaml`。默认 profile 通常使用 `~/.hermes/config.yaml`。

保留已有配置，尤其不要覆盖其他 `mcp_servers`。服务名必须为 `dsers`：
同名原生配置会完整覆盖插件里的 MCP 配置，因此示例同时保留
`trust: untrusted` 和 `strict_redirect_headers: true`。

代码提交并推送到仓库后，用户可以这样安装并登录：

```sh
hermes plugins install dsers/dsers-agent-plugin/plugins/hermes --no-enable
# 合并 OAuth 配置后，再执行下面的命令。
hermes plugins enable dsers
hermes mcp login dsers
hermes mcp test dsers
```

浏览器中的 DSers 登录及同意授权由用户完成。Hermes 管理动态客户端
注册、PKCE、凭证保存和刷新。不要把 access token、refresh token、
授权码或真实用户数据提交到 GitHub。配置完成后重新启动 Hermes 会话。

`trust: untrusted` 不隐藏工具。它让 Hermes 在调用未标注只读的工具时
要求批准。没有 `readOnlyHint: true` 的只读工具也可能触发批准。

## 已确认与待验证

2026-10-26，使用 Hermes 官方源码的解析器及准入验证器检查 `plugins/hermes/`，
识别到 1 个 MCP 服务，无诊断警告，安全扫描为 `safe`。
具体源码 SHA 和验证范围见[验证记录](validation.md)。当前机器未安装
Hermes CLI；这次使用官方 Python 验证入口，并非完整 CLI/OAuth 联调。

已检查公开发现端点：

- MCP：`https://ai.dsers.com/mcp`，未登录返回 HTTP 401，并提供资源元数据地址。
- 资源元数据：`https://ai.dsers.com/.well-known/oauth-protected-resource`。
- 授权服务器：`https://ai.dsers.com/.well-known/oauth-authorization-server`。
- 元数据声明支持 PKCE S256、动态客户端注册、授权码和 refresh token。

以上只能说明发现信息存在，不能证明完整 OAuth 流程和工具调用成功。
正式提交前还需完成：

1. 在实际目标 Hermes 版本上运行官方验证命令：
   `hermes plugins validate /绝对路径/dsers-agent-plugin/plugins/hermes --install-deps`。
2. 运行 `hermes plugins doctor /绝对路径/dsers-agent-plugin/plugins/hermes --ci`。
3. 安装插件，合并 OAuth 配置，使用测试账号完成浏览器授权。
4. 确认 Hermes 发现实际工具；检查服务端工具的输入 schema 和只读标注。
5. 完成一次用户授权的只读查询，确认能返回预期结果。不要为验证连接
   创建、修改或删除真实业务数据。
6. 移除 README 的未验证状态说明之前，补充实际通过的版本和验证结果。

## 提交 Hermes Catalog

1. 完成以上验证，提交并推送源码。记录插件版本和完整 40 位 commit SHA。
2. 根据 Hermes 官方目录的当前 schema，向
   `NousResearch/hermes-agent` 新增 `plugin-catalog/dsers.yaml`。
   仓库地址使用 `https://github.com/dsers/dsers-agent-plugin`，必须填写
   `subdir: plugins/hermes`，让安装器和审核 CI 定位到 Hermes 插件目录。
3. 版本填写 `"0.1.0"`；`sha` 使用实际发布提交。`maintainer` 使用真实
   提交者账号；`requires_hermes` 使用已测试且确实支持所需功能的
   SemVer 版本。不要把日期、分支名或占位符作为发布值。
4. 目录默认以 `community` 申请；`verified` 或 `official` 由 Nous 团队
   决定。README 需保持英文优先。
5. 在 PR 说明 DSers 功能、通用插件格式、OAuth 配置这一步
   用户操作，以及网络访问、宿主管理的凭证和可能的写操作。
   MCP 工具动态发现，不要把未验证的工具名或数量当成固定能力声明。
6. 等 CI 与人工审核。合并并完成目录发布后，可以通过
   `hermes plugins search dsers` 搜索，并使用 `hermes plugins install dsers`
   安装。对应目录页面预期为
   `https://hermes-agent.nousresearch.com/docs/plugins/dsers`；当前尚未上架。

仅把源码推到自己的 GitHub 仓库，不会自动出现在 Hermes 官方目录。
更新目录版本时，也需要新的 SHA 更新 PR。

官方依据：
[插件格式](https://hermes-agent.nousresearch.com/docs/developer-guide/plugins)、
[MCP 与 OAuth](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp)、
[目录提交要求](https://hermes-agent.nousresearch.com/docs/developer-guide/plugins/catalog-submission)。
