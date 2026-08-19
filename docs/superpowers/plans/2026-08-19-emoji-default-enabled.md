# Emoji Default Enabled Implementation Plan / Emoji 默认开启实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
>
> **面向代理执行者：** 实施本计划时必须使用 `superpowers:subagent-driven-development`（推荐）或 `superpowers:executing-plans` 子技能，逐任务执行。步骤使用复选框（`- [ ]`）跟踪。

**Goal:** Enable the shared Emoji switch by default while preserving its existing toggle and user-customization behavior.

**目标：** 在保留现有切换和用户自定义行为的同时，默认开启共享 Emoji 开关。

**Architecture:** `config_base.yaml` defines `switches_list`, which is included by the `jde`, `jde_fixed`, `pinyin`, and `tiger` schemas. Add Rime's existing `reset: 1` switch property to the `emoji` item so its second state is selected at initialization. No schema, Lua, shortcut, or custom-configuration files change.

**架构：** `config_base.yaml` 定义 `switches_list`，由 `jde`、`jde_fixed`、`pinyin` 和 `tiger` 方案引用。在 `emoji` 条目中加入 Rime 已有的 `reset: 1` 开关属性，使初始化时选择第二个状态。不修改方案、Lua、快捷键或自定义配置文件。

**Tech Stack:** Rime schema YAML; PowerShell for a one-off configuration assertion; Git for diff validation.

**技术栈：** Rime 方案 YAML；使用 PowerShell 执行一次性配置断言；使用 Git 检查差异。

## Global Constraints / 全局约束

- Modify only the `emoji` entry in `config_base.yaml`.

  只修改 `config_base.yaml` 中的 `emoji` 条目。

- Preserve `states: [ 💀, 😄 ]`, where index `1` is the enabled state.

  保留 `states: [ 💀, 😄 ]`，其中索引 `1` 是开启状态。

- Preserve the existing `Shift+Space` Emoji toggle and `*.custom.yaml` override behavior.

  保留现有的 `Shift+Space` Emoji 切换和 `*.custom.yaml` 覆盖行为。

- Do not add a permanent test, dependency, hash, baseline, or gate for this one-line configuration default.

  不为这一行配置默认值新增永久测试、依赖、哈希、基线或门禁。

- No Rime deployment binary or YAML parser is available in this repository environment; validate the exact affected fragment and diff rather than adding a toolchain solely for this change.

  当前仓库环境没有 Rime 部署程序或 YAML 解析器；应验证受影响的确切片段和差异，不要仅为此变更新增工具链。

---

### Task 1: Set the Shared Emoji Initial State / 任务 1：设置共享 Emoji 初始状态

**Files / 文件：**
- Modify: `config_base.yaml:9-10`

  修改：`config_base.yaml:9-10`

- Create: `docs/superpowers/plans/2026-08-19-emoji-default-enabled.md`

  创建：`docs/superpowers/plans/2026-08-19-emoji-default-enabled.md`

**Interfaces / 接口：**
- Consumes: `switches_list` in `config_base.yaml`, included by the `jde`, `jde_fixed`, `pinyin`, and `tiger` schemas.

  输入：`config_base.yaml` 中的 `switches_list`，由 `jde`、`jde_fixed`、`pinyin` 和 `tiger` 方案引用。

- Produces: the `emoji` Rime option initializes with `reset: 1`, selecting its existing second state (`😄`).

  输出：Rime 的 `emoji` 选项以 `reset: 1` 初始化，选择现有的第二个状态（`😄`）。

- [ ] **Step 1: Run the pre-change behavior assertion / 步骤 1：运行变更前行为断言**

Run this temporary assertion from the repository root:

从仓库根目录运行以下一次性断言：

```powershell
$lines = Get-Content -LiteralPath 'config_base.yaml'
$emojiStart = [Array]::IndexOf($lines, '  - name: emoji')
if ($emojiStart -lt 0) {
  throw 'emoji switch is missing'
}
$emojiBlock = $lines[$emojiStart..($emojiStart + 2)]
if ($emojiBlock[1] -notmatch '^\s*reset:\s*1\s*$' -or $emojiBlock[2] -notmatch '^\s*states:\s*\[\s*💀\s*,\s*😄\s*\]\s*$') {
  throw 'emoji default is not enabled'
}
```

Expected: the command fails with `emoji default is not enabled` before the edit.

预期：编辑前命令因 `emoji default is not enabled` 失败。

- [ ] **Step 2: Add the minimal Rime switch default / 步骤 2：加入最小 Rime 开关默认值**

Change the shared switch definition to:

将共享开关定义改为：

```yaml
  - name: emoji
    reset: 1
    states: [ 💀, 😄 ]
```

- [ ] **Step 3: Run the behavior assertion after the edit / 步骤 3：编辑后运行行为断言**

Run the Step 1 assertion again.

再次运行步骤 1 的断言。

Expected: the command exits successfully, proving `emoji` initializes with the existing enabled state while leaving the state order unchanged.

预期：命令成功退出，证明 `emoji` 以现有开启状态初始化，同时状态顺序保持不变。

- [ ] **Step 4: Inspect the scope and whitespace of the change / 步骤 4：检查变更范围和空白字符**

Run:

运行：

```powershell
git diff --check
git diff -- config_base.yaml
```

Expected: no whitespace errors; the diff contains only `reset: 1` directly beneath `- name: emoji`.

预期：没有空白错误；差异只包含紧邻 `- name: emoji` 下方的 `reset: 1`。

- [ ] **Step 5: Commit the implementation / 步骤 5：提交实施结果**

```powershell
git add -- config_base.yaml docs/superpowers/plans/2026-08-19-emoji-default-enabled.md
git commit -m "config: enable emoji by default"
```

Expected: the commit contains the configuration change and its implementation plan, without generated artifacts or unrelated files.

预期：提交包含配置变更及其实施计划，不包含生成物或无关文件。
