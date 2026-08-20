## Why

The simple-he schemas currently initialize the `chaifen` option in its first state, so character-root decomposition comments are hidden until the user toggles them manually. Making the existing second state the default exposes the configured decomposition aid immediately while retaining the existing per-session toggle.

## What Changes

- Set the `chaifen` switch default to state index `1` (`扌斥`) in both `jde` and `jde_fixed`.
- Preserve the existing `Ctrl+I` toggle, option labels, `simplifier@chaifen` filters, and `*.custom.yaml` override behavior.

## Capabilities

### New Capabilities
- `chaifen-default-state`: Defines the initial visibility of simple-he character-root decomposition comments across the simple-he schemas.

### Modified Capabilities

- None.

## Impact

- Affects the `switches` entries in `jde.schema.yaml` and `jde_fixed.schema.yaml`.
- Changes the initial UI state only; it introduces no API, dictionary, Lua, dependency, or migration changes.
