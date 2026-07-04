# Clawdbot完整配置指南：从安装到Claude Code中转
> 📖 **纸质书《OpenClaw超级个体实操手册》已上市！** 清华大学出版社出版，在开源教程基础上全面重写+逐条验证。🛒 [京东专属购买链接（¥42，原价¥59.8）](https://item.jd.com/14669463.html)

![](https://upload.maynor1024.live/file/1769478880951_image-20260127095432442.png)

## 目录

- [简介](#简介)
- [系统要求](#系统要求)
- [安装步骤](#安装步骤)
- [配置Claude Code API中转](#配置claude-code-api中转)
- [验证和测试](#验证和测试)
- [常见踩坑点](#常见踩坑点)
- [常见问题FAQ](#常见问题faq)
- [常用命令](#常用命令)

---

# 推荐新的部署教程，[从零开始打造你的AI工作助手：最全面的中文教程，涵盖安装、配置、实战案例和避坑指南！](https://awesome.tryopenclaw.asia)

## 🔗 MaynorAI 高星项目导航

- [awesome-openclaw-tutorial](https://github.com/xianyu110/awesome-openclaw-tutorial)：OpenClaw 中文教程合集，从安装配置到实战案例和常见坑位一站式梳理。
- [clawbot](https://github.com/xianyu110/clawbot)：Clawbot 完整配置指南，帮助开发者把 Claude Code 中转环境快速跑起来。
- [gpt-codex](https://github.com/xianyu110/gpt-codex)：面向国内开发者的 OpenAI Codex 教程网站，覆盖使用入口、配置和实战案例。
- [gemini-nanobanana-pro](https://github.com/xianyu110/gemini-nanobanana-pro)：基于 Gemini 图像模型的 AI 图片生成与编辑 Web 应用，适合快速体验文生图和改图流程。
- [awesome-chatgpt-project](https://github.com/xianyu110/awesome-chatgpt-project)：ChatGPT 注册、项目资源和高效使用技巧整理，帮助新用户快速入门 AI 工具。
- [awesome-claudcode-tutorial](https://github.com/xianyu110/awesome-claudcode-tutorial)：Claude Code 中文教程，从基础使用到团队级开发工作流。
- [awesome-gptimage2](https://github.com/xianyu110/awesome-gptimage2)：GPT Image 2 中文资料站，聚焦模型能力、提示词框架和商业图片生成场景。
- [CodexPlusPlus](https://github.com/xianyu110/CodexPlusPlus)：Codex App 外部增强启动器，通过 DevTools 协议注入增强能力，不修改原始安装文件。

## 简介

Clawdbot是一个开源的本地AI助手，支持通过消息应用（Telegram、WhatsApp、Discord等）或Web UI与AI模型交互。

**核心特点：**
- 🦞 完全开源，数据本地存储
- 💬 支持多种消息平台
- 🌐 提供Web控制面板
- 🔧 可自定义AI模型和API端点

---

## 系统要求

### 必需条件

| 项目 | 要求 |
|------|------|
| 操作系统 | macOS / Linux / Windows |
| Node.js | 22.0.0 或更高版本 |
| 包管理器 | pnpm（推荐）或 npm |

### 可选条件

- Xcode（仅macOS，如需构建原生应用）
- 消息平台账号（Telegram Bot Token、Discord Bot等）

---

## 安装步骤

### 1. 升级Node.js版本

Clawdbot要求Node.js 22+，使用nvm管理版本：

```bash
# 安装Node.js 22
nvm install 22

# 设置为默认版本
nvm use 22
nvm alias default 22

# 验证版本
node --version  # 应显示 v22.x.x
```

### 2. 选择安装方式

**方式一：npm安装（推荐）**

```bash
npm install -g clawdbot
```

**方式二：一键安装脚本**

```bash
curl -fsSL https://clawd.bot/install.sh | bash
```

**方式三：从源码安装**

```bash
# 克隆仓库
git clone https://github.com/clawdbot/clawdbot.git
cd clawdbot

# 安装依赖
pnpm install

# 构建
pnpm build

# 链接CLI
npm link
```

### 3. 初始化配置

安装完成后运行配置向导：

```bash
clawdbot onboard
```

**配置向导流程：**

#### 步骤1：安全确认

```
◇  Security ───────────────────────────────────────────────────────╮
│  Clawdbot agents can run commands, read/write files, and act     │
│  through any tools you enable.                                   │
│  Please read: https://docs.clawd.bot/security                    │
├──────────────────────────────────────────────────────────────────╯

◇  I understand this is powerful and inherently risky. Continue?
│  Yes
```

#### 步骤2：选择AI后端

```
◇  Model/auth provider
│  Anthropic

◆  Anthropic auth method
│  ● Anthropic token (paste setup-token) ← 推荐Claude Max用户
│  ○ Anthropic token (Claude Code CLI)
│  ○ Anthropic API key
```

**认证方式对比：**

| 认证方式 | 适用场景 | 优点 | 缺点 |
|---------|---------|------|------|
| setup-token | Claude Max/Pro订阅用户 | 无需额外付费 | 需要额外步骤生成token |
| Claude Code CLI | 已配置Claude Code的用户 | 自动读取凭证 | 可能找不到凭证文件 |
| API Key | API按量付费用户 | 最直接 | 需要独立付费 |

#### 步骤3：生成setup-token（Claude Max用户）

在另一个终端运行：

```bash
claude setup-token
```

复制生成的token，粘贴到配置向导中：

```
◇  Paste Anthropic setup-token
│  sk-ant-oat01-xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx...

◇  Token name (blank = default)
│  default

◇  Default model
│  Keep current (default: anthropic/claude-opus-4-5)
```

> **提示：** Claude Max订阅用户不需要单独购买API Key，使用setup-token即可复用订阅额度。

#### 步骤4：配置消息平台（可选）

```
◇  Channel status ────────────────────────────╮
│  Telegram: not configured                   │
│  WhatsApp: not configured                   │
│  Discord: not configured                    │
│  ...共支持 12+ 平台                          │
├─────────────────────────────────────────────╯

◇  Select channel (QuickStart)
│  Telegram (Bot API)
```

**获取Telegram Bot Token：**

1. 在Telegram中搜索 @BotFather
2. 发送 `/newbot`
3. 按提示输入Bot名称和用户名
4. 复制Bot Token

#### 步骤5：完成配置

```
◇  Telegram: ok (@YourBotName) (1416ms)
   Agents: main (default)
   Heartbeat interval: 1h (main)

◇  Control UI ─────────────────────────────────────────────────────╮
│  Web UI: http://127.0.0.1:18789/                                 │
│  Web UI (with token): http://127.0.0.1:18789/?token=your-token   │
│  Gateway WS: ws://127.0.0.1:18789                                │
├──────────────────────────────────────────────────────────────────╯

└  Onboarding complete.
```

#### 步骤6：配对验证（如使用Telegram）

去Telegram给你的Bot发消息，会收到配对码：

```
Clawdbot: access not configured.

Your Telegram user id: 1234567890
Pairing code: ABC12345

Ask the bot owner to approve with:
clawdbot pairing approve telegram <code>
```

在终端批准配对：

```bash
clawdbot pairing approve telegram ABC12345
```

---

## 配置自定义中转站（多模型支持）

### 方案一：配置多模型中转站（推荐）

这个方案支持同时配置 GPT、Claude、Gemini 等多个模型。

#### 1. 获取API凭证

从中转服务获取：
- **API Base URL**: `https://apipro.maynor1024.live`
- **API Key**: `sk-xxxxxxxxxxxxx`

**推荐服务：**
- 推荐中转API：https://apipro.maynor1024.live/
- 购买链接：https://maynorai.tqfk.xyz/item/7

#### 2. 修改主配置文件

> **⚠️ 重要提示：** Clawdbot不支持通过环境变量`ANTHROPIC_BASE_URL`来设置自定义API端点。必须通过配置文件的`models.providers`来配置。

**步骤1：备份配置文件**

```bash
cp ~/.clawdbot/clawdbot.json ~/.clawdbot/clawdbot.json.bak
```

**步骤2：编辑配置文件**

```bash
nano ~/.clawdbot/clawdbot.json
```

在配置文件中添加多个 provider：

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "api-proxy-gpt": {
        "baseUrl": "https://apipro.maynor1024.live/v1",
        "api": "openai-completions",
        "apiKey": "sk-你的API密钥",
        "models": [
          {
            "id": "gpt-4o",
            "name": "GPT-4o",
            "reasoning": false,
            "input": ["text"],
            "cost": {
              "input": 0,
              "output": 0,
              "cacheRead": 0,
              "cacheWrite": 0
            },
            "contextWindow": 128000,
            "maxTokens": 8192
          }
        ]
      },
      "api-proxy-claude": {
        "baseUrl": "https://apipro.maynor1024.live",
        "api": "anthropic-messages",
        "apiKey": "sk-你的API密钥",
        "models": [
          {
            "id": "claude-sonnet-4-5-20250929",
            "name": "Claude Sonnet 4.5",
            "reasoning": false,
            "input": ["text"],
            "cost": {
              "input": 0,
              "output": 0,
              "cacheRead": 0,
              "cacheWrite": 0
            },
            "contextWindow": 200000,
            "maxTokens": 8192
          }
        ]
      },
      "api-proxy-google": {
        "baseUrl": "https://apipro.maynor1024.live/v1beta",
        "api": "google-generative-ai",
        "apiKey": "sk-你的API密钥",
        "models": [
          {
            "id": "gemini-3-pro-preview",
            "name": "Gemini 3 Pro",
            "reasoning": false,
            "input": ["text"],
            "cost": {
              "input": 0,
              "output": 0,
              "cacheRead": 0,
              "cacheWrite": 0
            },
            "contextWindow": 2000000,
            "maxTokens": 8192
          }
        ]
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "api-proxy-claude/claude-sonnet-4-5-20250929"
      },
      "models": {
        "api-proxy-gpt/gpt-4o": {
          "alias": "GPT-4o"
        },
        "api-proxy-claude/claude-sonnet-4-5-20250929": {
          "alias": "Claude Sonnet 4.5"
        },
        "api-proxy-google/gemini-3-pro-preview": {
          "alias": "Gemini 3 Pro"
        }
      }
    }
  },
  "auth": {
    "profiles": {
      "api-proxy-gpt:default": {
        "provider": "api-proxy-gpt",
        "mode": "api_key"
      },
      "api-proxy-claude:default": {
        "provider": "api-proxy-claude",
        "mode": "api_key"
      },
      "api-proxy-google:default": {
        "provider": "api-proxy-google",
        "mode": "api_key"
      }
    }
  }
}
```

**关键配置说明：**

| 字段 | 说明 | 必需 |
|------|------|------|
| baseUrl | 自定义API端点 | ✅ |
| apiKey | 你的API密钥 | ✅ |
| api | API类型（openai-completions/anthropic-messages/google-generative-ai） | ✅ |
| models | 模型列表，必须包含此字段 | ✅ |

**步骤3：配置鉴权文件**

编辑 `~/.clawdbot/agents/main/agent/auth-profiles.json`：

```bash
nano ~/.clawdbot/agents/main/agent/auth-profiles.json
```

添加以下内容：

```json
{
  "version": 1,
  "profiles": {
    "api-proxy-gpt:default": {
      "type": "api_key",
      "provider": "api-proxy-gpt",
      "key": "sk-你的API密钥"
    },
    "api-proxy-claude:default": {
      "type": "api_key",
      "provider": "api-proxy-claude",
      "key": "sk-你的API密钥"
    },
    "api-proxy-google:default": {
      "type": "api_key",
      "provider": "api-proxy-google",
      "key": "sk-你的API密钥"
    }
  },
  "lastGood": {
    "api-proxy-gpt": "api-proxy-gpt:default",
    "api-proxy-claude": "api-proxy-claude:default",
    "api-proxy-google": "api-proxy-google:default"
  }
}
```

---

### 方案二：配置 Claude Code 中转（需要 User-Agent）

如果你使用的是需要 Claude Code User-Agent 验证的中转站：

#### 1. 获取API凭证

- **API Base URL**: `https://code.claude-opus.top/api`
- **API Key**: `cr_xxxxxxxxxxxxx`

#### 2. 配置文件（带 User-Agent）

```json
{
  "models": {
    "providers": {
      "code-claude-opus": {
        "baseUrl": "https://code.claude-opus.top/api",
        "apiKey": "cr_你的API密钥",
        "auth": "api-key",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-opus-4-20250514",
            "name": "Claude Opus 4",
            "reasoning": false,
            "input": ["text"],
            "cost": {
              "input": 0,
              "output": 0,
              "cacheRead": 0,
              "cacheWrite": 0
            },
            "contextWindow": 200000,
            "maxTokens": 8192,
            "headers": {
              "User-Agent": "Claude-Code/1.0.0"
            }
          }
        ]
      }
    }
  }
}
```

**关键点：** 在 `models` 数组的每个模型中添加 `headers` 字段来设置自定义 User-Agent。

**完整配置示例：**

```json
{
  "meta": {
    "lastTouchedVersion": "2026.1.25",
    "lastTouchedAt": "2026-01-27T01:05:21.233Z"
  },
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://code.claude-opus.top/api",
        "apiKey": "cr_你的API密钥",
        "api": "anthropic-messages",
        "models": []
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "anthropic/claude-sonnet-4-5"
      },
      "workspace": "/Users/你的用户名/clawd",
      "maxConcurrent": 4
    }
  },
  "gateway": {
    "port": 18789,
    "mode": "local",
    "bind": "loopback",
    "auth": {
      "mode": "token",
      "token": "你的gateway_token"
    }
  },
  "channels": {
    "telegram": {
      "enabled": false
    }
  }
}
```

#### 步骤4：验证配置格式

```bash
# 使用jq验证JSON格式
cat ~/.clawdbot/clawdbot.json | jq '.models'

# 验证鉴权配置
cat ~/.clawdbot/agents/main/agent/auth-profiles.json | jq '.'
```

### 3. 重启Gateway服务

```bash
clawdbot gateway restart
```

### 4. 验证配置生效

```bash
# 检查Gateway状态
clawdbot channels status

# 应该显示：
# Gateway reachable.
```

---

## 验证和测试

### 1. 检查Gateway状态

```bash
clawdbot channels status
```

**正常输出：**
```
Gateway reachable.
- Telegram default: disabled, configured, stopped
```

### 2. 访问Web UI

打开浏览器访问：

```
http://127.0.0.1:18789/?token=你的token
```

**Web UI功能：**
- 💬 Chat: 直接与AI对话
- 📊 Overview: 查看系统状态
- 🔌 Channels: 管理消息通道
- ⚙️ Config: 修改配置

### 3. 发送测试消息

在Web UI的Chat界面：

1. 输入消息：`Hello, can you hear me?`
2. 点击Send按钮
3. 等待AI回复

**预期结果：**
- 状态显示"Health OK"
- 收到AI的回复消息
- 右上角显示token使用情况

### 4. 查看日志

如果遇到问题，检查日志：

```bash
# Gateway主日志
tail -f ~/.clawdbot/logs/gateway.log

# 错误日志
tail -f ~/.clawdbot/logs/gateway.err.log

# 详细调试日志
tail -f /tmp/clawdbot/clawdbot-$(date +%Y-%m-%d).log
```

---

## 常见踩坑点

### ❌ 踩坑1：环境变量配置无效

**错误做法：**
```bash
# 在LaunchAgent中设置环境变量（无效！）
<key>ANTHROPIC_BASE_URL</key>
<string>https://code.claude-opus.top/api</string>
```

**问题原因：** Clawdbot不支持通过`ANTHROPIC_BASE_URL`环境变量来设置自定义API端点。

**✅ 正确做法：** 在`~/.clawdbot/clawdbot.json`配置文件中添加：
```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://code.claude-opus.top/api",
        "apiKey": "cr_你的密钥",
        "api": "anthropic-messages",
        "models": []
      }
    }
  }
}
```

### ❌ 踩坑2：缺少models字段

**错误配置：**
```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://code.claude-opus.top/api",
        "apiKey": "cr_xxx",
        "api": "anthropic-messages"
        // 缺少models字段！
      }
    }
  }
}
```

**错误信息：**
```
Invalid config at ~/.clawdbot/clawdbot.json:
- models.providers.anthropic.models: Invalid input: expected array
```

**✅ 正确做法：** 必须包含`models`字段，即使是空数组：
```json
{
  "models": {
    "providers": {
      "anthropic": {
        "baseUrl": "https://code.claude-opus.top/api",
        "apiKey": "cr_xxx",
        "api": "anthropic-messages",
        "models": []  // 必须有这一行！
      }
    }
  }
}
```

### ❌ 踩坑3：Telegram连接失败导致Gateway不稳定

**症状：**
- Gateway不断重启
- 日志显示`TypeError: fetch failed`
- Web UI无法连接

**✅ 解决方案：** 临时禁用Telegram：
```bash
clawdbot config set channels.telegram.enabled false
clawdbot gateway restart
```

### ❌ 踩坑4：Node.js版本过低

**错误信息：**
```
clawdbot requires Node >=22.0.0.
Detected: node 20.19.0
```

**✅ 解决方案：**
```bash
nvm install 22
nvm use 22
nvm alias default 22
node --version  # 应显示 v22.x.x
```

### ❌ 踩坑5：中转API需要特定User-Agent

**症状：** API返回403错误，提示"本服务仅限 Claude Code 官方客户端使用"

**问题原因：** 某些中转站要求特定的User-Agent header。

**✅ 解决方案：** 在模型配置中添加`headers`字段：
```json
{
  "models": {
    "providers": {
      "code-claude-opus": {
        "baseUrl": "https://code.claude-opus.top/api",
        "apiKey": "cr_你的密钥",
        "api": "anthropic-messages",
        "models": [
          {
            "id": "claude-opus-4-20250514",
            "name": "Claude Opus 4",
            "headers": {
              "User-Agent": "Claude-Code/1.0.0"
            },
            "contextWindow": 200000,
            "maxTokens": 8192
          }
        ]
      }
    }
  }
}
```

### ❌ 踩坑6：忘记重启Gateway

**问题：** 修改配置后没有重启Gateway，配置不生效。

**✅ 解决方案：**
```bash
# 每次修改配置后都要重启
clawdbot gateway restart

# 验证配置生效
clawdbot channels status
```

---

## 常见问题FAQ

### Q1: Gateway无法连接

**症状：** Web UI显示"disconnected (1006): no reason"

**解决方案：**
```bash
# 1. 检查Gateway是否运行
ps aux | grep clawdbot-gateway

# 2. 检查端口占用
lsof -i :18789

# 3. 检查配置文件是否有效
cat ~/.clawdbot/clawdbot.json | jq '.'

# 4. 重启Gateway
clawdbot gateway restart

# 5. 查看错误日志
tail -50 ~/.clawdbot/logs/gateway.err.log
```

### Q2: API调用失败

**症状：** 日志显示"TypeError: fetch failed"

**解决方案：**
```bash
# 1. 测试API端点可访问性
curl -s https://code.claude-opus.top/api/v1/messages \
  -H "x-api-key: 你的API密钥" \
  -H "anthropic-version: 2023-06-01" \
  -H "content-type: application/json" \
  -d '{"model":"claude-sonnet-4-5","max_tokens":100,"messages":[{"role":"user","content":"hi"}]}'

# 2. 验证配置文件
cat ~/.clawdbot/clawdbot.json | jq '.models.providers.anthropic'

# 3. 确保包含所有必需字段
# - baseUrl
# - apiKey
# - api: "anthropic-messages"
# - models: []

# 4. 重启Gateway
clawdbot gateway restart
```

### Q3: Assistant不回复消息

**症状：**
- Web UI发送消息后无响应
- 日志显示请求完成但耗时很短（< 1秒）
- 没有错误信息

**解决方案：**
1. 确认使用配置文件方式（见"配置Claude Code API中转"章节）
2. 检查配置文件格式正确
3. 重启Gateway
4. 在Web UI中发送测试消息

### Q4: 如何查看详细日志？

```bash
# Gateway主日志
tail -f ~/.clawdbot/logs/gateway.log

# 错误日志
tail -f ~/.clawdbot/logs/gateway.err.log

# 详细调试日志（JSON格式）
tail -f /tmp/clawdbot/clawdbot-$(date +%Y-%m-%d).log

# 过滤错误信息
tail -f /tmp/clawdbot/clawdbot-$(date +%Y-%m-%d).log | grep -i "error\|fail"
```

### Q5: 如何完全重置配置？

```bash
# 1. 备份当前配置
cp -r ~/.clawdbot ~/.clawdbot.backup

# 2. 停止Gateway
clawdbot gateway stop

# 3. 删除配置
rm -rf ~/.clawdbot

# 4. 重新运行配置向导
clawdbot onboard
```

---

## 常用命令

### Gateway管理

```bash
# 查看状态
clawdbot channels status

# 深度检查
clawdbot channels status --deep

# 重启Gateway
clawdbot gateway restart

# 停止Gateway
launchctl unload ~/Library/LaunchAgents/com.clawdbot.gateway.plist

# 启动Gateway
launchctl bootstrap gui/$(id -u) ~/Library/LaunchAgents/com.clawdbot.gateway.plist
```

### 配置管理

```bash
# 交互式配置
clawdbot configure

# 配置特定部分
clawdbot configure --section gateway
clawdbot configure --section channels
clawdbot configure --section model

# 设置配置项
clawdbot config set gateway.mode local
clawdbot config set channels.telegram.enabled false

# 查看配置
cat ~/.clawdbot/clawdbot.json
```

### 日志查看

```bash
# Gateway主日志
tail -f ~/.clawdbot/logs/gateway.log

# 错误日志
tail -f ~/.clawdbot/logs/gateway.err.log

# 详细日志（JSON格式）
tail -f /tmp/clawdbot/clawdbot-$(date +%Y-%m-%d).log
```

### 诊断工具

```bash
# 运行诊断
clawdbot doctor

# 自动修复问题
clawdbot doctor --fix

# 安全审计
clawdbot security audit --deep
```

### Web UI

```bash
# 打开Web UI（浏览器）
clawdbot dashboard

# 获取带token的URL（不打开浏览器）
clawdbot dashboard --no-open
```

### TUI（终端界面）

```bash
# 启动TUI
clawdbot tui

# TUI快捷键
# Ctrl+C: 退出
# Enter: 发送消息
# Shift+Enter: 换行
```

### 更新

```bash
# 从源码更新
cd /path/to/clawdbot
git pull --rebase
pnpm install
pnpm build
clawdbot gateway restart

# 从npm更新（全局安装）
npm install -g clawdbot@latest
```

---

## 配置文件位置

```
~/.clawdbot/
├── clawdbot.json              # 主配置文件
├── credentials/               # API凭证
├── sessions/                  # 会话数据
├── logs/                      # 日志文件
│   ├── gateway.log           # Gateway主日志
│   └── gateway.err.log       # 错误日志
└── agents/                    # Agent工作空间
    └── main/
        └── sessions/          # 会话存储

~/Library/LaunchAgents/        # macOS服务配置
└── com.clawdbot.gateway.plist

/tmp/clawdbot/                 # 临时日志
└── clawdbot-YYYY-MM-DD.log   # 详细调试日志
```

---

## 安全建议

### 1. 保护API Key

- ❌ 不要将API Key提交到Git
- ✅ 定期轮换API Key
- ✅ 使用配置文件而非环境变量

### 2. Gateway Token

- ❌ 不要分享Web UI的token URL
- ✅ 定期更换Gateway token

### 3. 网络安全

- ✅ Gateway默认只监听localhost
- ✅ 如需远程访问，使用Tailscale或VPN
- ❌ 不要直接暴露到公网

### 4. 数据备份

```bash
# 备份配置和会话
tar -czf clawdbot-backup-$(date +%Y%m%d).tar.gz ~/.clawdbot/
```

---

## 参考资源

- **官方文档**: https://docs.clawd.bot/
- **GitHub仓库**: https://github.com/clawdbot/clawdbot
- **官方网站**: https://clawd.bot/
- **社区讨论**: GitHub Issues

---

## 总结

通过本教程，你应该已经：

✅ 成功安装Clawdbot  
✅ 正确配置Claude Code API中转（通过配置文件）  
✅ 启动Gateway服务  
✅ 通过Web UI与AI对话  
✅ 了解常用命令和故障排除  
✅ 避开了所有常见踩坑点

### 关键要点

1. **API配置方式：** 必须使用配置文件`~/.clawdbot/clawdbot.json`中的`models.providers`
2. **必需字段：** `baseUrl`、`apiKey`、`api`、`models`四个字段缺一不可
3. **配置后重启：** 每次修改配置后必须运行`clawdbot gateway restart`
4. **Telegram问题：** 如遇连接问题，先禁用Telegram使用Web UI
5. **Node.js版本：** 必须22.0.0或更高

### 快速参考

```bash
# 查看状态
clawdbot channels status

# 重启Gateway
clawdbot gateway restart

# 查看日志
tail -f ~/.clawdbot/logs/gateway.log

# 打开Web UI
clawdbot dashboard

# 配置文件位置
~/.clawdbot/clawdbot.json
```

祝使用愉快！🦞

---

**教程版本：** v2.0 (2026-01-27)  
**更新内容：** 修正API配置方式，添加完整踩坑指南  
**适用版本：** Clawdbot 2026.1.25+
