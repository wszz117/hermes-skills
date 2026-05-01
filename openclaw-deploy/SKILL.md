---
name: openclaw-deploy
description: OpenClaw 从零开始在 Linux 服务器上部署的完整指南 — 环境准备、安装、配置模型/飞书、systemd 服务、验证
category: devops
---

# OpenClaw 服务器部署完整指南

## 触发条件
- 需要在新服务器上部署 OpenClaw
- 需要重新安装 OpenClaw（包丢失/损坏）
- 需要迁移 OpenClaw 到新环境

## 环境基线
- **OS**: Ubuntu 22.04 LTS (或类似 Debian 系)
- **Node.js**: v22+ (必须)
- **npm**: v10+
- **用户**: root (或具有 sudo 权限的用户)
- **最低配置**: 2C4G，20GB 磁盘

---

## 第一阶段：环境准备

### 步骤 1：系统更新与依赖安装
```bash
apt-get update
apt-get install -y curl build-essential
```

### 步骤 2：安装 Node.js v22
```bash
# 安装 nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# 加载 nvm
source ~/.bashrc

# 安装 Node.js v22
nvm install 22
nvm use 22

# 验证
node -v   # 应显示 v22.x.x
npm -v    # 应显示 10.x.x
```

> ⚠️ **关键**: OpenClaw 要求 Node.js v22+，旧版本会导致安装或运行失败。

---

## 第二阶段：安装 OpenClaw

### 步骤 3：全局安装
```bash
npm install -g openclaw@latest
```

安装可能需要几分钟（依赖较多，含 Puppeteer/Playwright 等）。

### 步骤 4：验证安装
```bash
openclaw --version
openclaw --help
```

---

## 第三阶段：核心配置

所有配置集中在 `~/.openclaw/openclaw.json`。

### 步骤 5：配置模型提供商

在 `openclaw.json` 中配置 LLM 提供商。以下以 CloseAI (OpenAI 兼容接口) 为例：

```json
{
  "models": {
    "mode": "merge",
    "providers": {
      "closeai": {
        "baseUrl": "https://api.openai-proxy.org/v1",
        "apiKey": "YOUR_API_KEY_HERE",
        "api": "openai-completions",
        "models": [
          {
            "id": "claude-sonnet-4-20250514",
            "name": "Claude Sonnet 4",
            "api": "openai-completions",
            "reasoning": true,
            "input": ["text"],
            "contextWindow": 200000,
            "maxTokens": 16384
          },
          {
            "id": "gemini-2.5-flash",
            "name": "Gemini 2.5 Flash",
            "api": "openai-completions",
            "reasoning": true,
            "input": ["text"],
            "contextWindow": 1000000,
            "maxTokens": 65536
          }
        ]
      }
    }
  }
}
```

**配置要点**：
- `baseUrl`: 你的 API 代理地址
- `apiKey`: 从 LLM 提供商获取
- `models`: 列出所有可用模型，`id` 必须与提供商支持的模型名一致
- `reasoning: true`: 启用推理模式（适用于支持推理的模型）

### 步骤 6：配置 Agent 默认行为

```json
{
  "agents": {
    "defaults": {
      "models": {
        "closeai/claude-sonnet-4-20250514": { "alias": "claude-sonnet-4" },
        "closeai/gemini-2.5-flash": { "alias": "gemini-2.5-flash" }
      },
      "compaction": { "mode": "safeguard" },
      "maxConcurrent": 4,
      "subagents": { "maxConcurrent": 8 }
    }
  }
}
```

### 步骤 7：配置飞书通道

```json
{
  "channels": {
    "feishu": {
      "enabled": true,
      "connectionMode": "websocket",
      "streaming": true,
      "blockStreaming": true,
      "accounts": {
        "main": {
          "appId": "YOUR_FEISHU_APP_ID",
          "appSecret": "YOUR_FEISHU_APP_SECRET",
          "botName": "OpenClaw AI助手",
          "dmPolicy": "pairing"
        },
        "default": {
          "appId": "YOUR_FEISHU_APP_ID",
          "appSecret": "YOUR_FEISHU_APP_SECRET",
          "botName": "OpenClaw AI助手",
          "dmPolicy": "pairing"
        }
      }
    }
  },
  "plugins": {
    "entries": {
      "feishu": { "enabled": true }
    }
  }
}
```

**飞书前置条件**：
1. 在飞书开放平台创建企业自建应用
2. 添加「机器人」能力
3. 开通所需权限（im:message, im:chat 等）
4. 记录 App ID 和 App Secret

### 步骤 8：配置 Gateway

```json
{
  "gateway": {
    "port": 18789,
    "mode": "local",
    "bind": "lan",
    "controlUi": {
      "allowedOrigins": [
        "http://localhost:18789",
        "http://127.0.0.1:18789",
        "http://YOUR_SERVER_IP:18789"
      ]
    },
    "auth": {
      "mode": "token",
      "token": "YOUR_SECURE_TOKEN_HERE"
    }
  }
}
```

**配置要点**：
- `bind: "lan"`: 允许局域网/公网访问（云服务器必须）
- `allowedOrigins`: 添加你的服务器公网 IP
- `auth.token`: 设置一个安全的访问令牌

---

## 第四阶段：服务管理 (systemd)

### 步骤 9：启用用户服务
```bash
# 允许 root 用户运行 systemd user 服务（持久化 session）
loginctl enable-linger root

# 设置运行时目录（关键！）
export XDG_RUNTIME_DIR=/run/user/$(id -u)
```

