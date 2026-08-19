# Emoji Default Enabled Design

## Goal

Enable the Emoji switch by default in the shared base configuration.

## Scope

- Add `reset: 1` to the `emoji` entry in `config_base.yaml`.
- Preserve the existing state order, `Shift+Space` toggle, and custom configuration overrides.
- Apply through the existing shared `switches_list` included by `jde`, `jde_fixed`, `pinyin`, and `tiger`.

## Behavior

`reset: 1` selects the second Emoji state at initialization. Users can still toggle the switch with `Shift+Space` and override the value in their own custom configuration.

## Validation

Verify the YAML parses, the Emoji entry contains `reset: 1`, and no unrelated configuration changes are introduced.
