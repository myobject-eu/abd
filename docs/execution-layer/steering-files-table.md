# steering-files-table.md

## Purpose

This document is the authoritative reference for the Steering Files used in ABD projects. It declares the complete list of Steering Files, their function, the layer in which they are produced, and the conditions under which they are required.

The coding agent reads this document as part of the Execution Layer Setup to determine which Steering Files apply to the current project. The AGENTS.md of each project references only the Steering Files declared as applicable here.

---

## Steering Files Table

| File | Function | Required when |
|---|---|---|
| `actions-list.md` | Executable contract of the system: full Actions List with domains, signatures, and descriptions | Always |
| `stack-config.md` | Technological foundations: language, runtime, framework, platform, persistence engine, base UI framework, tooling, `abd/` structure | Always |
| `integration-config.md` | External dependencies register: third-party libraries, external APIs, authentication services, document generators | When the project adopts external dependencies |
| `database-schema.md` | Database schema in DBML notation, proposed by the LLM and validated by the human | When the project requires persistence |
| `transport-contract.md` | API endpoints and contracts derived from the Actions List | When the project exposes APIs |
| `binding-contract.md` | Controller-to-Action binding: maps each endpoint to its Action and declares authorization middleware | When the project exposes APIs |
| `security-model.md` | Application security model: Identity Management, Authentication, Session Management, Authorization, Access Control, Auditing, Accountability | When the project includes user management or event tracking |
| `ui-spec.md` | UI framework, components, design tokens, layout conventions, and screen flows derived from the Actions List | When the project includes a UI |
| `action-dependencies.md` | Dependencies between Actions in adjacency list format, classified as hard or soft, ordered by topological sort | When dependencies exist between Actions |
| `AGENTS.md` | Automatic entry point for the coding agent: references all applicable Steering Files, declares development rules and project-specific constraints | Always |

---

## Applicability by project type

| File | API only | API + UI | No API | Single user | Multi-user | Multi-tenant |
|---|---|---|---|---|---|---|
| `actions-list.md` | yes | yes | yes | yes | yes | yes |
| `stack-config.md` | yes | yes | yes | yes | yes | yes |
| `integration-config.md` | if needed | if needed | if needed | if needed | if needed | if needed |
| `database-schema.md` | if needed | if needed | if needed | if needed | yes | yes |
| `transport-contract.md` | yes | yes | no | if needed | if needed | if needed |
| `binding-contract.md` | yes | yes | no | if needed | if needed | if needed |
| `security-model.md` | if needed | if needed | if needed | yes | yes | yes |
| `ui-spec.md` | no | yes | yes | if needed | if needed | if needed |
| `action-dependencies.md` | if needed | if needed | if needed | if needed | if needed | if needed |
| `AGENTS.md` | yes | yes | yes | yes | yes | yes |

---

## Presets

For the most common stack combinations, precompiled Steering Files are available in the `presets/` directory of the ABD repository. A Preset is a validated starting point derived from a real ABD project. It is not a final file: it must be reviewed and customized with project-specific details before use.

| Steering File | Preset priority | Notes |
|---|---|---|
| `security-model.md` | High | Presets for common authentication stacks reduce the risk of incomplete security configurations |
| `ui-spec.md` | High | Presets for common UI frameworks ensure aesthetic consistency across ABD applications |
| `stack-config.md` | Medium | Presets for common stack combinations cover the majority of ABD projects |
| `database-schema.md` | Medium | A single starter kit Preset covers the foundational entities common to most projects |
| `integration-config.md` | Low | External dependencies vary too much between projects for generic Presets |

Presets reside in `presets/[steering-file-name]/` and follow the naming convention `[steering-file]-[stack]-[library].md`, with the exception of `database-schema-starter-kit.md` which is stack-independent.

---

## Definition files

Each Steering File has a corresponding definition document in `docs/execution-layer/` that declares its template, compilation rules, and relationship with other Steering Files.

| Steering File | Definition document |
|---|---|
| `actions-list.md` | `docs/execution-layer/actions-list-definition.md` |
| `stack-config.md` | `docs/execution-layer/stack-config-definition.md` |
| `integration-config.md` | `docs/execution-layer/integration-config-definition.md` |
| `database-schema.md` | `docs/execution-layer/database-schema-definition.md` |
| `transport-contract.md` | `docs/execution-layer/transport-contract-definition.md` |
| `binding-contract.md` | `docs/execution-layer/binding-contract-definition.md` |
| `security-model.md` | `docs/execution-layer/security-model-definition.md` |
| `ui-spec.md` | `docs/execution-layer/ui-spec-definition.md` |
| `action-dependencies.md` | `docs/execution-layer/action-dependencies-definition.md` |
| `AGENTS.md` | `docs/execution-layer/AGENTS-definition.md` |