### 步骤 10：管理服务
```bash
# 启用开机自启
systemctl --user enable openclaw-gateway.service

# 启动
systemctl --user start openclaw-gateway.service

# 查看状态
systemctl --user status openclaw-gateway.service

# 查看实时日志
journalctl --user -u openclaw-gateway.service -f
```

> ⚠️ **关键坑**: 操作 systemd user 服务时必须设置 `XDG_RUNTIME_DIR=/run/user/0`（root 用户），否则会报错。

---

## 第五阶段：功能验证

### 步骤 11：多维度验证

```bash
# 1. 检查服务状态
systemctl --user status openclaw-gateway.service
# 应显示: Active: active (running)

# 2. 检查端口监听
ss -tlnp | grep -E '18789|18790'

# 3. 本地 HTTP 测试
curl -s -o /dev/null -w '%{http_code}\n' http://127.0.0.1:18789/
# 应返回: 200

# 4. 检查健康状态
openclaw health

# 5. 检查模型列表
openclaw models list
# 应列出所有配置的模型

# 6. 测试 LLM API（直接调用）
curl https://api.openai-proxy.org/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_API_KEY_HERE" \
  -d '{"model":"gemini-2.5-flash","messages":[{"role":"user","content":"Say hello in Chinese"}]}'

# 7. 飞书通道验证
# 在飞书中找到机器人并发送消息，应收到回复
```

### 步骤 12：Web 控制台访问
```
http://YOUR_SERVER_IP:18789/chat?session=agent:main:main#token=YOUR_SECURE_TOKEN_HERE
```

---

## 常见问题排查

### 问题 1：安装后 openclaw 命令找不到
```bash
# 检查 npm 全局路径
npm config get prefix
# 确保该路径的 bin 目录在 $PATH 中
echo $PATH
```

### 问题 2：systemd 服务启动失败
```bash
# 查看详细错误
journalctl --user -u openclaw-gateway.service --no-pager -n 50

# 常见原因：
# a) XDG_RUNTIME_DIR 未设置
export XDG_RUNTIME_DIR=/run/user/0

# b) 配置文件 JSON 语法错误
python3 -c "import json; json.load(open('/root/.openclaw/openclaw.json'))"

# c) Node.js 版本过低
node -v  # 必须 >= 22

# d) npm 包丢失
ls /usr/lib/node_modules/openclaw/dist/index.js
# 不存在 → npm install -g openclaw@latest
```

### 问题 3：502 Bad Gateway
参见 `openclaw-upgrade-502-fix` 技能中的排查流程。

核心链路：`Nginx (18789) → Gateway (18790)`，502 = Gateway 未运行。

### 问题 4：飞书机器人无响应
```bash
# 检查飞书通道状态
openclaw health

# 检查日志中的飞书相关错误
grep -i feishu /tmp/openclaw/openclaw-$(date +%Y-%m-%d).log

# 常见原因：
# a) App ID / Secret 错误
# b) 飞书应用未开通机器人能力
# c) 权限不足（需要 im:message 等）
# d) WebSocket 连接被防火墙阻断
```

### 问题 5：bonjour 插件导致崩溃
云服务器上 mDNS 不可用，bonjour 插件会导致进程崩溃。

```bash
# 从配置中移除 bonjour
python3 -c "
import json
with open('/root/.openclaw/openclaw.json') as f:
    config = json.load(f)
if 'plugins' in config and 'installs' in config['plugins']:
    config['plugins']['installs'].pop('bonjour', None)
with open('/root/.openclaw/openclaw.json', 'w') as f:
    json.dump(config, f, indent=2)
"
systemctl --user restart openclaw-gateway.service
```

---

## Skills 扩展

技能文件存放在 `~/.openclaw/skills/`，每个技能是一个独立目录，包含 `SKILL.md`。

```
/root/.openclaw/skills/
├── greeting/
│   └── SKILL.md
└── system_info/
    └── SKILL.md
```

### 创建自定义 Skill 示例
```bash
mkdir -p ~/.openclaw/skills/greeting
cat > ~/.openclaw/skills/greeting/SKILL.md << 'EOF'
# Greeting Skill
- id: greeting
- name: 中文问候
- description: 使用中文进行友好的问候
- model: closeai/gemini-2.5-flash

## Prompt
当用户向你问好时，根据当前时间用一句温暖的中文回复。
EOF

# 重启加载
systemctl --user restart openclaw-gateway.service
```

---

## 安全建议

1. **HTTPS**: 生产环境务必配置 Nginx + SSL 证书，不要 HTTP 裸奔
2. **防火墙**: 只开放必要端口（18789），其他端口用 iptables/ufw 限制
3. **Token 强度**: auth.token 使用至少 32 位随机字符串
4. **定期更新**: 关注 OpenClaw 版本更新，按 `openclaw-upgrade-502-fix` 技能执行升级
5. **备份配置**: 修改 `openclaw.json` 前先备份

---

## 关键文件清单

| 文件 | 用途 |
|------|------|
| `~/.openclaw/openclaw.json` | 核心配置（模型、通道、网关） |
| `~/.config/systemd/user/openclaw-gateway.service` | systemd 服务定义 |
| `/etc/nginx/sites-available/openclaw` | Nginx 反向代理配置 |
| `/tmp/openclaw/openclaw-YYYY-MM-DD.log` | 应用日志 |
| `~/.openclaw/skills/*/SKILL.md` | 自定义技能 |
