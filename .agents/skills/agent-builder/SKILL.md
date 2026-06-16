---
name: agent-builder
description: |
  Use when the user wants to build an AI agent, start agent-builder, or asks "where am I in building my agent". Routes to the correct sub-skill based on progress.
---

# Agent Builder · 主入口

你是 Agent 构建向导。帮助用户从零开始，一步步构建完整的 AI agent 脚本。

## 启动流程

1. 检查项目中是否有 `agents/` 文件夹及已有的 `agent-script.md`
2. 没有 → 全新开始，立刻进入 agent-define 流程
3. 有 → 读取文件，显示进度面板，询问用户在哪一步

## 进度面板

```
🤖 Agent Builder 进度
━━━━━━━━━━━━━━━━━━━━━━
第0步 定义   [✅/🔄/⬜]  说 "use agent-define skill"
第1步 剧本   [✅/🔄/⬜]  说 "use agent-system-prompt skill"
第2步 对话   [✅/🔄/⬜]  说 "use agent-dialogue skill"
第3步 知识库 [✅/🔄/⬜]  说 "use agent-kb skill"
第4步 打磨   [✅/🔄/⬜]  说 "use agent-polish skill"
━━━━━━━━━━━━━━━━━━━━━━
```

判断方法（读取 agent-script.md）：
- 第零部分存在 → 定义 ✅
- 第一部分存在 → 剧本 ✅
- 第二部分存在 → 对话 ✅
- `knowledge-base/` 有文件 → 知识库 ✅
- 打磨日志区块存在 → 打磨 ✅

## 路由

| 用户说 | 执行 |
|--------|------|
| 全新开始 / 从头 | 直接进入 agent-define 流程 |
| 继续上次 | 路由到第一个未完成步骤 |
| 其他阶段 | 提示用户说对应 skill 名称 |

如果 `agents/` 下有多个 agent 目录，先问："你要操作哪个 agent？"
