---
name: opc-battle-system
description: OPC (One-Person Company) 超级作战系统 — 飞书 CLI + Hermes Agent 自动化工作流。构建 24 小时不断电的个人组织系统：早简报、线索归档、会议转任务、每日复盘。
---

# OPC 超级作战系统

一人公司自动化工作流系统：Hermes Agent 负责判断和编排，飞书 CLI 负责执行动作。

## 核心架构

```
第6层 风控层 — dry_run、缺字段不脑补、二次确认、权限隔离
第5层 节奏层 — 08:30早简报 / 11:30线索 / 18:30同步 / 22:30复盘
第4层 记忆层 — 飞书存运行信息 + 本地存prompt/模板/脚本/SOP
第3层 执行层 — 飞书 CLI：建任务/建文档/发消息/上传文件
第2层 判断层 — Hermes Agent：提取重点/判断优先级/生成动作草案
第1层 信号层 — 消息/日程/任务/纪要/文档/本地文件
```

## 系统位置

- 工作目录：`/root/opc-system/`
- 飞书 CLI：`lark-cli v1.0.19`（已安装并认证）
- 配置文件：`/root/opc-system/config/opc-system.yaml`
- App ID：`cli_a96612368ef89bd5`
- User openId：`ou_08727eef795ce597058c9ac72d64c296`

## 目录结构

```
/root/opc-system/
├── 01-inbox/          # 信号输入
├── 02-morning-brief/  # 早简报输出
├── 03-leads/          # 线索归档
├── 04-meetings/       # 会议记录
├── 05-delivery/       # 交付物
├── 06-review/         # 每日复盘
├── 07-sop/            # 沉淀的 SOP
├── prompts/           # 6 个流程 prompt 模板
├── scripts/           # 飞书命令封装脚本
└── config/            # 系统配置
```

## 飞书 CLI 配置要点

### 安装
```bash
npm install -g @larksuite/cli
npx skills add larksuite/cli -y -g
```

### 认证（关键坑）
1. `lark-cli config init --new` — 初始化配置
2. `lark-cli auth login --recommend` — 用户授权（弹出链接在浏览器打开）
3. `lark-cli auth status` — 验证

### 已知坑（已踩并修复）
1. **Hermes 绑定冲突**：`lark-cli config bind --source hermes` 会生成 `~/.lark-cli/hermes/config.json`，使用不同 appId。需要手动将 `~/.lark-cli/config.json` 中的 user 信息同步到 hermes/config.json。
2. **strictMode 阻止 user 登录**：hermes/config.json 中 `"strictMode": "bot"` 需改为 `"auto"`（通常在第 8 行）。
3. **双身份策略**：bot 身份自动续期，user 身份 7 天有效期（refresh token 可能自动续）。系统设计应假设 user 会过期，降级运行。
4. **授权页面报错 200340**：点击 "Allow Once" 可能报错，但直接修改配置文件同样有效，不必依赖网页授权。
5. **⚠️ lark-cli 命令语法不能靠猜测**：`doc` vs `docs`、`task +list`（不存在）vs `task +get-my-tasks`、`im +send`（不存在）vs `im +messages-send` 等差异极大。**每次新命令必须先 `lark-cli <command> --help` 确认**。
6. **⚠️ 文档创建必须 v2 API**：`docs +create` 默认 v1 已废弃，必须加 `--api-version v2`，且 `--content` 是必填项（不能只有 `--title`）。
7. **⚠️ 文件上传必须相对路径**：`drive +upload --file /tmp/xxx` 会报 "unsafe file path"，必须 `cd` 到文件所在目录后用相对路径。
8. **⚠️ 消息搜索需额外权限**：`im +messages-search` 需要 `search:message` scope，默认授权不包含，需单独 `lark-cli auth login --scope "search:message"`。

### 身份分工
| 身份 | 用途 | 过期影响 |
|------|------|----------|
| bot | 发消息、建文档、建任务、上传文件 | 不会过期 |
| user | 读个人日程、读个人任务、读会议记录 | 过期后早简报/会议转任务降级，其他功能照常 |

### lark-cli 参数传递规则（关键坑 ⚠️）

lark-cli 的参数传递方式因命令类型而异，**不能想当然用统一格式**：

