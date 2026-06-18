# Agent Builder Skills — 设计文档

**日期：** 2026-06-16  
**状态：** 已实现，已验证  

---

## 背景

基于 AI Agent 课程（S2→S5）的作业流程，抽象出一套可复用的 skills，帮助初学者一步步回答问题，产出完整的 agent 脚本（System Prompt + 对话剧本 + 知识库文件）。

课程流程参考：`AI Agent 作业/量化策略验证官_Agent/`

---

## 架构决策

**方案：瘦主 + 胖子 sub-skills（方案 A）**

- `/agent-builder`：主入口，路由，显示进度
- 5 个独立 sub-skills，可单独调用，不依赖顺序

**适配平台：两个**
- Claude Code（付费）：主力版，`/skill-name` 调用，读写文件
- Google Antigravity（免费）：每个 skill 底部附独立 prompt，粘贴到 Project Instructions 即用。支持 Claude 模型，可读写文件，无需信用卡。

注：ChatGPT + Codex 已于 2026-05-16 合并，重度用户走 Claude Code，免费用户走 Antigravity。

---

## 文件结构

```
ai_agent/
├── skills/
│   ├── agent-builder.md        # 主入口
│   ├── agent-define.md         # 定义阶段
│   ├── agent-system-prompt.md  # 写系统提示词
│   ├── agent-dialogue.md       # 对话剧本
│   ├── agent-polish.md         # 打磨
│   └── agent-kb.md             # 知识库生成
└── docs/superpowers/specs/
    └── 2026-06-16-agent-builder-skills-design.md
```

**学员输出目录（每个 agent）：**
```
[工作目录]/agents/[agent-name]/
├── agent-script.md
└── knowledge-base/
    ├── 01_xxx.md
    └── 02_xxx.md
```

---

## Skill 接口规范

### `/agent-builder`
- 问当前阶段 → 路由到对应 sub-skill
- 全新开始 → 直接启动 `/agent-define`
- 显示 5 步进度（✅已完成 / 🔄进行中 / ⬜未开始）

### `/agent-define`
引导序列：领域 → 英雄 → 3个痛点 → 选种子 → WHO/WHEN/WHAT/DONE-WHEN  
英雄描述完成后，用**三层穿梭**检验质量（现象层/本质层/哲学层），缺层则追问补齐  
输出：`agent-script.md` 第零部分

### `/agent-system-prompt`
读取定义区块 → 引导写 5 件套：身份/服务对象/核心任务/铁律/Disclaimer  
件套①身份：英雄视角部分必须按**三层穿梭**结构写（现象→本质→哲学）  
输出：`agent-script.md` 第一部分

### `/agent-dialogue`
问步骤数 → 每步引导：🎯子目标/✅关闭准则/🗣台词/📌Fallback/〔越界〕/📚KB引用  
输出：`agent-script.md` 第二部分 + KB引用清单

### `/agent-polish`
用户粘贴对话 → 找失控点 → 选一刀 → 改前/改后 → 验证 DONE-WHEN  
输出：`agent-script.md` 打磨日志区块

### `/agent-kb`
扫描 KB 引用清单 → 逐条引导提供原始内容 → 格式化  
输出：`knowledge-base/0N_xxx.md`

---

## `agent-script.md` 输出格式

```markdown
# [Agent 名称]

**版本：** v0.1
**类别：** [A目标驱动闭环型 / B开放探索型 / C情绪陪伴型]
**英雄原型：** [人物名]

---

## 第零部分 · 核心定义

**WHO：** ...
**WHEN：** ...
**WHAT：** ...
**DONE-WHEN：** ...

---

## 第一部分 · System Prompt

### ① 身份
### ② 服务对象
### ③ 核心任务
### ④ 铁律
### ⑤ Disclaimer

---

## 第二部分 · 对话剧本

### Step N · [名称]
🎯 子目标：
✅ 关闭准则：
🗣 标准台词：
📌 Fallback：
〔越界场景〕
📚 KB 引用：

---

## 第三部分 · 知识库引用清单

| Step | 文件 | 用途 |
|------|------|------|

---

## 打磨日志

**v0.1 → v0.2**
打磨点：
改前：
改后：
验证：
```

