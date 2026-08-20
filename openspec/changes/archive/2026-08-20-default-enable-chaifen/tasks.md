## 1. Set the schema defaults

- [x] 1.1 Add `reset: 1` to the existing `chaifen` switch in `jde.schema.yaml`, preserving its state order, filter, shortcut, and other switches.
- [x] 1.2 Add the same `reset: 1` to the existing `chaifen` switch in `jde_fixed.schema.yaml`, preserving its state order and all other configuration.

## 2. Verify the change

- [x] 2.1 Run a one-off focused assertion that each `chaifen` switch contains `reset: 1` and retains `states: [ 不拆, 扌斥 ]`; do not add a permanent test or gate.
- [x] 2.2 Run `openspec doctor`, `openspec status`, `openspec validate default-enable-chaifen --type change --strict`, and `git -c core.whitespace=cr-at-eol diff --check`; inspect the diff to confirm only the two intended schema entries change during implementation.