| 命令类型 | 路径参数（如 chat_id） | 示例 |
|----------|----------------------|------|
| `im chat.members *`（子命令） | 必须用 `--params '{"chat_id":"oc_xxx"}'` | `lark-cli im chat.members bots --params '{"chat_id":"oc_xxx"}'` |
| `im +messages-send`（+前缀命令） | 直接用 `--chat-id "oc_xxx"` 标志 | `lark-cli im +messages-send --chat-id "oc_xxx" --text "hello"` |
| `im chat.members create` | chat_id 是路径参数，走 `--params`；成员信息也走 `--params` | 需分两次传 params 或用 stdin |

**规律**：带 `+` 前缀的命令（如 `+messages-send`）支持标准 CLI 标志（`--chat-id`、`--user-id` 等）；不带 `+` 的子命令（如 `chat.members bots`）路径参数必须走 `--params` JSON。

**其他注意**：
- `+messages-send` **不支持** `--format` 标志，输出默认就是 JSON
- `--as bot` / `--as user` 用于指定身份，默认 `bot`
- `--dry-run` 可用于所有命令，先打印请求不执行

### API 连通性（2026-04-26 全面验证 ✅）

| API | 命令 | 状态 | 备注 |
|-----|------|------|------|
| 日程查询 | `lark-cli calendar +agenda --start "2026-04-26" --end "2026-04-27"` | ✅ | 注意：`--start/--end` 直接传日期即可，不需要 ISO 时间戳 |
| 任务查询 | `lark-cli task +get-my-tasks` | ✅ | 无 `task +list` 命令，用 `+get-my-tasks` 或 `+search` |
| 任务创建 | `lark-cli task +create --summary "xxx" --description "xxx"` | ✅ | |
| 文档创建 | `lark-cli docs +create --title "xxx" --content "xxx" --doc-format markdown --api-version v2` | ✅ | **必须 v2 API + --content**，v1 已废弃 |
| 文件上传 | `lark-cli drive +upload --file ./filename` | ✅ | **必须相对路径**，绝对路径报 "unsafe file path" |
| 发消息 | `lark-cli im +messages-send --chat-id "oc_xxx" --as bot --text "xxx"` | ✅ | 通知目标：投资资讯群 `oc_6774a7b2e12e04176acead3c3394d69e` |
| 会议查询 | `lark-cli vc +search --start "2026-04-20" --end "2026-04-26"` | ✅ | |
| 群机器人列表 | `lark-cli im chat.members bots --params '{"chat_id":"oc_xxx"}'` | ✅ | 路径参数必须走 --params JSON |
| 消息搜索 | `lark-cli im +messages-search --query "xxx"` | ❌ | **缺少 `search:message` 权限**，需运行 `lark-cli auth login --scope "search:message"` |

### lark-commands.sh 已验证修正（6 处错误已修复）

脚本 `/root/opc-system/scripts/lark-commands.sh` 初始版本有 6 处命令错误，已全部修正：

| 错误命令 | 正确命令 | 原因 |
|----------|----------|------|
| `lark-cli agenda +search` | `lark-cli calendar +agenda` | 命令路径错误 |
| `lark-cli task +list` | `lark-cli task +get-my-tasks` / `+search` | 无 +list 子命令 |
| `lark-cli im +search` | `lark-cli im +messages-search` | 命令名错误 |
| `lark-cli doc +create` | `lark-cli docs +create --api-version v2 --content "xxx"` | doc→docs，需 v2 + --content |
| `lark-cli im +send` | `lark-cli im +messages-send --text "xxx"` | 命令名错误，--text 直接传 |
| `drive +upload --folder-token` | `drive +upload --parent-token` | 参数名错误 |

**教训**：lark-cli 命令语法不能靠猜测，必须用 `lark-cli <command> --help` 确认。

## 3 个工程原则

1. **先抽取，再执行** — 不让模型看完消息直接建任务，先产出结构化 JSON 提取结果
2. **缺字段就标红，不猜** — owner/due/deliverable 不明确就输出 `missing_xxx`
3. **输出 JSON 化** — 为了稳定映射成命令

## 4 条核心工作流

### 流程 A：早简报 (08:30)
1. 获取日程 → `lark-cli calendar +agenda`
2. 获取未完成任务 → `lark-cli task +get-my-tasks`
3. 获取最近 24 小时消息 → `lark-cli im +chat-search`
4. 交给 `prompts/morning-brief.md` 处理
5. 生成飞书文档 + 发提醒消息

