---
name: viagra
description: Use immediately after running a git commit in a project (git 提交后自动执行一次提交后自查), when entering a project that lacks AGENTS.md or .agents/ memory, when the user asks to initialize/organize/maintain project agent memory (初始化agent工作区、整理.agents、项目记忆、记录决策、写决策记录、更新AGENTS.md、沉淀经验), when the user refers to this skill by its name viagra (伟哥), when a durable technical decision with trade-offs is made during coding, when the same question or pitfall comes up a second time, or when a workflow is executed for the second time — to bootstrap and grow the project-local .agents system so any future AI session can understand the project cheaply.
---

# viagra — 伟哥 · 项目记忆园丁

目标:通过仓库贡献指南、决策记录和可复用工作流,减少后续 AI 会话重新理解项目的成本。根据项目实际工作增量维护记忆。

## 执行边界

- 本 skill 是代理执行的指令,不包含 Git hook 或后台监听器。提交后自查仅在宿主加载本 skill 且代理知道提交已经完成时执行;不保证捕获外部终端的提交。
- 用户仅要求审查或修改本 skill 时,只处理技能文件及相关文档,不顺带在当前仓库初始化记忆系统。其他任务中仅因缺少记忆系统触发时,初始化应服务当前项目工作。
- 先读取并保留已有项目指令;只补缺失内容。不得为了套模板删掉有效规则,不得把推测写成已确认事实。
- `.agents/` 不保证被宿主自动发现。项目工作流应从根指南或 notes 索引提供相对链接,使代理能按需读取;不要宣称所有工具都会自动加载。
- 记忆只记录项目所需信息,不写入密钥、令牌或个人敏感数据。是否提交这些文件沿用仓库约定;本 skill 不自行执行提交、推送或修改 Git 历史。

## 何时用 · 怎么用(时机 → 动作)

| 时机 | 怎么用 |
|---|---|
| 代理完成 **git commit 之后**(本 skill 已加载时主动执行) | 执行操作二:按判断表自查本次提交;命中才写,全部未命中则静默继续,不输出冗长确认 |
| 首次进入没有 .agents 系统的项目,或用户要求初始化 | 执行操作一:侦查项目 → 生成骨架 → 报告并请用户口述补充隐性约定 |
| 用户要求整理记忆/清理 .agents,或根 AGENTS.md 达到 100 行 | 执行操作三:去重 → 归档 → 删除 → 瘦身 → 查断链 |
| 开发中拍板了一个有取舍的技术决策 | 立即按模板 B 写决策 note,与本次改动放进同一次提交 |
| 同一个坑第二次出现 / 同类问题第二次被问 | 向根 AGENTS.md 追加 1~3 行祈使句规则 + 理由链接 |
| 同一套流程第二次执行 | 执行操作四:准入三问通过后,按创作规则沉淀为 `.agents/skills/` 工作流(模板 C) |

## 核心模型:三层知识,按"加载时机"分层

| 层 | 位置 | 加载时机 | 装什么 | 铁律 |
|---|---|---|---|---|
| 宪法 | 项目根 `AGENTS.md` | 宿主支持时自动加载,否则显式读取 | 仓库贡献指南 + 常设规则;有取舍的规则附理由链接 | 建议 200–400 words,< 100 行 |
| 决策 | `.agents/notes/` | AI 按需读取 | 为什么这么做、放弃了什么 | 一个事实只有一个家 |
| 流程 | `.agents/skills/` | 描述匹配场景时触发 | 精确到命令的工作流 | 只装步骤,不装事实 |

六条原则:

