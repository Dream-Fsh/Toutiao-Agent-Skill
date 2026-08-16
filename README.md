# Toutiao Agent Skill

This repository keeps a Toutiao/Juyu placement orchestrator separate from independently versioned placement Skills. The orchestrator routes a request to one verified module; a module owns only one workflow boundary.

## Layout

| Path | Responsibility |
| --- | --- |
| `toutiao-placement-orchestrator/` | Routes a placement request and enforces handoffs; it does not contain platform-specific actions. |
| `toutiao-account-import/` | Imports and verifies specified Toutiao media account IDs. |

`gdt-advertising-placement-backup/` is a local-only reference while extracting reusable ideas. It is intentionally excluded from Git and must not be treated as a runnable Toutiao module.

## GitHub workflow

Create one branch and pull request per module: `skill/<module-name>`. Keep changes to the orchestrator in `orchestrator/<topic>` branches. A module PR must update its own Skill resources and the module registry together; it must not silently expand another module's scope.

Do not commit secrets, browser session data, exported cookies, tokens, or production account data.
