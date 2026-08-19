# OpenSpec Project Initialization Implementation Plan / OpenSpec 项目初始化实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
>
> **面向代理执行者：** 实施本计划时必须使用 `superpowers:subagent-driven-development`（推荐）或 `superpowers:executing-plans` 子技能，逐任务执行。步骤使用复选框（`- [ ]`）跟踪。

**Goal:** Initialize this Rime schema repository with the standard Codex OpenSpec layout and a bilingual, repository-specific `AGENTS.md`.

**目标：** 为这个 Rime 输入方案仓库初始化标准的 Codex OpenSpec 结构，并创建仓库专用的双语 `AGENTS.md`。

**Architecture:** Let the installed OpenSpec CLI create its own standard `openspec/` files and Codex instruction files. Add only a root-level `AGENTS.md` for repository context and safety rules; do not alter Rime schemas, dictionaries, Lua code, or existing project records.

**架构：** 由已安装的 OpenSpec CLI 生成标准的 `openspec/` 文件和 Codex 指令文件。仅新增根目录 `AGENTS.md` 记录仓库背景与安全规则；不修改 Rime 方案、词库、Lua 代码或现有项目记录。

**Tech Stack:** OpenSpec CLI; Markdown; PowerShell; Git.

**技术栈：** OpenSpec CLI；Markdown；PowerShell；Git。

## Global Constraints / 全局约束

- Run `openspec init --tools codex --no-animation` and let the CLI own the standard OpenSpec layout.

  执行 `openspec init --tools codex --no-animation`，由 CLI 负责标准 OpenSpec 目录结构。

- Write all initialization documents in bilingual English-first format: place the English description before its Chinese counterpart, while keeping commands, paths, and code unchanged.

  所有初始化文档均采用英文在前的双语格式：英文描述放在对应中文之前，命令、路径和代码保持不变。

- Keep all existing Rime schemas, dictionaries, Lua files, and prior design/plan documents unchanged.

  保持现有 Rime 方案、词库、Lua 文件以及此前的设计/计划文档不变。

- Do not generate integrations for unrelated AI tools, add dependencies, or introduce permanent release gates.

  不为无关的 AI 工具生成集成，不新增依赖，也不引入永久性的发布门禁。

---

### Task 1: Initialize Standard OpenSpec Files / 任务 1：初始化标准 OpenSpec 文件

**Files / 文件：**
- Create or modify: files selected by the installed OpenSpec CLI under `openspec/` and its Codex instruction path.

  创建或修改：已安装 OpenSpec CLI 在 `openspec/` 及其 Codex 指令路径下选择的文件。

**Interfaces / 接口：**
- Consumes: the repository root and the `codex` tool profile.

  输入：仓库根目录和 `codex` 工具配置。

- Produces: a CLI-recognized OpenSpec project with Codex instructions.

  输出：可被 CLI 识别并包含 Codex 指令的 OpenSpec 项目。

- [ ] **Step 1: Confirm the pre-initialization state / 步骤 1：确认初始化前状态**

Run:

运行：

```powershell
Test-Path -LiteralPath 'openspec'
Test-Path -LiteralPath 'AGENTS.md'
```

Expected: both commands output `False`.

预期：两个命令均输出 `False`。

- [ ] **Step 2: Run the standard OpenSpec initializer / 步骤 2：运行标准 OpenSpec 初始化器**

Run from the repository root:

在仓库根目录运行：

```powershell
openspec init --tools codex --no-animation
```

Expected: the command completes successfully and creates the CLI-selected standard files without touching Rime source files.

预期：命令成功完成，并创建 CLI 选择的标准文件，不修改 Rime 源文件。

- [ ] **Step 3: Confirm CLI recognition / 步骤 3：确认 CLI 能识别项目**

Run:

运行：

```powershell
openspec doctor
openspec status
```

Expected: both commands recognize the initialized project and do not report an initialization error.

预期：两个命令都能识别已初始化项目，且不报告初始化错误。

### Task 2: Add Repository Agent Rules / 任务 2：添加仓库代理规则

