---
name: viagra
description: Use immediately after running a git commit in a project (git 提交后自动执行一次提交后自查), when entering a project that lacks AGENTS.md or .agents/ memory, when the user asks to initialize/organize/maintain project agent memory (初始化agent工作区、整理.agents、项目记忆、记录决策、写决策记录、更新AGENTS.md、沉淀经验), when the user refers to this skill by its name viagra (伟哥), when a durable technical decision with trade-offs is made during coding, when the same question or pitfall comes up a second time, or when a workflow is executed for the second time — to bootstrap and grow the project-local .agents system so any future AI session can understand the project cheaply.
---

# viagra — 伟哥 · 项目记忆园丁

名字由来:针对 AI 的"伟哥"——专治 AI 接手项目时的"疲软"(看不懂项目、忘记决策、重复踩坑),让它在任何项目里快速起势、持续硬朗。

目标:让任何项目的 AI 接手成本趋近于零。.agents 记忆系统**不是一次性生成的,是在开发中按规则长出来的**;你的职责是园丁——按规则种植、修剪,而不是搬运别人的花园。

## 何时用 · 怎么用(时机 → 动作)

| 时机 | 怎么用 |
|---|---|
| **git commit 之后**(自动触发,无需用户提醒) | 执行操作二:按判断表自查本次提交;命中才写,全部未命中则静默继续,不输出冗长确认 |
| 首次进入没有 .agents 系统的项目,或用户要求初始化 | 执行操作一:侦查项目 → 生成骨架 → 报告并请用户口述补充隐性约定 |
| 用户要求整理记忆/清理 .agents,或根 AGENTS.md 超过 100 行 | 执行操作三:去重 → 归档 → 删除 → 瘦身 → 查断链 |
| 开发中拍板了一个有取舍的技术决策 | 立即按模板 B 写决策 note,与本次改动放进同一次提交 |
| 同一个坑第二次出现 / 同类问题第二次被问 | 向根 AGENTS.md 追加 1~3 行祈使句规则 + 理由链接 |
| 同一套流程第二次执行 | 执行操作四:准入三问通过后,按创作规则沉淀为 `.agents/skills/` 工作流(模板 C) |

## 核心模型:三层知识,按"加载时机"分层

| 层 | 位置 | 加载时机 | 装什么 | 铁律 |
|---|---|---|---|---|
| 宪法 | 项目根 `AGENTS.md` | 每次会话必在上下文 | 常设规则,每条 1~3 行 + 理由链接 | < 100 行,超了必须瘦身 |
| 决策 | `.agents/notes/` | AI 按需读取 | 为什么这么做、放弃了什么 | 一个事实只有一个家 |
| 流程 | `.agents/skills/` | 描述匹配场景时触发 | 精确到命令的工作流 | 只装步骤,不装事实 |

六条原则:

1. **One home per fact**:每个事实只写一处,其他地方只放链接。重复 = 漂移 = AI 读到矛盾信息。
2. **规则配理由**:AGENTS.md 里每条规则后面跟一个指向决策记录的链接,不服可以追溯论证。
3. **记录输家**:决策记录强制包含 `Alternatives considered`(每个备选方案 + 为什么输)。没有输家的决策会引来重新争论。
4. **同步落地**:决策记录与产生它的代码在同一次提交里完成,不事后补。
5. **有生有死**:过期内容归档或删除。文档系统的敌人不是"没写",是"只进不出"。
6. **相对链接**:跨文件引用一律用相对 markdown 链接,机械可查,移动文件后可修复。

**绝不照搬其他项目的 .agents 内容**——每次都根据本项目的实际侦查结果推断生成,内容必须是本项目独有的。

## 操作一:初始化(项目没有这套系统时)

1. 侦查现状:读 package.json / 构建脚本 / 目录结构 / 现有文档 / git log,推断项目类型、技术栈、常用命令、已形成的隐性约定。
2. 按下方模板创建:
   - 项目根 `AGENTS.md`
   - `.agents/notes/README.md`
   - `.agents/notes/{proposed,implemented,rejected,archived}/` 四个目录
   - `.agents/skills/` 不预建:小A 由操作四在流程真正重复时才生长
