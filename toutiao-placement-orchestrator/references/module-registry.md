# Module Registry

## Contract

Register one row per independently deployable Skill. A module must contain `SKILL.md` and `agents/openai.yaml`; its Skill must state exact inputs, a stop boundary, committed-state proof, retry rule, and completion report.

| Module | Path | Capability | Lifecycle boundary | Status |
| --- | --- | --- | --- | --- |
| Toutiao account import | `toutiao-account-import/` | Exact account-ID search, selection, and confirmation | Stops when the account-selection dialog closes and the main-page account state is committed | Enabled |

## Add a Module

1. Create a hyphen-case directory at the project root with the official Skill initializer.
2. Write the module Skill and interface metadata; keep detailed platform selectors or payload schemas in its own `references/` directory.
3. Add its row above only after validation succeeds.
4. Use the `skill/<module-name>` branch prefix and add the module name to its pull-request title, for example `skill/toutiao-creative-upload`.
5. Do not make the orchestrator route to a module until its pull request is merged.
