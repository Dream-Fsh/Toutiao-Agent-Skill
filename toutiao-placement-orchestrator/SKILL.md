---
name: toutiao-placement-orchestrator
description: Route a Toutiao/Juyu advertising request to one approved project Skill, enforce its declared scope and state gates, and return its verified result. Use when a request may span account selection, campaign configuration, creative, budget, review, preview, submission, or a new Toutiao placement module.
---

# Toutiao Placement Orchestrator

Route rather than perform an unbounded placement workflow. Read [module-registry.md](references/module-registry.md) first, then load only the selected module's `SKILL.md`.

## Routing

1. Classify the request against the registry by capability and lifecycle boundary.
2. If exactly one enabled module matches, use it and preserve its input contract, stop boundary, and validation gates.
3. If no module matches, report the unsupported capability and create no campaign-side effects. Point the maintainer to the module contract in the registry.
4. If multiple modules match or the request crosses modules, execute only the first module that has a complete, independently verifiable end state. Report the required handoff rather than guessing a downstream action.

## Operational Rules

- Treat saved settings, a toast, an HTTP response, or `submitting` as intermediate state, not completion.
- Before any irreversible placement action, require the selected module to define a committed UI proof and an explicit manual-submit boundary. If it does not, stop at preview or draft.
- Never infer advertiser accounts, landing pages, products, audiences, budgets, or creatives from a prior run.
- Do not collect, store, echo, or commit cookies, tokens, authorization headers, or passwords.
- Return the module name, requested inputs, verified end state, and next handoff. Clearly label exceptions and unsupported requests.

## Module Maintenance

Each module must be independently versionable under the project root and be registered before the orchestrator can route to it. Follow the registry contract; do not add duplicate capability descriptions to this Skill.
