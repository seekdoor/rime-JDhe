# Emoji Default Enabled Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Enable the shared Emoji switch by default while preserving its existing toggle and user-customization behavior.

**Architecture:** `config_base.yaml` defines `switches_list`, which is included by the `jde`, `jde_fixed`, `pinyin`, and `tiger` schemas. Add Rime's existing `reset: 1` switch property to the `emoji` item so its second state is selected at initialization. No schema, Lua, shortcut, or custom-configuration files change.

**Tech Stack:** Rime schema YAML; PowerShell for a one-off configuration assertion; Git for diff validation.

## Global Constraints

- Modify only the `emoji` entry in `config_base.yaml`.
- Preserve `states: [ 💀, 😄 ]`, where index `1` is the enabled state.
- Preserve the existing `Shift+Space` Emoji toggle and `*.custom.yaml` override behavior.
- Do not add a permanent test, dependency, hash, baseline, or gate for this one-line configuration default.
- No Rime deployment binary or YAML parser is available in this repository environment; validate the exact affected fragment and diff rather than adding a toolchain solely for this change.

---

### Task 1: Set the Shared Emoji Initial State

**Files:**
- Modify: `config_base.yaml:9-10`
- Create: `docs/superpowers/plans/2026-08-19-emoji-default-enabled.md`

**Interfaces:**
- Consumes: `switches_list` in `config_base.yaml`, included by the `jde`, `jde_fixed`, `pinyin`, and `tiger` schemas.
- Produces: the `emoji` Rime option initializes with `reset: 1`, selecting its existing second state (`😄`).

- [ ] **Step 1: Run the pre-change behavior assertion**

Run this temporary assertion from the repository root:

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

- [ ] **Step 2: Add the minimal Rime switch default**

Change the shared switch definition to:

```yaml
  - name: emoji
    reset: 1
    states: [ 💀, 😄 ]
```

- [ ] **Step 3: Run the behavior assertion after the edit**

Run the Step 1 assertion again.

Expected: the command exits successfully, proving `emoji` initializes with the existing enabled state while leaving the state order unchanged.

- [ ] **Step 4: Inspect the scope and whitespace of the change**

Run:

```powershell
git diff --check
git diff -- config_base.yaml
```

Expected: no whitespace errors; the diff contains only `reset: 1` directly beneath `- name: emoji`.

- [ ] **Step 5: Commit the implementation**

```powershell
git add -- config_base.yaml docs/superpowers/plans/2026-08-19-emoji-default-enabled.md
git commit -m "config: enable emoji by default"
```

Expected: the commit contains the configuration change and its implementation plan, without generated artifacts or unrelated files.