**Files / 文件：**
- Create: `AGENTS.md`

  创建：`AGENTS.md`

**Interfaces / 接口：**
- Consumes: the repository structure and generated OpenSpec instruction path from Task 1.

  输入：仓库结构以及任务 1 生成的 OpenSpec 指令路径。

- Produces: bilingual English-first instructions for agents working in this repository.

  输出：面向本仓库代理的英文优先双语说明。

- [ ] **Step 1: Write the repository context and branch rule / 步骤 1：写入仓库背景与分支规则**

Include these exact points in English followed by Chinese:

英文后紧跟中文，必须包含以下要点：

```markdown
# AGENTS.md

## Repository / 仓库

This repository contains Rime input schemas, dictionaries, OpenCC data, and Lua extensions.
本仓库包含 Rime 输入方案、词库、OpenCC 数据和 Lua 扩展。

## Branch / 分支

The default remote branch is `mine`; keep feature changes on the local `mine` branch unless the user explicitly requests another branch.
远程默认分支是 `mine`；除非用户明确要求其他分支，否则将功能变更保留在本地 `mine` 分支。
```

- [ ] **Step 2: Write scoped-change and safety rules / 步骤 2：写入范围控制与安全规则**

Add bilingual rules stating that configuration changes must remain focused, preserve `*.custom.yaml` overrides, and avoid destructive commands or unrelated generated files. Mention that existing user changes must be preserved.

加入双语规则，说明配置变更必须保持聚焦、保留 `*.custom.yaml` 覆盖能力，并避免破坏性命令或无关生成文件；同时说明必须保留用户已有改动。

- [ ] **Step 3: Point to OpenSpec and verification / 步骤 3：指向 OpenSpec 与验证方式**

Link to the generated OpenSpec instructions using a repository-relative path. Document focused fragment assertions, `openspec doctor`, `openspec status`, and `git -c core.whitespace=cr-at-eol diff --check` as appropriate checks.

使用仓库相对路径链接到生成的 OpenSpec 指令。记录片段断言、`openspec doctor`、`openspec status` 和适用的 `git -c core.whitespace=cr-at-eol diff --check` 作为验证方式。

### Task 3: Verify Scope and Commit / 任务 3：验证范围并提交

**Files / 文件：**
- Inspect: all files created or modified by Tasks 1-2.

  检查：任务 1-2 创建或修改的全部文件。

**Interfaces / 接口：**
- Consumes: initialized OpenSpec files and `AGENTS.md`.

  输入：已初始化的 OpenSpec 文件和 `AGENTS.md`。

- Produces: a clean, reviewable commit containing only initialization artifacts.

  输出：只包含初始化产物、可审阅的干净提交。

- [ ] **Step 1: Check bilingual content and paths / 步骤 1：检查双语内容与路径**

Run:

运行：

```powershell
rg -n "OpenSpec|AGENTS|仓库|分支|验证|Repository|Branch|Verification" AGENTS.md openspec
```

Expected: the command finds English and Chinese instructions, and all referenced paths exist.

预期：命令找到英文和中文说明，且所有引用路径都存在。

- [ ] **Step 2: Check the diff scope / 步骤 2：检查差异范围**

Run:

运行：

```powershell
git status --short
git diff --check
git diff --name-only HEAD
```

Expected: only `AGENTS.md`, OpenSpec-generated files, and this implementation plan/design documentation are changed; no Rime schema, dictionary, Lua, or OpenCC source file appears.

预期：只有 `AGENTS.md`、OpenSpec 生成文件以及本实施计划/设计文档发生变化；不出现 Rime 方案、词库、Lua 或 OpenCC 源文件。

- [ ] **Step 3: Commit the initialization / 步骤 3：提交初始化结果**

```powershell
git add -- AGENTS.md openspec docs/superpowers/specs/2026-08-20-openspec-project-initialization-design.md docs/superpowers/plans/2026-08-20-openspec-project-initialization.md
git commit -m "chore: initialize OpenSpec project"
```

Expected: the commit contains only bilingual initialization artifacts and documentation.

预期：提交只包含双语初始化产物和文档。