### 流程 B：线索归档 (11:30)
1. 搜索消息中的合作信号
2. 交给 `prompts/lead-intake.md` 判断
3. 生成可跟进对象 + 追问问题
4. 在 `03-leads/` 下落本地记录 + 飞书建任务

### 流程 C：会议转任务 (会后 10 分钟)
1. 搜索会议记录 → `lark-cli vc +search`
2. 拉会议纪要 → `lark-cli vc +notes`
3. 交给 `prompts/meeting-to-tasks.md`
4. 抽决策/待办/风险 → 建任务
5. 注意：vc +search 单次查询跨度不超过 1 个月，默认适合汇总过去 7 天

### 流程 D：每日复盘 (22:30)
1. 读今日任务状态 + 文档更新 + 交付结果
2. 交给 `prompts/daily-review.md`
3. 输出：完成/未闭环/风险/可沉淀模板/明日第一步

## 命令封装示例（已验证修正版）

不要直接拼长命令，封装成脚本调用。以下命令均已通过实际验证：

```bash
# scripts/lark-commands.sh — 已验证版本

Get-LarkAgenda() {
  lark-cli calendar +agenda --start "$(date +%Y-%m-%d)" --end "$(date +%Y-%m-%d)" --format json
}

Get-MyTasks() {
  lark-cli task +get-my-tasks --format json
}

New-LarkDoc() {
  # 注意：必须用 v2 API，--content 是必填项
  lark-cli docs +create --title "$1" --content "${2:-}" --doc-format markdown --api-version v2
}

Send-LarkTextMessage() {
  # 注意：+messages-send 不支持 --format 标志
  lark-cli im +messages-send --chat-id "$1" --as bot --text "$2"
}

Upload-LarkFile() {
  # 注意：必须使用相对路径
  lark-cli drive +upload --file "$1" ${2:+--parent-token "$2"}
}

Search-Meetings() {
  lark-cli vc +search --start "${1:-$(date -d '7 days ago' +%Y-%m-%d)}" --end "${2:-$(date +%Y-%m-%d)}" --format json
}
```

## 7 天 MVP 路径

| Day | 任务 | 状态 |
|-----|------|------|
| 1 | 安装飞书 CLI，跑通认证 | ✅ 完成 |
| 2 | 建好 opc-system/ 目录结构 | ✅ 完成 |
| 3 | 封装飞书命令为脚本函数 | ✅ 完成（6 处错误已修正） |
| 3.5 | **API 全模块连通性验证** | ✅ 完成（8/9 通过，消息搜索缺权限） |
| 4 | 跑通早简报流程 | ⏳ 待做 |
| 5 | 加会议转任务 | ⏳ 待做 |
| 6 | 加线索归档 | ⏳ 待做 |
| 7 | 加每日复盘，开始沉淀 SOP | ⏳ 待做 |

第一周目标：**让上下文不再断电**，不追求全自动。

## 风控规则

- 高风险动作先 `dry_run`
- 所有时间统一绝对时间（ISO 8601，Asia/Shanghai）
- 缺字段不脑补，显式输出 `missing_xxx`
- 重要外发动作必须二次确认
- 不把高权限 bot 直接扔进公共群

## 待配置项

- [x] `main_chat_id` — ✅ 已配置：投资资讯群 `oc_6774a7b2e12e04176acead3c3394d69e`
- [x] bot 入群 — ✅ 「我的飞书 CLI」已加入投资资讯群
- [x] lark-commands.sh 命令修正 — ✅ 6 处错误已全部修复
- [ ] `search:message` 权限 — ❌ 需运行 `lark-cli auth login --scope "search:message"`
- [ ] cronjob 定时触发 — 设置 4 个固定时点的自动执行
- [ ] daily_report_folder_token — 飞书云文档目录 token
- [ ] tasklist_id — 飞书任务列表 ID
- [ ] 清理测试文件 — 飞书云端有测试文档「OPC系统测试文档-可删除」和「opc-test.txt」

## 参考链接

- 飞书 CLI 官方仓库：https://github.com/larksuite/cli
- 官方 standup workflow：https://github.com/larksuite/cli/tree/main/skills/lark-workflow-standup-report
- 官方 meeting summary workflow：https://github.com/larksuite/cli/tree/main/skills/lark-workflow-meeting-summary