1. **One home per fact**:每个事实只写一处,其他地方只放链接。重复 = 漂移 = AI 读到矛盾信息。
2. **规则配理由**:AGENTS.md 中源于技术取舍或重复踩坑的规则附决策记录链接;目录、命令、现有编码规范等可直接核实的基础指南不强制新建 note。
3. **记录输家**:决策记录强制包含 `Alternatives considered`(每个备选方案 + 为什么输)。没有输家的决策会引来重新争论。
4. **同步落地**:尽量在提交前完成对应决策记录;提交后发现遗漏则补写到工作区,说明对应提交,留待正常提交,不自动 amend 或额外 commit。
5. **有生有死**:过期内容归档或删除。文档系统的敌人不是"没写",是"只进不出"。
6. **相对链接**:跨文件引用一律用相对 markdown 链接,机械可查,移动文件后可修复。

**绝不照搬其他项目的 .agents 内容**——每次都根据本项目的实际侦查结果推断生成,内容必须是本项目独有的。

## 操作一:初始化(项目没有这套系统时)

1. 侦查现状:读取已有 `AGENTS.md`、目录结构、项目清单、构建脚本、测试与格式化配置、现有文档及 Git 历史,确认真实的项目结构、可用命令、编码与提交约定。缺少工具或历史时注明无法确认,不得编造。
2. 按下方模板创建或补全:
   - 项目根 `AGENTS.md`:按模板 A 的生成规则编写基础贡献指南;已有文件则保留有效指令并增量整合,不直接覆盖
   - `.agents/notes/README.md`:按模板 D 创建,并写入操作二的判断表及模板 B 的格式要求,使生成文件可独立指导后续自查
   - `.agents/notes/{proposed,implemented,rejected,archived}/` 四个目录
   - `.agents/skills/` 不预建:小A 由操作四在流程真正重复时才生长
3. 检查基础指南符合模板 A:标题、篇幅、适用章节、示例及链接均准确,不保留占位符。后续操作二至四均以这份 `AGENTS.md` 为基础,先读取再增量更新对应章节;保留贡献指南结构与有效内容,不重新生成旧模板或另建 `Agent.md`。
4. 向用户报告创建了什么,邀请用户口述补充隐性约定(很多约定只在用户脑子里)。

## 操作二:git 提交后 / 任务完成时自查(判断表)

**每次 git commit 之后自动过一遍这张表**(无需用户提醒),完成较大开发任务时同理。命中才写,全部未命中则直接继续,不输出冗长确认:

| 发生了什么 | 写什么 | 写到哪 |
|---|---|---|
| 拍板了一个有取舍的技术决策(选库、定架构、定协议、定目录) | 决策 note | `notes/implemented/{class}/yyyy-mm-dd-主题.md` |
| 大改动动手前需要论证方案 | 提案 note | `notes/proposed/...` |
| 方案讨论后被否决 | 原提案移入 rejected,Status 改为 rejected 并附否决理由,修复相对链接 | `notes/rejected/...` |
| 同一个坑踩了第二次 / 同类问题被问了第二遍 | 1~3 行祈使句规则 + 理由链接 | 根 AGENTS.md(或子目录 AGENTS.md) |
| 一套流程执行了第二遍(发布、排障、部署、review) | 工作流 skill | `.agents/skills/{kebab-case-name}/SKILL.md` |
| 代码挪了 / 改名 / 改默认值 | 同一提交里更新受影响 note 的事实部分 | 相应 note |
| 某条规则被证明错了 | 写新 note 反转 + 新旧互相链接,旧规则指向新 note | notes + AGENTS.md |
| note 只剩历史价值 | 归档或删除,修复所有入链 | `notes/archived/` 或删除 |

**明确不写**(防过度文档化):机械小改、局部 UI 调整、代码本身已自解释的实现细节、凭空预测的计划。已实际讨论且需要论证的未落地方案可以写 proposed,不得标成 implemented。检查已有记录后再写;同一提交重复自查不创建重复 note。

类别(class):`architecture`(源码结构)/ `process`(流程工具)/ `testing` / `misc`(其他)。项目变大后可扩充,但保持封闭集合并更新 notes/README.md。表中的 `notes/` 路径均相对项目 `.agents/`。

## 操作三:定期整理(用户说"整理记忆/清理.agents"时,或 AGENTS.md 超行时)

