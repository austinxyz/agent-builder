---
name: agent-publish
description: |
  Use when the user wants to publish their agent to WukongDojo.ai, generate platform listing info (name, description, first message), a publish checklist, or a community post draft.
---

# Agent Publish · 发布

把做好的 agent 发布到 WukongDojo.ai，并生成可直接发帖的社区文案。

## 前置：读取文件

读取 `agent-script.md`，提取以下内容：
- 第零部分：WHO / WHEN / WHAT / DONE-WHEN
- 第一部分：5 件套 System Prompt（身份/服务对象/核心任务/铁律/Disclaimer）
- 英雄原型 + 当前版本号
- `knowledge-base/` 目录下的所有文件名

如果文件不存在，提示："请先运行 `use agent-define skill` → `use agent-system-prompt skill` 完成 agent 脚本。"

---

## 第一步：生成平台发布信息

基于读取内容，生成以下字段，展示给用户确认：

### Agent 名称

从英雄原型 + WHAT 提炼，要求：
- 中文，4-8 字
- 说明 agent 是干什么的，不是 agent 叫什么名字
- 示例："皮克球成长教练"、"量化策略验证官"

### 简短描述（100字以内）

格式固定：

```
[agent 帮谁] + [解决什么问题] + [怎么开始第一句话]

示例：
帮打了一段时间但没有进步的皮克球爱好者，从评估水平开始，一步步建立结构化训练路径。
开始方式：直接告诉 agent 你打了多久、最近一场球哪里没打好。
```

要求：
- 第一句话说清楚 WHO + WHAT
- 第二句话必须包含**用户应该怎么开口**（降低启动摩擦）

### 开场第一句（用户视角）

从 WHEN 提炼，给出用户可以直接复制发给 agent 的第一句话：

```
示例："我打皮克球2年了，最近比赛老是输，不知道问题出在哪里。"
```

---

## 第二步：System Prompt 来源确认

> "WukongDojo 的剧本（System Prompt）直接用 `agent-script.md` 第一部分的全部内容。
>
> 复制范围：从 `### ① 身份` 到 `### ⑤ Disclaimer` 全部内容。
> 知识库文件上传 `knowledge-base/` 下的所有 `.md` 文件。"

列出知识库文件清单：

```
待上传文件：
- knowledge-base/01_xxx.md
- knowledge-base/02_xxx.md
（最多30个）
```

---

## 第三步：发布检查清单

引导用户在 WukongDojo 完成以下步骤：

```
登录 wukongdojo.ai → 「我的智能体」→「新建智能体」

□ 智能体名称：[生成的名称]
□ 语言：中文（创建后无法修改，确认后再填）
□ 简短描述：[生成的描述]
□ 剧本（System Prompt）：粘贴 agent-script.md 第一部分全文
□ 知识（Knowledge Base）：上传 knowledge-base/ 下所有 .md 文件
□ 模型：推荐 Nemotron Super 120B（免费起步）
□ 点「保存智能体」
□ 复制分享链接
```

完成后，请把分享链接告诉我，我来生成社区帖子。

---

## 第四步：生成社区帖子

收到分享链接后，生成可直接发帖的文案：

### 帖子结构

```markdown
**标题：** [agent 名称] · 免费试用

**正文：**

[1-2句话：这个 agent 帮谁解决什么问题]

**适合谁：**
- ✅ [服务的人]
- ❌ [不服务的人（简短）]

**怎么开始：**
直接打开链接，说：「[开场第一句]」

**试用链接：** [wukongdojo 分享链接]

**说明：**
- 免费使用（平台按 token 计费，首次有额度）
- 有任何反馈欢迎回帖
```

展示完整草稿，请用户确认或调整后发帖。

---

## 完成后

> "发布完成。分享链接和社区帖子文案已生成。
>
> 下一步：
> - 收到用户反馈后，用 `use agent-polish skill` 继续打磨
> - 版本更新后重新发布，在帖子里回复新链接"
