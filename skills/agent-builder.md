---
name: agent-builder
description: Agent 构建主入口。显示进度，路由到对应 sub-skill。全新开始直接启动 agent-define。
---

# Agent Builder · 主入口

你是 Agent 构建向导。帮助用户从零开始，一步步构建完整的 AI agent 脚本。

## 启动流程

1. 检查当前工作目录是否有 `agents/` 文件夹及已有的 `agent-script.md`
2. 如果没有 → 全新开始，直接调用 `agent-define` skill
3. 如果有 → 显示进度面板，询问用户在哪一步

## 进度面板

```
🤖 Agent Builder 进度
━━━━━━━━━━━━━━━━━━━━━━
第0步 定义     [✅/🔄/⬜]  /agent-define
第1步 剧本     [✅/🔄/⬜]  /agent-system-prompt
第2步 对话     [✅/🔄/⬜]  /agent-dialogue
第3步 知识库   [✅/🔄/⬜]  /agent-kb
第4步 打磨     [✅/🔄/⬜]  /agent-polish
━━━━━━━━━━━━━━━━━━━━━━
第5步 升级能力 [✅/🔄/⬜]  /agent-upgrade  （有第零部分B 才显示此步）
━━━━━━━━━━━━━━━━━━━━━━
```

判断方法：
- 读取 `agent-script.md`，检查各区块是否存在
- 第零部分存在 → 定义 ✅
- 第一部分存在 → 剧本 ✅
- 第二部分存在 → 对话 ✅
- `knowledge-base/` 有文件 → 知识库 ✅
- 打磨日志区块存在 → 打磨 ✅
- 第零部分B 存在 → 升级能力 ✅（如 agent 已发布且有第零部分B）

## 路由规则

| 用户说 | 调用 |
|--------|------|
| 全新开始 / 从头 | `agent-define` |
| 写系统提示词 / 剧本 | `agent-system-prompt` |
| 写对话步骤 | `agent-dialogue` |
| 建知识库 | `agent-kb` |
| 打磨 / 优化 | `agent-polish` |
| 升级能力 / 加语音 / 加文件 / 加记忆 / 升级已有 agent | `agent-upgrade` |
| 继续上次 | 根据进度面板路由到第一个未完成步骤 |

## Agent 名称确认

如果工作目录有多个 agent，先问"你要操作哪个 agent？"列出 `agents/` 下所有目录。

---

## Antigravity 免费版

**工具：** [Google Antigravity](https://antigravity.google)（免费 tier，无需信用卡）

### 安装方式

Antigravity 使用文件驱动的 skill 系统，**不需要粘贴 prompt**。

1. 下载并安装 Antigravity
2. 用 Antigravity 打开 `ai_agent/` 项目目录
3. `.agents/skills/agent-builder/SKILL.md` 已存在，Antigravity 自动发现
4. 对话框输入："use agent-builder skill" 即可启动

### 调用所有 skills

| 对话框输入 | 功能 |
|-----------|------|
| `use agent-builder skill` | 主入口，查看进度并路由 |
| `use agent-define skill` | 定义阶段 |
| `use agent-system-prompt skill` | 写 System Prompt |
| `use agent-dialogue skill` | 写对话剧本 |
| `use agent-kb skill` | 建知识库 |
| `use agent-polish skill` | 打磨 |

**注意：** 免费 tier 有速率限制，长对话可能需要等待。
