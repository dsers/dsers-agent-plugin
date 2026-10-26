# DSers 插件的定位与发现渠道

## 面向用户的定位

插件展示名称建议为 **DSers — Dropshipping for Hermes**，安装标识保持
`dsers`。描述围绕 dropshippers 的任务展开：选品、供应商映射、店铺管理、
订单履约和物流追踪。MCP 是当前连接方式，在技术和认证说明中介绍。

当前版本不附带 workflow skills。后续可以在插件包中增加工作流组件，
但只在实际发布时宣传其能力。插件名称不绑定某一种技术组件。

## 各渠道实际检索什么

| 渠道 | 当前可优化的位置 | 限制 |
| --- | --- | --- |
| GitHub | 仓库简介、topics、官网链接和根 README | 有助于描述和发现项目，不能保证排名或收录时间 |
| 插件宿主 | `plugin.json` 的名称、业务描述、官网和 keywords | 各宿主如何索引这些字段需分别确认 |
| Hermes CLI Catalog | Catalog 条目的 `name`、`description`、`capabilities.provides_tools` | v0.22.5 使用不区分大小写的子串匹配；不读取 manifest keywords、README 或目录 title |
| Hermes 目录页面 | Catalog `title`、`description`、`category` 和固定 SHA 的 README | 需人工审核合并，并完成目录发布后才存在页面 |
| 外部搜索引擎 | 公开 README、GitHub 页面、DSers 官网的相关页面和链接 | 收录与排名由搜索引擎决定，不能承诺精确召回或首位 |

Hermes 当前不是语义搜索。例如 `dropshipping` 不等于 `drop shipping`，
增加 manifest keywords 不会让尚未进入 Catalog 的仓库出现在
`hermes plugins search` 中。不要为扩大搜索面虚构原生工具声明。

## 建议用于 Catalog 的信息

以下是提交元信息，不是已经上线的目录条目。实际提交时还需要最终
40 位 SHA 和真实提交者/维护者信息。

```yaml
name: dsers
title: DSers — Dropshipping for Hermes
repo: https://github.com/dsers/dsers-agent-plugin
subdir: plugins/hermes
description: >-
  DSers ecommerce plugin for dropshippers: dropshipping and drop shipping with
  AliExpress and Shopify, product sourcing, supplier mapping, order fulfillment,
  shipping and shipment tracking.
category: tools
tier: community
requires_hermes: ">=0.22.5"
version: "0.1.0"
readme: true
```

目录描述覆盖品牌词、用户人群、主要生态和任务词。目录 `title` 用于展示，
命令行搜索能否命中主要依赖上述 `description`。`community` 是提交起点；
更高 tier 由 Nous 决定，不能把 DSers 自有品牌身份等同于 Hermes 审核认证。

审核及目录发布后，验证以下搜索词：

```sh
hermes plugins search dsers
hermes plugins search dropshipping
hermes plugins search dropshipper
hermes plugins search "drop shipping"
hermes plugins search aliexpress
hermes plugins search shopify
hermes plugins search "product sourcing"
hermes plugins search "supplier mapping"
hermes plugins search "order fulfillment"
hermes plugins search tracking
```

这些命令目前不是上线验证结果。可以先使用 Hermes 官方 `filter_entries`
对候选描述做本地检索验证，上架后再检查真实 Catalog。

## 后续曝光动作

1. 提交 Hermes 官方 Catalog：这是 Hermes 目录内被搜索到的前提。
2. 目录上线后，从 DSers 官网的 Agent/MCP 接入页面和帮助中心链接到
   Hermes 插件安装指南，方便现有商家和外部搜索找到入口。
3. 准备一段真实演示，展示店铺概览、只读订单诊断和物流查询；账号及
   客户信息脱敏。使用发布版本实际具备的能力。
4. 由团队在官方社区、DSers 自有渠道和合适的 Shopify/AliExpress 社群
   发布有明确使用场景的介绍。仓库元信息优化不会自动完成这些推广。
5. 如以后增加工作流 skills，再把具体任务、适用范围和示例补充到插件页，
   不把尚未发布的功能当成已经可用。

依据：
[Hermes Catalog 搜索实现](https://github.com/NousResearch/hermes-agent/blob/v0.22.5/hermes_cli/plugin_catalog.py)、
[官方上架流程](https://hermes-agent.nousresearch.com/docs/developer-guide/plugins/catalog-submission)、
[DSers 产品定位](https://www.dsers.com/)、
[DSers 已公开的服务能力](https://www.dsers.com/en/mcp-server)。
