# Emoji Default Enabled Design / Emoji 默认开启设计

## Goal / 目标

Enable the Emoji switch by default in the shared base configuration.

在共享基础配置中默认开启 Emoji 开关。

## Scope / 范围

- Add `reset: 1` to the `emoji` entry in `config_base.yaml`.

  在 `config_base.yaml` 的 `emoji` 条目中加入 `reset: 1`。

- Preserve the existing state order, `Shift+Space` toggle, and custom configuration overrides.

  保留现有状态顺序、`Shift+Space` 切换键以及自定义配置覆盖能力。

- Apply through the existing shared `switches_list` included by `jde`, `jde_fixed`, `pinyin`, and `tiger`.

  通过 `jde`、`jde_fixed`、`pinyin` 和 `tiger` 方案已引用的共享 `switches_list` 生效。

## Behavior / 行为

`reset: 1` selects the second Emoji state at initialization. Users can still toggle the switch with `Shift+Space` and override the value in their own custom configuration.

`reset: 1` 会在初始化时选择 Emoji 的第二个状态。用户仍可使用 `Shift+Space` 切换，也可以在自己的自定义配置中覆盖该值。

## Validation / 验证

Verify the YAML parses, the Emoji entry contains `reset: 1`, and no unrelated configuration changes are introduced.

验证 YAML 可以解析、Emoji 条目包含 `reset: 1`，且没有引入无关的配置改动。
