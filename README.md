# Solon Plugins

**Solon 官方插件仓库** — CLI / MCP Server / Skill Pack 等扩展的注册中心。

Solon 桌面端在启动时从本仓库拉取 [`registry.json`](./registry.json)，用户在「设置 → Extensions」中一键安装。

---

## 目录结构

```
registry.json              # 插件总索引，Solon 启动时拉取
schemas/
  manifest.schema.json     # 单个插件 manifest 的 JSON Schema
plugins/
  <plugin-id>/
    manifest.json          # 插件完整清单（所有版本、资产、权限）
    icon.svg               # 可选，卡片图标
CONTRIBUTING.md            # 贡献新插件指南
```

---

## 插件类型

| type          | 说明                                                 | 状态     |
|---------------|------------------------------------------------------|----------|
| `cli`         | 命令行二进制（如 `rtk`、`lark-cli`）                 | v1       |
| `mcp-server`  | MCP 协议服务                                         | Phase 2  |
| `skill-pack`  | AI Agent Skill 文档集合                              | Phase 2  |
| `theme`       | UI 主题                                              | 未来     |
| `provider`    | AI 模型 / Provider 适配                              | 未来     |

新类型需在 Solon 主仓新增 handler，然后在此仓更新 schema。

---

## 安全模型

- 所有资产下载必须提供 `sha256`，Solon 会强制校验
- Phase 1：仅接受本仓 manifest；用户无法添加第三方 registry
- Phase 2：引入 Ed25519 签名的 `registry.json`

详见 Solon 主仓文档的 Plugin 部分。

---

## 贡献插件

查看 [CONTRIBUTING.md](./CONTRIBUTING.md)。

---

## License

本仓库的 metadata（manifest、schema、文档）采用 MIT。各插件所引用的第三方二进制遵循其自身 License。