1. 重复事实:同一事实出现在两处 → 合并到该层的家,另一处改链接。
2. implemented 里只剩历史价值的 note → 移入 `archived/`,将 Status 改为 archived 并加 `Archived: yyyy-mm-dd`;冻结决策正文,允许修复相对链接和路径元数据。移动生命周期目录时修复该文件的出链和所有入链。
3. rejected 里已无参考价值的 note → 检查所有引用及 Git 状态;已提交、没有本地修改且无有效引用时可删除,否则保留或归档,不丢失未提交的唯一记录。
4. 根 AGENTS.md >= 100 行 → 压缩措辞,把细节挪到 note 并保留入口;仅把特定子目录的规则挪到子目录 AGENTS.md,全局规则保留在根文件。不得为了行数删除有效指令。
5. 检查所有相对链接未断。

## 操作四:沉淀专属项目 skill(小A)

> 小A 与 A 的分工:A(本 skill)教 AI 维护项目记忆;小A(项目 `.agents/skills/` 下的 SKILL.md)装**本项目特有的工作流**。skill 只装步骤和判断,不装事实和为什么——后者归 notes(一个事实只有一个家)。

**准入三问**(任何一问不过就不建,宁可等下一次重复):

1. 这套流程的核心步骤已经成功执行至少两次,且能从当前会话、记录或用户说明确认吗?参数变化不影响判断;次数不明时不推定重复。
2. 它是**步骤/判断**而不是事实/决策吗?"本项目用 pnpm"是事实(→ AGENTS.md 规则或 note);"发版要按 1-2-3 走"是流程(→ 小A)。
3. 触发场景能一句话说清吗?说不清的 skill 永远不会被触发,不如不建。

**创作规则**:

1. **description 说明触发场景**:写清何时使用及完成什么;保留有助于发现的常用词,不堆砌关键词。宿主也可能按名称或用户显式请求调用。
2. **SKILL.md 保持精简**:只装必要步骤、判断与约束;较长的专项细节放 `references/*.md`,重复执行的确定性操作可放 `scripts/`,正文用相对链接指路。
3. **精确到命令**:每步给可直接执行的命令,不写"运行测试"这类模糊话。
4. **显式写禁止**:把不希望 AI 做的事写进"禁止"节,比指望它自觉有效。
5. **内容必须来自已发生的现实**:从真实踩过的坑、真实执行过的流程提炼;禁止发明"理论上应该"的规则——checklist 膨胀是 skill 腐烂的主要方式。
6. 命名 kebab-case,见名知用途(如 `release-checklist`、`debug-flaky-test`)。

**验收**:建完检查描述能否匹配真实场景、步骤能否用已知项目条件执行,并从根指南或 notes 索引链接新工作流。已有同类 skill 则更新它。只有存在失效或不再需要的证据时才整理,不根据未经记录的使用频率删除。

## 模板

### A. 项目根 AGENTS.md

生成项目根目录的 `AGENTS.md`,作为本仓库的贡献者指南。以下是生成规则,应根据侦查结果写成具体内容,不要把规则原文或占位符直接复制到产物。

**文档要求**:

- 一级标题固定为 `Repository Guidelines`。
- 使用 Markdown 标题组织结构,标题应描述章节内容;说明简短、直接、可执行,与本仓库相关,采用专业的指导语气。
- 建议全文 200–400 words,并保持少于 100 行;不为凑篇幅添加无关内容。
- 按项目实际情况调整下面的大纲:相关章节可增加,不适用章节可省略。
- 在有帮助时提供真实的命令、目录路径、命名模式等示例;不要将 `npm test`、`make build` 等示例当成本项目已有命令。
- 提交规范从 Git 历史归纳,PR 要求参考现有贡献文档和 PR 模板。无法确认时明确说明,不将建议伪装成既有规范。

**推荐章节与内容**:

