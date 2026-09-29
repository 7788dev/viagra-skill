<p align="center">
  <img src="assets/lion.svg" width="144" height="144" alt="viagra 狮子图标" />
</p>

<h1 align="center">viagra</h1>

<p align="center">伟哥 · 为 AI 编码助手维护可持续的项目记忆。</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-2563eb" alt="License: MIT" /></a>
  <a href="https://agentskills.io"><img src="https://img.shields.io/badge/Format-Agent%20Skills-7c3eae" alt="Format: Agent Skills" /></a>
  <a href="viagra/SKILL.md"><img src="https://img.shields.io/badge/Skills-1-15803d" alt="Available skills: 1" /></a>
</p>

<p align="center">
  <a href="#功能">功能</a> · <a href="#安装">安装</a> · <a href="#使用">使用</a> · <a href="#工作原理">工作原理</a> · <a href="#贡献">贡献</a>
</p>

`viagra` 是一个采用 [Agent Skills](https://agentskills.io) 格式的技能包。它指导 AI 根据实际仓库生成 `AGENTS.md`，记录技术决策，并把重复执行的流程整理为项目专属 Skill，减少新会话反复了解项目、重复讨论和重复踩坑的成本。

## 功能

本仓库提供一个 Skill：[viagra](viagra/SKILL.md)。

| 能力 | 适用场景 | 产出 |
|---|---|---|
| 初始化贡献指南 | 项目缺少指南或用户要求初始化记忆 | 标题为 `Repository Guidelines` 的 `AGENTS.md` |
| 记录技术决策 | 架构、工具或流程出现有取舍的决定 | 问题、决定、备选方案及后果的决策记录 |
| 提交后自查 | 代理完成 Git 提交或较大开发任务 | 按需更新记忆；没有相关变化时不新增文档 |
| 整理项目记忆 | 规则重复、内容过期或指南过长 | 去重、归档、链接修复和精简后的指南 |
| 沉淀工作流 | 同一核心流程已有至少两次成功执行的证据 | 可按需读取的项目工作流 Skill |

生成的基础指南建议 200–400 words、少于 100 行，覆盖实际适用的项目结构、开发命令、编码风格、测试和提交/PR 约定。后续操作在这份指南上增量维护，保留已有有效指令。

## 安装

### 使用 Skills CLI

需要本地可用的 Node.js/npm 环境。在目标项目中运行：

```bash
npx skills add 7788dev/viagra-skill --skill viagra
```

按提示选择目标编码助手和安装方式。添加 `--global` 可安装到用户级目录。具体支持的助手和选项见 [Skills CLI 文档](https://github.com/vercel-labs/skills#install-a-skill)。

### 从本地源码安装

克隆本仓库后，在仓库根目录运行下面的命令，可安装当前本地版本：

```bash
git clone https://github.com/7788dev/viagra-skill.git
cd viagra-skill
npx skills add . --skill viagra
```

也可以将完整的 `viagra/` 文件夹复制到宿主工具支持的技能目录。安装路径、重新加载方式及自动发现机制以宿主文档为准。

## 使用

安装后，在项目会话中直接描述任务：

**初始化项目记忆**

```text
使用 viagra，为当前仓库生成基础 AGENTS.md 并初始化项目记忆。
保留已有规则，所有命令和约定以仓库实际内容为准。
```

**记录已作出的决定**

```text
使用 viagra，记录刚才确定的技术方案、考虑过的替代方案以及取舍。
```

**整理已有记忆**

```text
使用 viagra，整理当前项目的 .agents，合并重复内容并检查相对链接。
```

**检查最近一次提交**

```text
使用 viagra，自查最近一次提交是否需要更新项目记忆，只在命中规则时补写。
```

Skill 被加载后会指导代理在相关时机自查。它不包含 Git hook 或后台服务，不能独立监听外部终端的提交；如果宿主没有自动调用，请显式使用上述提示。

## 工作原理

记忆按用途分为三层，详细规则见 [SKILL.md](viagra/SKILL.md)：

| 位置 | 保存内容 | 读取方式 |
|---|---|---|
| `AGENTS.md` | 贡献指南、常设规则与记忆入口 | 宿主支持时自动加载，否则显式读取 |
| `.agents/notes/` | 决策理由、备选方案与状态 | 通过相对链接按需读取 |
| `.agents/skills/` | 已验证、可重复执行的项目流程 | 宿主支持时发现，或从指南索引读取 |

下面是项目使用一段时间后可能形成的结构。工作流只在满足条件后创建：

```text
your-project/
├── AGENTS.md
└── .agents/
    ├── notes/
    │   ├── README.md
    │   ├── proposed/
    │   ├── implemented/
    │   ├── rejected/
    │   └── archived/
    └── skills/
        └── release-checklist/
            └── SKILL.md
```

维护遵循以下规则：

- 以仓库事实为依据，不编造命令、覆盖率或提交规范。
- 同一事实只保留一个主要来源，其他位置用链接引用。
- 决策记录包含备选方案和取舍，不把普通小改动都写成决策。
- 尽量随代码更新记录；提交后发现遗漏则补到工作区，不自动改写提交历史。
- 归档保留历史语义，移动文件时修复入链和出链。

## 常见问题

**需要运行服务或配置 API Key 吗？**

Skill 本身是一份指令文档，没有独立运行时，也不要求配置专用 API Key。安装工具和编码助手各自的环境要求另计。

**记忆文件存在本地，是否就代表数据不会离开设备？**

产物保存在项目目录中，Skill 本身不提供上传服务。编码助手读取这些文件后的数据处理方式取决于宿主和模型配置；Git 同步也可能将文件推送到远端。不要把密钥或敏感信息写入记忆。

**会覆盖现有 AGENTS.md 吗？**

规则要求先读取已有内容并增量整合。已有有效指令需要保留，清理篇幅时保留必要的导航入口。

**支持所有编码助手吗？**

文件使用 Agent Skills 格式，但安装、调用和 `AGENTS.md` 自动加载能力因宿主而异。本仓库没有完成所有宿主的兼容性测试。

## 仓库结构

```text
viagra-skill/
├── viagra/
│   └── SKILL.md       # 技能入口、操作规则与生成模板
├── assets/
│   └── lion.svg       # 仓库狮子图标
├── README.md
└── LICENSE
```

## 贡献

欢迎通过 [Issue](https://github.com/7788dev/viagra-skill/issues) 报告问题，或提交 PR 改进规则与文档。

请描述触发场景、预期行为和实际行为。修改 Skill 时同步检查 README 中的能力说明，并在临时项目中验证相关场景：已有指南不被覆盖、缺失配置不被编造、重复自查不生成重复记录、移动记录后链接仍然有效。不要把真实项目的私密记录提交为示例。

## 参考

仓库组织参考 [Anthropic Skills](https://github.com/anthropics/skills) 与 [Vercel Agent Skills](https://github.com/vercel-labs/agent-skills) 的公开文档。项目独立维护，与上述组织无隶属关系。

## 友情链接

- [LINUX DO](https://linux.do/) · 技术交流社区

## License

[MIT](LICENSE) · [7788dev](https://github.com/7788dev)
