# Hermes Skills

个人自定义 Hermes Agent skills 集合。

## Skills

| Skill | 描述 |
|-------|------|
| [dogfood](./dogfood/) | Web 应用探索式 QA 测试 — 发现 bug、捕获证据、生成结构化报告 |
| [openclaw-deploy](./openclaw-deploy/) | OpenClaw 从零部署完整指南 — 环境准备、安装、配置、systemd 服务 |
| [openclaw-upgrade-502-fix](./openclaw-upgrade-502-fix/) | OpenClaw 升级运维指南 — 502 错误排查、升级 SOP、应急回滚 |

## 使用方法

将对应 skill 目录复制到你的 `~/.hermes/skills/` 下即可使用。

```bash
git clone https://github.com/houshi666/hermes-skills.git
cp -r hermes-skills/dogfood ~/.hermes/skills/
cp -r hermes-skills/openclaw-deploy ~/.hermes/skills/
cp -r hermes-skills/openclaw-upgrade-502-fix ~/.hermes/skills/
```