---

## Antigravity 免费版使用方式

Antigravity 的 skill 系统与 Claude Code 一致：**文件驱动，自动发现**，不需要粘贴 prompt。

### 目录结构

```
ai_agent/
└── .agents/
    └── skills/
        ├── agent-builder/SKILL.md
        ├── agent-define/SKILL.md
        ├── agent-system-prompt/SKILL.md
        ├── agent-dialogue/SKILL.md
        ├── agent-polish/SKILL.md
        └── agent-kb/SKILL.md
```

### SKILL.md 格式

```yaml
---
name: agent-define          # 必须与目录名一致
description: |              # 语义触发词（max 200 chars）
  Use when the user wants to define an AI agent...
---
[Markdown 正文：完整引导指令]
```

Antigravity 通过 `description` 字段的语义匹配自动决定激活哪个 skill。

### 学员安装步骤

1. 下载 [Google Antigravity](https://antigravity.google)（免费，无需信用卡）
2. 打开此项目目录（`ai_agent/`）
3. `.agents/skills/` 目录已存在，Antigravity 自动发现所有 skills
4. Settings → Model → 选 Claude（可选，默认 Gemini 也可用）

### 调用方式

| 目标 | 在对话框输入 |
|------|------------|
| 开始构建 agent | "use agent-builder skill" |
| 定义阶段 | "use agent-define skill" |
| 写 System Prompt | "use agent-system-prompt skill" |
| 写对话剧本 | "use agent-dialogue skill" |
| 建知识库 | "use agent-kb skill" |
| 打磨 | "use agent-polish skill" |

Antigravity 可直接读写文件，输出写入 `agents/[名称]/agent-script.md`，与 Claude Code 版行为一致。

**注意：** 免费 tier 有速率限制，长对话可能需要等待。

---

## 验证记录

**验证 agent：** 量化策略验证官（S2→S5 课程成果）  
**验证日期：** 2026-06-16  
**方法：** 用原 agent 的已知答案 dry-run 走一遍所有 skills，对比 skill 输出与最终脚本等价性

### 结论

| Skill | 覆盖 | Gap |
|-------|------|-----|
| `/agent-define` | ✅ 完整 | 无 |
| `/agent-system-prompt` | ✅ 完整 | Disclaimer 里"英雄名言"没专门引导，靠用户自主加 |
| `/agent-dialogue` | ✅ 已修复 | ~~对话里嵌代码/模板无引导~~ → 已补"对话里嵌工具"章节 |
| `/agent-kb` | ✅ 完整 | 5 种内容类型全覆盖原 agent 的 6 个 KB 文件 |

### 修复记录

**2026-06-16：** `/agent-dialogue` 和 `.agents/skills/agent-dialogue/SKILL.md` 补充"对话里嵌工具（代码/模板）"章节。

背景：原 agent Step 5 嵌了可执行 Python 代码（walk-forward 模板），是专业度核心体现。旧 skill 只引导对话结构，不引导工具嵌入。新章节强制追问"用户需要自己计算吗？如果是，把工具直接递给他"，解决这个 gap。

**2026-06-18：** `/agent-define` 和 `/agent-system-prompt`（含 Antigravity 版）补充**三层穿梭**模型。

背景：学员写英雄 Role 时最常犯的错是只写头衔和鸡汤，缺现象层（真实成就）和本质层（独家思维方式）。三层穿梭（现象→本质→哲学）来自课程 S2 作业指南，评分占 10%。`agent-define` 在问题 2 收到英雄描述后自动做三层检验并追问补齐；`agent-system-prompt` 件套①引导时给出三层结构模板。