| 章节 | 应写内容 |
|---|---|
| Project Structure & Module Organization | 源码、测试、资源的实际位置及模块组织方式 |
| Build, Test, and Development Commands | 构建、测试、本地运行的关键命令,简述各命令用途 |
| Coding Style & Naming Conventions | 缩进、语言风格、命名模式及已有格式化或 lint 工具 |
| Testing Guidelines | 测试框架、已有覆盖率要求、测试命名规范及运行方式;无相关配置时如实说明或省略不适用项 |
| Commit & Pull Request Guidelines | Git 历史中的提交信息惯例,以及 PR 描述、关联 issue、截图等实际要求 |

可按需增加 `Security & Configuration Tips`、`Architecture Overview` 或 `Agent-Specific Instructions`。使用本 skill 初始化记忆系统时,在 `Agent-Specific Instructions` 中保留后续记忆维护入口,链接到实际生成的 notes 指南。

下面仅为结构示意,生成时替换占位内容并裁剪不适用章节:

```markdown
# Repository Guidelines

## Project Structure & Module Organization

<本仓库源码、测试、资源等目录的职责>

## Build, Test, and Development Commands

<实际可用的构建、测试、本地开发命令及用途>

## Coding Style & Naming Conventions

<从代码和配置确认的缩进、命名、格式化和 lint 约定>

## Testing Guidelines

<实际测试框架、覆盖率要求、测试命名及运行方式>

## Commit & Pull Request Guidelines

<从 Git 历史和贡献文档确认的提交惯例及 PR 要求>

## Agent-Specific Instructions

为持久决策记录原因和备选方案,尽量随相关代码提交。完成 Git 提交后,按 [记忆维护指南](.agents/notes/README.md) 自查,命中才写,未命中静默继续;遗漏补到工作区,不自动改写提交历史。后续操作先读本文件,再增量维护对应章节。
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

注释:`name` 用 kebab-case;`description` 写清用途和触发场景。正文装步骤和判断,决策理由归 notes。按需把长清单放 `references/`,确定性步骤放 `scripts/`,并添加相对链接。

### D. `notes/README.md`

生成本文件时将操作二的判断表和模板 B 的格式内联到对应章节,不要保留“见本 skill 模板”的外部依赖。已有指南增量更新。

```markdown
# Agent Notes

这里只放一种文档:记录影响本项目的决策——"为什么"和"放弃了什么"。

## 路径 = 元数据

notes/{lifecycle}/{class}/yyyy-mm-dd-topic.md

- proposed/ 未落地提案;implemented/ 已落地且随代码更新事实;rejected/ 已否决;archived/ 冻结决策正文,允许修复链接和路径元数据。
- class: architecture / process / testing / misc。新增类别需改本文件。
- 日期 = 首次提出日。文件间引用一律相对链接。

## 何时写

只为持久决策理由写;机械性修改豁免;已有 note 拥有该决策就更新它,禁止重复建。
<在此内联操作二判断表,路径须明确相对项目 .agents/;包含提交后补记与重复自查的处理规则>

## 记录格式

<在此内联模板 B 的必需字段和章节,以及 proposed 的章节要求;输出中不得保留占位符>

## 维护

迁移文件时同步 Status 与相对链接。删除前检查入链与 Git 跟踪状态,未提交的唯一记录优先保留或归档。新增项目工作流时在此或根指南提供链接。
```

## 红线

- 新生成的根 AGENTS.md 标题为 `Repository Guidelines`,建议 200–400 words,少于 100 行;维护已有文件时保留有效指令优先于篇幅目标,有取舍或踩坑规则带理由链接。
- 决策 note 必须有 `Alternatives considered`。
- 一个事实只写一处。
- 所有跨文件引用用相对链接。
- 内容必须来自对本项目的侦查推断,禁止从别的项目复制。
- 写完自查 AI 味:重复的规则只留一个家;删掉推理过程叙述;少用加粗和"重要";implemented 记录里禁止 should 式规格腔——只写已发生的事实。