3. 初始 AGENTS.md 只写:①项目 3 句话简介 + 架构文档链接;②目录结构注释(只列 AI 需要导航的);③常用命令;④当前最重要的 3~5 条约定。**宁少勿多,后面会长出来**。
4. 向用户报告创建了什么,邀请用户口述补充隐性约定(很多约定只在用户脑子里)。

## 操作二:git 提交后 / 任务完成时自查(判断表)

**每次 git commit 之后自动过一遍这张表**(无需用户提醒),完成较大开发任务时同理。命中才写,全部未命中则直接继续,不输出冗长确认:

| 发生了什么 | 写什么 | 写到哪 |
|---|---|---|
| 拍板了一个有取舍的技术决策(选库、定架构、定协议、定目录) | 决策 note | `notes/implemented/{class}/yyyy-mm-dd-主题.md` |
| 大改动动手前需要论证方案 | 提案 note | `notes/proposed/...` |
| 方案讨论后被否决 | 原提案冻结 + Status 行加一行否决理由 | `notes/rejected/...` |
| 同一个坑踩了第二次 / 同类问题被问了第二遍 | 1~3 行祈使句规则 + 理由链接 | 根 AGENTS.md(或子目录 AGENTS.md) |
| 一套流程执行了第二遍(发布、排障、部署、review) | 工作流 skill | `.agents/skills/{kebab-case-name}/SKILL.md` |
| 代码挪了 / 改名 / 改默认值 | 同一提交里更新受影响 note 的事实部分 | 相应 note |
| 某条规则被证明错了 | 写新 note 反转 + 新旧互相链接,旧规则指向新 note | notes + AGENTS.md |
| note 只剩历史价值 | 归档或删除,修复所有入链 | `notes/archived/` 或删除 |

**明确不写**(防过度文档化):机械小改、局部 UI 调整、代码本身已自解释的实现细节、还没发生的计划。

类别(class)默认三选一,放不进就用 `misc`:`architecture`(源码结构)/ `process`(流程工具)/ `testing`。项目变大后可扩充,但保持封闭集合并更新 notes/README.md。

## 操作三:定期整理(用户说"整理记忆/清理.agents"时,或 AGENTS.md 超行时)

1. 重复事实:同一事实出现在两处 → 合并到该层的家,另一处改链接。
2. implemented 里只剩历史价值的 note → 移入 `archived/`(文件头加一行 `Archived: yyyy-mm-dd`),之后永不修改。
3. rejected 里已不能再防止任何错误的 note → 整个删除。
4. 根 AGENTS.md > 100 行 → 依次尝试:挪到子目录 AGENTS.md / 挪到 note / 压缩措辞。
5. 检查所有相对链接未断。

## 操作四:沉淀专属项目 skill(小A)

> 小A 与 A 的分工:A(本 skill)教 AI 维护项目记忆;小A(项目 `.agents/skills/` 下的 SKILL.md)装**本项目特有的工作流**。skill 只装步骤和判断,不装事实和为什么——后者归 notes(一个事实只有一个家)。

**准入三问**(任何一问不过就不建,宁可等下一次重复):

1. 这套流程已经**原样跑过第二遍**了吗?第一遍不沉淀,第二遍仍命中才证明可复用。
2. 它是**步骤/判断**而不是事实/决策吗?"本项目用 pnpm"是事实(→ AGENTS.md 规则或 note);"发版要按 1-2-3 走"是流程(→ 小A)。
3. 触发场景能一句话说清吗?说不清的 skill 永远不会被触发,不如不建。

**创作规则**(反推自 deepseek-harness 的产物):

1. **description 是唯一的触发面**:写 `Use when <具体场景/任务/会出现的关键词>, to <达成什么>`;把用户和 AI 未来会说的词(中英文都放)写进去。写完自问:新会话扫到这行字,遇到场景 X 会想起它吗?
2. **SKILL.md 保持薄**(一屏读完):只装步骤 + 判断分支 + 禁止项;深层细节放 `references/*.md`,确定性操作放 `scripts/`,正文用相对链接指路。
3. **精确到命令**:每步给可直接执行的命令,不写"运行测试"这类模糊话。
4. **显式写禁止**:把不希望 AI 做的事写进"禁止"节,比指望它自觉有效。
5. **内容必须来自已发生的现实**:从真实踩过的坑、真实执行过的流程提炼;禁止发明"理论上应该"的规则——checklist 膨胀是 skill 腐烂的主要方式。
6. 命名 kebab-case,见名知用途(如 `release-checklist`、`debug-flaky-test`)。

