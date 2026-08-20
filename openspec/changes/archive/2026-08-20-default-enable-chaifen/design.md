## Context

See `proposal.md` for the motivation. `jde` and `jde_fixed` each define their own `chaifen` switch, attach a `simplifier@chaifen` filter, and bind `Ctrl+I` to toggle the option. Neither switch currently declares a reset value, so its first state (`不拆`) is the default. `default.yaml` exposes the option in the UI but does not define its state.

## Goals / Non-Goals

**Goals:**
- Initialize `chaifen` in the existing second state (`扌斥`) for both supported simple-he schemas.
- Keep the switch labels, runtime shortcut, filter configuration, and custom configuration precedence unchanged.

**Non-Goals:**
- Change decomposition data, OpenCC mappings, comment formatting, or filter ordering.
- Add the simple-he-only option to `config_base.yaml` or unrelated schemas.
- Alter user dictionaries, Rime deployment behavior, or persisted user settings.

## Decisions

### Set the existing reset property in both schema-local switches

Add `reset: 1` beside the existing `chaifen` switch declaration in `jde.schema.yaml` and `jde_fixed.schema.yaml`. Index `1` selects the existing second state without reordering or renaming the state list.

Keeping the setting local follows the current ownership of `chaifen`: both the switch and its filter are schema-specific. Moving it into `config_base.yaml` would expose an option to schemes that do not configure the corresponding filter.

### Preserve user configuration as the override layer

Use the existing Rime switch default mechanism rather than changing the `Ctrl+I` binding or forcing an option value at runtime. This preserves the normal `*.custom.yaml` configuration path and keeps the user-visible control reversible.

## Risks / Trade-offs

- [A user expects hidden decomposition after deployment] -> The existing `Ctrl+I` toggle remains available, and user custom configuration continues to override the bundled default.
- [The two schema definitions drift] -> Apply and verify the same explicit reset value in both existing `chaifen` entries.

## Migration Plan

Deploy the updated schemas through the normal Rime deployment flow; no data migration is required. To roll back, remove the two `reset: 1` entries and redeploy. Existing custom schema overrides remain untouched throughout.
