# Agent Builder Skills

一套帮助初学者从零构建 AI Agent 的引导工具。回答 8 个问题，输出完整的 agent 脚本（System Prompt + 对话剧本 + 知识库文件），可直接粘贴到 Claude / ChatGPT 使用。

支持两个平台：

| 平台 | 费用 | 适合谁 |
|------|------|--------|
| [Claude Code](https://claude.ai/code) | 付费订阅 | 已有 Claude Code 的用户 |
| [Google Antigravity](https://antigravity.google) | 免费 | 想零成本上手的学员 |

---

## 使用方式

### 方式 A：Claude Code（付费）

**第一步：安装 skills**

```bash
# 克隆此仓库到你的工作目录
git clone https://github.com/austinxyz/agent-builder.git
cd agent-builder
```

或者把 `skills/` 目录复制到你已有项目的根目录。

**第二步：开始构建**

在 Claude Code 对话框输入：

```
/agent-builder
```

Claude Code 自动发现 `skills/` 目录下的所有 skills，显示 5 步进度，引导你逐步完成。

---

### 方式 B：Google Antigravity（免费）

**第一步：下载 Antigravity**

前往 [antigravity.google](https://antigravity.google) 下载，无需信用卡。

**第二步：打开此项目**

用 Antigravity 打开克隆下来的 `agent-builder/` 目录。`.agents/skills/` 目录会被自动发现。

**第三步：开始构建**

在对话框输入：

```
use agent-builder skill
```

---

## 构建流程

两个平台的引导流程完全一致，分 5 步：

```
Step 1  /agent-define          定义阶段
        └─ 领域 → 英雄 → 3个痛点 → 种子 → WHO / WHEN / WHAT / DONE-WHEN

Step 2  /agent-system-prompt   写 System Prompt（5 件套）
        └─ ①身份 ②服务对象 ③核心任务 ④铁律 ⑤Disclaimer

Step 3  /agent-dialogue        写对话剧本（N 步）
        └─ 每步：🎯子目标 / ✅关闭准则 / 🗣台词 / 📌Fallback / 〔越界〕/ 📚KB引用

Step 4  /agent-kb              建知识库文件
        └─ 扫描 KB 引用清单 → 逐条引导提供内容 → 写入 knowledge-base/

Step 5  /agent-polish          打磨
        └─ 粘贴真实对话 → 找失控点 → 改前/改后 → 验证 DONE-WHEN
```

每个 step 也可以独立调用，不需要按顺序。

---

## 输出文件

完成后，你的 agent 在 `agents/[名称]/` 目录下：

```
agents/我的agent/
├── agent-script.md          # 直接粘贴到 Claude / ChatGPT 的 System Prompt + 对话剧本
└── knowledge-base/
    ├── 01_数字基准.md
    ├── 02_判断框架.md
    └── 03_案例.md
```

`agent-script.md` 分四部分：

| 部分 | 内容 | 由哪个 skill 生成 |
|------|------|-----------------|
| 第零部分 | WHO / WHEN / WHAT / DONE-WHEN | `/agent-define` |
| 第一部分 | System Prompt（5 件套） | `/agent-system-prompt` |
| 第二部分 | 对话剧本（N 步） | `/agent-dialogue` |
| 第三部分 | 知识库引用清单 | `/agent-dialogue`（自动生成） |
| 打磨日志 | 改前/改后记录 | `/agent-polish` |

---

## 示例

`examples/量化策略验证官/agent-script.md` — 完整示例，展示一个经过 S2→S5 打磨的量化策略验证 agent 最终长什么样。

可以用它对照自己的输出，判断质量。

---

## Skills 快捷参考

| 目标 | Claude Code | Antigravity |
|------|-------------|-------------|
| 主入口（推荐从这里开始） | `/agent-builder` | `use agent-builder skill` |
| 只做定义 | `/agent-define` | `use agent-define skill` |
| 只写 System Prompt | `/agent-system-prompt` | `use agent-system-prompt skill` |
| 只写对话剧本 | `/agent-dialogue` | `use agent-dialogue skill` |
| 打磨已有剧本 | `/agent-polish` | `use agent-polish skill` |
| 建知识库 | `/agent-kb` | `use agent-kb skill` |
