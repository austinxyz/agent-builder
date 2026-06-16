# Agent Builder Skills

一套帮助初学者从零构建 AI Agent 的 skills。基于 S2→S5 课程流程抽象，输出完整的 agent 脚本（System Prompt + 对话剧本 + 知识库文件）。

## 支持平台

| 平台 | 费用 | 调用方式 |
|------|------|---------|
| [Claude Code](https://claude.ai/code) | 付费 | `/agent-builder` |
| [Google Antigravity](https://antigravity.google) | 免费 | `use agent-builder skill` |

## 快速开始

### Claude Code

1. 把 `skills/` 目录复制到项目根目录（或 `~/.claude/skills/`）
2. 对话框输入 `/agent-builder`

### Antigravity

1. 下载 Antigravity，打开此项目目录
2. `.agents/skills/` 已存在，自动发现
3. 对话框输入 `use agent-builder skill`

## Skills 列表

| Skill | 用途 | Claude Code | Antigravity |
|-------|------|-------------|-------------|
| agent-builder | 主入口，显示进度，路由 | `/agent-builder` | `use agent-builder skill` |
| agent-define | 定义阶段：领域/英雄/痛点/WHO/WHEN/WHAT/DONE-WHEN | `/agent-define` | `use agent-define skill` |
| agent-system-prompt | 写 5 件套 System Prompt | `/agent-system-prompt` | `use agent-system-prompt skill` |
| agent-dialogue | 写 N 步对话剧本（含 Fallback/越界/KB引用） | `/agent-dialogue` | `use agent-dialogue skill` |
| agent-polish | 打磨：找失控点，改前/改后，验证 DONE-WHEN | `/agent-polish` | `use agent-polish skill` |
| agent-kb | 建知识库文件（5 种内容类型） | `/agent-kb` | `use agent-kb skill` |

## 输出结构

每个 agent 输出到 `agents/[名称]/`：

```
agents/[agent-name]/
├── agent-script.md          # System Prompt + 对话剧本 + KB引用清单
└── knowledge-base/
    ├── 01_xxx.md
    └── 02_xxx.md
```

## 示例

`examples/量化策略验证官/` — 完整的量化策略验证 agent，展示 skills 产出的最终效果。

## 设计文档

`docs/design.md` — 架构决策、skill 接口规范、验证记录。
