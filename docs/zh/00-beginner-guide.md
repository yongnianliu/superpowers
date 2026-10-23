# Superpowers 新手使用指南（大白话版）

> 看完这篇，你就能装好 Superpowers，并让 AI 编程助手按"靠谱程序员"的流程帮你干活。
> 不需要懂原理，跟着做就行。
>
> 对应版本：7.0.1。想知道和旧版相比变了什么，看[附录 D](./11-appendix.md#d-从-50-到-70-的主要变化)。

---

## 一、它到底是个啥？

**一句话：Superpowers 是给你的 AI 编程助手装的"职业素养包"。**

没装之前，AI 助手经常是这样的：

- 你刚说一句"帮我做个待办清单"，它就噼里啪啦开始写代码，写出来不是你想要的。
- 改了 bug 就说"好了"，其实根本没验证。
- 代码写一大坨，没测试，出问题不知道怎么回事。

装了之后，AI 会像一个有经验的工程师：

1. **先问你想要什么**，别急着动手。
2. **先出方案、给你确认**，再写计划。
3. **按计划一步步做**，每步都检查。
4. **先写测试，再写代码**。
5. **说"修好了"之前，必须真的验证过**。

你不用背任何命令，**这些"好习惯"会自动触发**。

---

## 二、一个生活化的比喻

把 AI 助手想象成一个刚入职的装修工人：

| 没装 Superpowers | 装了 Superpowers |
|---|---|
| 你说"装修一下客厅"，他拿起锤子就砸墙 | 先问你喜欢什么风格、预算多少，画好图给你确认 |
| 干完说"搞定"，你一看墙是歪的 | 每做完一步都验收，最后给你看验收结果 |
| 出问题了他瞎猜原因乱试 | 按"找原因 → 验证 → 修复"的步骤来 |

Superpowers 里的每一个"技能（skill）"，就是一份**工作规范**，AI 到了对应场景会自己翻出来照着做。

---

## 三、安装（选你用的那个工具）

> 先确认：你用的是哪个 AI 编程工具？每个工具装法不一样。
> 如果你同时用多个工具，**每个都要单独装一遍**。

### 最常见的几种

**Claude Code**

在对话框里输入：

```text
/plugin install superpowers@claude-plugins-official
```

**Cursor**

在 Agent 对话里输入：

```text
/add-plugin superpowers
```

或者在插件市场里搜索 `superpowers`，点安装。

**Codex App（图形界面）**

1. 点左侧栏的 `Plugins`
2. 在 Coding 分类里找到 `Superpowers`
3. 点旁边的 `+`，按提示走

**Codex CLI（命令行）**

```text
/plugins
```

然后搜索 `superpowers`，选 `Install Plugin`。

**Gemini CLI**

```bash
gemini extensions install https://github.com/obra/superpowers
```

**GitHub Copilot CLI**

```bash
copilot plugin marketplace add obra/superpowers-marketplace
copilot plugin install superpowers@superpowers-marketplace
```

**Qwen Code**

```bash
qwen extensions install obra/superpowers
```

### 其他工具

目前一共支持 15 个工具。上面没列到的 OpenCode、Pi、Kimi Code、Factory Droid、Devin CLI、Grok Build CLI、Hermes Agent、Muse、Antigravity，安装命令请直接看仓库根目录的 [README.md](../../README.md) 里对应小节，照抄命令即可。

### 装完怎么确认成功了？

**重新开一个新会话**（旧会话不会生效），然后对 AI 说：

```text
Let's make a react todo list
```

（或者中文："我想做一个待办清单网页"）

- 如果 AI **没有直接写代码，而是先问你问题** → 装成功了。
- 如果它一上来就哗哗写代码 → 可能没生效，检查是否重开了会话，或重装一次。

---

## 四、日常怎么用？

### 核心原则：像平常一样说话就行

**你不需要敲什么特殊命令。** 直接告诉 AI 你想干嘛：

- "帮我做一个记账小程序"
- "这个按钮点了没反应，帮我看看"
- "给登录页加个'记住密码'功能"

AI 会自己判断该用哪个"技能"。

### 一个完整的做新功能流程

下面是 Superpowers 带你走的标准流程，一共 7 步。你只需要在关键节点**回答问题、点头同意**：

```text
你提需求
   ↓
① 头脑风暴      AI 反复问你，搞清楚你到底要什么
   ↓
② 建独立工作区   AI 开一个单独的"分支"，不会弄乱你原来的代码
   ↓
③ 写计划        把活拆成一个个小任务，每个任务都写清楚怎么验收
   ↓
④ 按计划执行    AI（或它派出的"小助手"）逐个完成
   ↓
⑤ 先测试再写码   每个小任务都"先写测试→看它失败→写代码→看它通过"
   ↓
⑥ 做完就检查    小助手模式下每个任务都有人复查；最后整个分支再由最强的模型总查一遍
   ↓
⑦ 收尾         AI 先跑一遍测试，再问你：本地合并？推上去提 PR？还是先放着不动？
```

> 想让 AI 直接丢弃这批成果？可以，但得你**明确说**。它不会主动把"丢弃"摆在选项里。

#### 你在每一步要做什么？

| 步骤 | 技能名 | 大白话 | 你要做的 |
|---|---|---|---|
| ① | `brainstorming` | AI 像产品经理一样追问需求，每次只问一个问题；会先看你的项目，能自己查到的不问你 | 认真回答问题；看完设计方案后说"可以"或提修改意见。**你批准之前，它不会动手写代码** |
| ② | `using-git-worktrees` | 开个新房间干活，不弄脏主房间 | 它会先问你要不要开；回答即可 |
| ③ | `writing-plans` | 把大活拆成小步骤，写成清单 | 它会请你**读一下计划**，没问题再说"开始"；还会让你在"小助手模式"和"自己做模式"之间选一个 |
| ④ | `subagent-driven-development` / `executing-plans` | AI 开始干活了 | 等着，偶尔回答它的疑问 |
| ⑤ | `test-driven-development` | 先写验收标准，再写代码 | 不用管 |
| ⑥ | `requesting-code-review` | 做完一块就复查一遍 | 不用管 |
| ⑦ | `finishing-a-development-branch` | 收工，问你怎么处理成果 | 从选项里选一个 |

> 正常情况下，你说一句"开始吧"之后，AI 可以**自己连续干一两个小时**不跑偏。

---

## 五、常见场景速查

### 场景 1：我想做个新东西

直接说需求，比如：

```text
我想做一个能记录每天喝水量的网页
```

AI 会先把你的话复述一遍（它理解的是什么、给谁用、为什么），再问你一个最关键的问题。接着一问一答：给谁用？要不要登录？数据存哪？……**别嫌烦，这一步越认真，后面越省事。**

它会按事情大小调整排场：

- **很明确的小活**（比如改个按钮颜色）：直接做，做完告诉你做了什么。
- **小改动**：在聊天里跟你说定，再写一份很短的设计文档。
- **一个项目**：写完整的设计文档，还会让另一个"小助手"以施工方的眼光先读一遍，把没写清楚的地方汇总起来一次性问你。

如果某些事你有自己的想法（比如"要做成 iOS 应用，不是网页"），一开始就告诉它；你不说，它会自己选，并在文档里标明"这是我选的"。

### 场景 2：出 bug 了

直接描述现象：

```text
点"提交"按钮后页面白屏了
```

AI 会走 `systematic-debugging`（系统化排错），大概分四步：

1. **查根因**：读报错、稳定复现、看最近改了什么
2. **找规律**：找到类似的、正常工作的代码，对比差异
3. **提假设并验证**：一次只猜一个原因，用最小的改动去验证
4. **动手修**：先写一个会失败的测试，再修；如果连续修了好几次都没好，它会停下来怀疑整体设计，而不是继续瞎试

你会发现它不会一上来就"试试这样改"，而是先查证据。

### 场景 3：AI 说"搞定了"，我不放心

放心，`verification-before-completion` 这个技能会要求 AI：**没跑过验证，不许说"完成了"**。它得拿出运行结果、测试通过的证据。

你也可以主动问一句："你验证过了吗？给我看看结果。"

### 场景 4：想让 AI 帮我看看代码质量

```text
帮我审查一下刚才的改动
```

AI 会用 `requesting-code-review`，按严重程度列出问题，严重的问题会卡住后续流程，要先修。

### 场景 5：别人给我的代码审查意见，我不知道怎么处理

把意见贴给 AI，它会用 `receiving-code-review` 去分析：哪些该改、哪些不合理。不会无脑照单全收。

### 场景 6：AI 好像没按 Superpowers 的方式干活 / 行为怪怪的

对 AI 说：

```text
figure out what went wrong with superpowers in this session
```

（意思是"帮我查查这次会话里 superpowers 哪里出问题了"）

它会去翻会话记录，告诉你发生了什么，还能帮你打包一份去掉敏感信息的材料，方便提 bug 反馈。

---

## 六、所有技能一览（想了解可以看，不用背）

**写代码前**
- `brainstorming`：头脑风暴，搞清需求
- `writing-plans`：写实施计划
- `using-git-worktrees`：开独立工作区

**写代码时**
- `test-driven-development`：先测试后代码
- `subagent-driven-development`：派"小助手"逐个任务做，做完每个都复查（最仔细）
- `executing-plans`：在当前会话里一口气做完，最后统一复查（最省钱）
- `dispatching-parallel-agents`：几件互不相干的事，同时派多个小助手并行做

**写代码后**
- `requesting-code-review`：请求代码审查
- `receiving-code-review`：处理审查意见
- `verification-before-completion`：完成前必须验证
- `finishing-a-development-branch`：收尾（本地合并 / 提 PR / 先放着）

**出问题时**
- `systematic-debugging`：系统化排错
- `diagnosing-superpowers`：诊断 Superpowers 自己哪里出问题

**进阶（给想自己扩展的人）**
- `writing-skills`：自己写新技能
- `using-superpowers`：技能系统的"入门说明"，AI 每次开场会自动读

---

## 七、几个"为什么它这么啰嗦"的解释

**Q：为什么 AI 不直接写代码，总要先问我一堆问题？**
A：因为"理解错需求然后写了一大堆没用的代码"是最浪费时间的事。多问几句，能少返工很多次。

**Q：为什么要写测试？我只是想要个能用的东西。**
A：测试就是"自动验收"。有了它，AI 以后改别的地方时，能立刻发现有没有把之前的功能弄坏。

**Q：为什么要开新分支（worktree）？**
A：相当于"复印一份来改"。改坏了直接丢掉，你原来的代码毫发无损。

**Q：这些流程能跳过吗？**
A：Superpowers 的设计是**强制流程，不是建议**。但小事（比如改个错别字）它会自动简化。如果你真想跳过某步，可以明确告诉 AI，比如"这个很小，不用写计划了，直接改"。

---

## 八、常见问题排查

| 现象 | 可能原因 | 怎么办 |
|---|---|---|
| 装完了，AI 还是直接写代码 | 没有新开会话 | 退出，**开一个全新会话**再试 |
| 技能时灵时不灵 | 会话太长，被压缩后丢了开场说明（Hermes 里尤其明显） | 开新会话 |
| Muse 里技能不触发 | 插件还没"批准" | 运行 `muse plugins approve superpowers`，然后重开会话 |
| 想更新到新版本 | 各工具更新方式不同 | 多数是自动更新；也可用对应命令，如 `gemini extensions update superpowers`、`qwen extensions update superpowers` |
| AI 做事跑偏、重复劳动、特别费 token | 流程没跟好 | 让它执行"figure out what went wrong with superpowers in this session" |

---

## 九、隐私说明（简单版）

- Superpowers 有个**可选**的"可视化辅助"功能（头脑风暴时可能会用到），页面上的 Logo 会从官方网站加载，**仅带版本号**，不含你的项目内容、提示词或使用的工具。
- 不想要的话，设置环境变量 `SUPERPOWERS_DISABLE_TELEMETRY` 为任意"真值"（比如 `1`）即可关闭。

---

## 十、想继续了解？

- 想知道它怎么运作的 → [01 概述与设计哲学](./01-overview.md)、[03 核心工作流](./03-workflow.md)
- 想自己写技能 → [09 技能开发指南](./09-writing-skills.md)
- 遇到问题 / 想交流 → [GitHub Issues](https://github.com/obra/superpowers/issues) 或 [Discord](https://discord.gg/35wsABTejz)

---

## 三句话总结

1. **装上它，重开会话。**
2. **像平常一样说需求，认真回答 AI 的提问。**
3. **看到 AI 在问问题、写计划、跑测试，别急——这就是它在帮你避坑。**
