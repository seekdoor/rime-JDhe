## Purpose

Define a consistent initial display state for simple-he character-root decomposition comments while preserving the existing user controls and schema customization behavior.

## ADDED Requirements

### Requirement: Simple-he decomposition is enabled by default
The system SHALL initialize the `chaifen` option in its existing `扌斥` state for both the `jde` and `jde_fixed` schemas when no user-specific configuration overrides that option.

#### Scenario: Simple-he schema starts with decomposition shown
- **WHEN** the `jde` schema is initialized without a user override for `chaifen`
- **THEN** the `chaifen` option selects its `扌斥` state and eligible candidate comments show character-root decomposition

#### Scenario: Fixed-code simple-he schema starts with decomposition shown
- **WHEN** the `jde_fixed` schema is initialized without a user override for `chaifen`
- **THEN** the `chaifen` option selects its `扌斥` state and eligible candidate comments show character-root decomposition

### Requirement: Existing decomposition controls remain available
The system SHALL retain the existing `Ctrl+I` toggle and SHALL allow `*.custom.yaml` configuration to override the initial `chaifen` state.

#### Scenario: User toggles decomposition visibility
- **WHEN** a user presses `Ctrl+I` while the candidate menu is open in either simple-he schema
- **THEN** the `chaifen` option changes between its existing `不拆` and `扌斥` states

#### Scenario: User configuration overrides the shared default
- **WHEN** a user supplies a valid schema custom configuration that overrides the `chaifen` initial state
- **THEN** the user-specified state takes precedence over the bundled default