**验收**:建完做两个自测——①新会话视角:只读 description,遇到触发场景会不会想起它?②执行视角:不问任何人,照步骤能走通吗?任一不过就改。三周内从未被触发的小A:重写 description 或删除。

## 模板

### A. 项目根 AGENTS.md

```markdown
# AGENTS.md

<项目名>是<一句话:做什么、给谁用>。改核心模块前先读 <架构文档链接>。

## 项目结构

<代码树注释,10 行以内,只列 AI 需要导航的目录>

## 常用命令

<build / dev / test / lint,每条一行带注释>

## 约定

- <规则,祈使句,1~3 行> (<为什么>[.agents/notes/implemented/…/xxx.md])
- <规则> (<为什么>[...])

## Agent Notes

为持久的决策理由写 note:代码和文档解释不了的"为什么"和"放弃了什么",与产生它的代码同一次提交落地。**每次 git 提交后,按 [.agents/notes/README.md](.agents/notes/README.md) 的判断表自查本次提交**,命中才写,未命中静默继续。格式与生命周期见 [.agents/notes/README.md](.agents/notes/README.md)。
```

### B. 决策记录 `notes/implemented/{class}/yyyy-mm-dd-主题.md`

```markdown
# Agent Note: <标题>

Status: implemented

## Problem

<当时的矛盾/约束,写到不看答案也能读懂>

## Decision

<已落地的事实,现在时,链接关键代码路径>

## Alternatives considered

**<方案A>。** <为什么输>
**<方案B>。** <为什么输>

## Consequences

<这个取舍花了什么代价、买到了什么>
```

proposed 版骨架:`Problem / Proposal / Alternatives considered / Acceptance criteria / Risks`。

### C. 工作流 `.agents/skills/{name}/SKILL.md`

```markdown
---
name: release-checklist
description: Use when preparing to release or publish a version, or the user says 发版/发布/上线, to run this project's exact release steps without missing a gate
---

# 发版检查清单

<一句话:适用范围和唯一目标;顺带点名不适用的相邻场景,防止误触发>

## 步骤

1. <精确到命令的操作,可直接复制执行>
2. <判断分支写全:如果 X 则…,如果 Y 则…,不留"看着办">

## 禁止

- <不希望 AI 做的事,如"不要跳过步骤 2 直接打 tag">
```

注释:`name` 用 kebab-case 见名知意;`description` 是唯一触发面,塞进未来会出现的场景词和用户口头语(中英文都要),不是功能简介;正文只装步骤和判断,不装事实和为什么(归 notes,需要时链接)。完整表格、长清单、背景放 `references/*.md`,可脚本化的确定性步骤放 `scripts/`,SKILL.md 本体保持一屏能读完。

### D. `notes/README.md`

```markdown
# Agent Notes

这里只放一种文档:记录影响本项目的决策——"为什么"和"放弃了什么"。

## 路径 = 元数据

notes/{lifecycle}/{class}/yyyy-mm-dd-topic.md

- proposed/ 未落地提案;implemented/ 已落地且随代码更新事实;rejected/ 已否决(能防止一个诱人错误就留,否则删);archived/ 冻结档案,永不再改。
- class: architecture / process / testing。封闭集合,新增需改本文件。
- 日期 = 首次提出日。文件间引用一律相对链接。

## 何时写

只为持久决策理由写;机械性修改豁免;已有 note 拥有该决策就更新它,禁止重复建。
格式骨架与强制章节见本文件所在 skill 的模板 B。
```

## 红线

- 根 AGENTS.md 永远 < 100 行;每条规则带理由链接。
- 决策 note 必须有 `Alternatives considered`。
- 一个事实只写一处。
- 所有跨文件引用用相对链接。
- 内容必须来自对本项目的侦查推断,禁止从别的项目复制。
- 写完自查 AI 味:重复的规则只留一个家;删掉推理过程叙述;少用加粗和"重要";implemented 记录里禁止 should 式规格腔——只写已发生的事实。
