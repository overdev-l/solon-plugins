# 快速开始（零命令）

本 Skill 配合 Solon 内置的 **`wecom-cli` 插件** 使用,安装过程不需要你打开终端:

1. Solon → **Settings → Extensions**
2. 找到 **"企业微信 CLI (wecom-cli)"** 卡片,点 **安装**(Solon 会按你的平台自动拉取对应的二进制,约 4 MB)
3. 安装完成后 Solon 会弹出 **"扫码授权"** 按钮,点它会跑一次 `wecom-cli init`,把登录 URL 在你的浏览器里打开
4. 用手机企业微信扫码完成登录

完成后,本 Skill 就能让 Agent 直接操作文档 / 智能表格 / 待办。

## 备用手动安装

如果你所在平台 Solon 打包还没覆盖,或你想自己装 npm 版本:

```bash
npm i -g @wecom/cli
wecom-cli init           # 扫码登录
wecom-cli contact get_userlist '{}'   # 验证通畅
```

## 当前限制（官方）

> ⚠️ WecomTeam/wecom-cli README 明确: **目前仅对 ≤ 10 人的企业开放**。超过规模的组织装上也用不了,需要等企业微信放开。

## Skill 清单

本次一起内置了 6 个 skill,按业务域拆分,Agent 会自动按意图选:

| Skill | 覆盖 |
|-------|------|
| `wecomcli-contact` | 通讯录查询(拿 userid / 按姓名模糊匹配) |
| `wecomcli-doc` | 企业微信文档 + 智能表格(CRUD / 字段 / 子表) |
| `wecomcli-todo` | 待办增删改查 + 分派 + 完成状态 |
| `wecomcli-msg` | 会话列表 / 消息记录 / 文本发送 |
| `wecomcli-schedule` | 日程增删改查 + 闲忙查询 |
| `wecomcli-meeting` | 会议预约 / 成员管理 |

## 沙箱提示

Solon 在 `full-auto` 审批模式下用 macOS Seatbelt 沙箱。`wecom-cli` 读写配置目录(约在 `~/.wecom/` 或 `~/Library/Application Support/wecom-cli/`)首次可能被拦。遇到 "operation not permitted" 就切到 `auto-edit` 或 `suggest` 审批模式跑一次,token 落盘后再切回也不受影响。
