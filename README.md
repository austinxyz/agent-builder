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

## 做出 agent 后，怎么用？

skills 产出的是 `agent-script.md` + `knowledge-base/` 文件。做完之后，把它部署到任意平台：

---

### 部署方式 A：Claude Code（对话式，直接用）

不需要额外操作。在 Claude Code 对话框粘贴 System Prompt 启动：

```
[把 agent-script.md 第一部分的内容粘贴到对话开头]
```

或者用 `/project:` 命令把 `agent-script.md` 设为项目级 System Prompt，每次对话自动加载。

**知识库：** 把 `knowledge-base/` 目录放在项目根目录，Claude Code 自动读取。

---

### 部署方式 B：Antigravity（对话式，直接用）

1. 在 Antigravity 打开你的工作目录
2. 把 `agent-script.md` 第一部分复制到 Antigravity 的 System Prompt 设置
3. `knowledge-base/` 放项目根目录，Antigravity 自动读取

---

### 部署方式 C：悟空 WukongDojo.AI（托管平台，可分享给他人）

[wukongdojo.ai](https://wukongdojo.ai) — 把 agent 部署成一个可分享的链接，别人打开即用，不需要安装任何工具。

**步骤：**

1. 登录 [wukongdojo.ai](https://wukongdojo.ai) → 「我的智能体」→「新建智能体」

2. 填写基本信息：

   | 字段 | 填什么 |
   |------|--------|
   | 智能体名称 | agent 的名字（如：量化策略验证官） |
   | 语言 | 中文（创建后无法修改） |
   | 简短描述 | 一句话说明这个 agent 做什么 |

3. **剧本（System Prompt）：** 把 `agent-script.md` 第一部分全部内容粘贴进去

   ```
   ① 身份
   ② 服务对象
   ③ 核心任务
   ④ 铁律
   ⑤ Disclaimer
   ```

4. **知识（Knowledge Base）：** 上传 `knowledge-base/` 目录下的所有 `.md` 文件（最多 30 个）

5. **模型：** 推荐选 `Nemotron Super 120B (Free)` 免费起步，效果满意后再换付费模型

6. 点「保存智能体」→ 复制分享链接给学员/客户

**注意：** 悟空按 token 计费（In/Out 分开），对话量大时记得充值。

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
