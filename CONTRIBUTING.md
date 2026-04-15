# Contributing a Plugin

本指南描述如何向 `solon-plugins` 提交一个新插件。

---

## 0. 前置：这个插件属于哪种类型？

| type         | 用途                        | v1 支持 |
|--------------|-----------------------------|---------|
| `cli`        | 命令行二进制                | ✅       |
| `mcp-server` | MCP 协议服务                | 🚧 Phase 2 |
| `skill-pack` | Agent Skill 文档集合        | 🚧 Phase 2 |

只有 `cli` 类型 v1 会被 Solon 客户端加载，其他类型的 PR 可以提前准备但不会启用。

---

## 1. 写一份 manifest

在 `plugins/<your-id>/manifest.json` 新建文件。顶部加：

```json
{
  "$schema": "../../schemas/manifest.schema.json",
  ...
}
```

字段完整定义见 [schemas/manifest.schema.json](./schemas/manifest.schema.json)。

### 必需字段

- `schema_version: 1`
- `id` — 全局唯一短 ID，小写字母 + 数字 + 连字符
- `type` — 目前填 `"cli"`
- `name`、`version`、`assets`

### CLI 类型的 `spec` 示例

```json
"spec": {
  "command": "your-cli",
  "bash_rewrite": false
}
```

- `command`：AI 在 bash 工具里调用的命令名
- `bash_rewrite`：是否前缀改写其他命令（仅 RTK 类工具设 true）
- `rewrite_targets`：bash_rewrite=true 时的目标命令白名单，如 `["git", "docker"]`

---

## 2. 计算 SHA-256

每个平台资产都必须提供 `sha256`，否则 Solon 拒绝安装。

```bash
curl -fsSL <your-asset-url> | shasum -a 256
```

---

## 3. 更新 `registry.json`

把插件摘要加到顶层 `plugins` 数组：

```json
{
  "id": "your-id",
  "type": "cli",
  "name": "Your CLI",
  "description": "一句话描述",
  "latest_version": "1.0.0",
  "manifest": "plugins/your-id/manifest.json",
  "author": "Your Name",
  "license": "MIT",
  "homepage": "https://github.com/you/your-cli"
}
```

同时更新顶层的 `updated_at`（ISO 8601 UTC）。

---

## 4. PR

- 一个插件一个 PR
- PR 标题：`add <plugin-id> v<version>`
- PR 描述里贴 `shasum -a 256` 的输出作为证据
- 维护者 review 后合并，合并后 Solon 客户端下次启动即可看到

---

## 5. 更新已有插件

新增版本不要改已有版本的 asset — 只追加新 manifest 字段。发现安全问题请把对应版本的 `revoked: true` 立即提 PR 走快速通道。

---

## 6. 本地验证（可选但推荐）

```bash
npx ajv-cli validate -s schemas/manifest.schema.json -d plugins/<id>/manifest.json
```

---

## 安全红线

- 只接受 HTTPS URL
- 资产必须有 `sha256`
- `permissions` 字段必须诚实声明（虚报会导致插件被吊销）
- 不接受任何「安装时执行任意命令」的请求；`post_install.suggested_command` 仅作为文字建议展示
