# AGENTS.md

## Repository / 仓库

This repository contains Rime input schemas, dictionaries, OpenCC data, and Lua extensions.
本仓库包含 Rime 输入方案、词库、OpenCC 数据和 Lua 扩展。

## Branch / 分支

The default remote branch is `mine`; keep feature changes on the local `mine` branch unless the user explicitly requests another branch.
远程默认分支是 `mine`；除非用户明确要求其他分支，否则将功能变更保留在本地 `mine` 分支。

## Scoped Changes and Safety / 范围控制与安全

Keep configuration changes focused on the requested behavior, preserve the override capability of `*.custom.yaml` files, and preserve existing user changes.
配置变更必须聚焦于请求的行为，保留 `*.custom.yaml` 文件的覆盖能力，并保留用户已有的改动。

Avoid destructive commands and avoid adding or changing unrelated generated files; inspect targets before any removal or overwrite.
避免执行破坏性命令，避免添加或修改无关的生成文件；任何删除或覆盖操作前都必须先检查目标。

Do not rewrite, delete, or normalize unrelated schemas, dictionaries, OpenCC data, Lua extensions, or local configuration while completing a focused task.
完成聚焦任务时，不得重写、删除或规范化无关的输入方案、词库、OpenCC 数据、Lua 扩展或本地配置。

## OpenSpec and Verification / OpenSpec 与验证

Follow the generated OpenSpec instructions in [`.codex/skills/openspec-propose/SKILL.md`](.codex/skills/openspec-propose/SKILL.md); related generated workflows are under `.codex/skills/openspec-*`.
请遵循[`.codex/skills/openspec-propose/SKILL.md`](.codex/skills/openspec-propose/SKILL.md)中的生成式 OpenSpec 指令；相关生成工作流位于 `.codex/skills/openspec-*` 下。

Use focused fragment assertions for the files and behavior touched by a change, and run `openspec doctor` and `openspec status` when OpenSpec tooling is available.
针对变更涉及的文件和行为使用聚焦的片段断言；OpenSpec 工具可用时运行 `openspec doctor` 和 `openspec status`。

For whitespace-sensitive reviews, run `git -c core.whitespace=cr-at-eol diff --check` and inspect the resulting diff for unintended changes.
对于对空白敏感的审查，运行 `git -c core.whitespace=cr-at-eol diff --check`，并检查差异中是否存在意外变更。
