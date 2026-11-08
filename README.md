# NodeSeek 自动签到 - GitHub Actions 版

基于 GitHub Actions 的 NodeSeek 论坛自动化工具，无需服务器，Fork 即用。

本 Fork 用于仅签到并通过 QQ 邮箱接收报告：工作流固定关闭自动评论，签到失败或邮件发送失败时任务返回失败。QQ SMTP 授权码和 NodeSeek Cookie 均只保存在 GitHub Secrets。

## ✨ 功能

- ✅ 自动签到 + 领取奖励（支持"试试手气"和"鸡腿 x 5"智能选择）
- 💬 评论代码保留，但本 Fork 的工作流固定关闭评论
- 👥 **多账号支持**（Cookie 用 `|` 分隔）
- 🎯 **可配置评论区域**
- ⏰ **随机延迟执行**（0-10 分钟，防止固定时间触发）
- 📧 QQ 邮箱报告（可额外配置 Telegram）
- 🔄 失败自动重试 + 智能容错
- 🔐 Cookie 过期检测告警

## 🚀 快速开始

1. Fork 本仓库
2. 在 `Settings → Secrets and variables → Actions` 中添加配置
3. Actions 将每天自动执行两次：
   - **北京时间 00:10**（仅签到）
   - **北京时间 12:20**（仅签到）

首次配置后先手动运行并确认签到结果及邮件收到，再保留定时运行。工作流每次执行会检查最后提交时间，达到 45 天时提交保活文件，以降低长期无活动导致定时任务停用的可能。

## 🍪 如何获取 NodeSeek Cookie

1. 打开浏览器，访问 [NodeSeek](https://www.nodeseek.com) 并登录
2. 按 `F12` 打开开发者工具，切换到 **Network（网络）** 标签
3. 刷新页面，在请求列表中点击任意一个请求
4. 在右侧 **Headers（标头）** 中找到 `Cookie` 字段
5. 复制整个 Cookie 值（一长串文本）

**示例：**
```
session=abc123xyz; token=def456uvw; user_id=12345
```

> ⚠️ Cookie 包含登录凭证，请勿泄露！

## ⚙️ 配置说明

### Secrets（敏感信息）

在 `Settings → Secrets and variables → Actions → Secrets` 中添加：

| 变量名 | 必填 | 说明 |
|--------|------|------|
| `NS_COOKIE` | ✅ | NodeSeek Cookie，多账号用 `\|` 分隔 |
| `NS_RANDOM` | ❌ | `true`(默认): 试试手气 / `false`: 鸡腿 x 5 |
| `NS_COMMENT_URL` | ❌ | 评论区域 URL（默认交易区） |
| `TG_BOT_TOKEN` | ❌ | Telegram Bot Token |
| `TG_CHAT_ID` | ❌ | Telegram Chat ID |
| `SMTP_HOST` | ✅ | QQ 邮箱使用 `smtp.qq.com` |
| `SMTP_PORT` | ✅ | SSL 端口 `465` |
| `SMTP_USER` | ✅ | 发件 QQ 邮箱完整地址 |
| `SMTP_PASS` | ✅ | QQ 邮箱 SMTP 授权码，不是邮箱登录密码 |
| `SMTP_TO` | ✅ | 收件邮箱完整地址 |

### Variables（非敏感配置）

在 `Settings → Secrets and variables → Actions → Variables` 中添加：

| 变量名 | 默认值 | 说明 |
|--------|--------|------|
| `NS_DELAY_MIN` | `0` | 随机延迟最小分钟 |
| `NS_DELAY_MAX` | `10` | 随机延迟最大分钟 |

## 📝 多账号配置示例

```
账号1的完整Cookie|账号2的完整Cookie
```

示例：
```
session=abc123; token=xyz|session=def456; token=uvw
```

## 📱 通知示例

### 单账号
```
NodeSeek 每日简报
━━━━━━━━━━━━━━━
👤 账号: 账号 1
🏆 奖励: 5 🍗
💬 评论: 4 条
━━━━━━━━━━━━━━━
✅ 状态: 已签到
🕒 2026-02-10 00:10:00
```

### 多账号
```
🎯 NodeSeek 多账号任务完成

👤 账号1: 签到✅ | 评论4条
👤 账号2: 签到✅ | 评论3条

⏰ 执行时间: 北京时间 2026-02-10 00:10:00
```

## ❓ 常见问题

**Q: Cookie 多久过期？**  
A: 有效期取决于网站登录策略；失效后需手动更新 `NS_COOKIE`。检测失败会写入 QQ 邮件报告。

**Q: 如何手动运行测试？**  
A: 进入 Actions 页面，选择 workflow，点击 "Run workflow"。

## License

MIT
