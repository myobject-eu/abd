# AGENTS definition

## What is AGENTS.md

`AGENTS.md` is the **Project Agent Instruction File** in ABD. It is a Markdown file that the coding agent reads automatically at the start of every session. Before the first prompt, before any code is touched, the agent loads this file and treats its contents as ground truth for the entire session. It is the primary mechanism through which a project communicates its rules, structure, and context to the coding agent.

`AGENTS.md` is not a prompt. It is the project's constitution: instructions declared here take precedence over anything said in conversation. If a rule in `AGENTS.md` conflicts with a temporary instruction given during a session, `AGENTS.md` wins.

`AGENTS.md` is agent-agnostic. It is the canonical source of context for any coding agent used in the project. Agent-specific entry files such as `CLAUDE.md` or `GEMINI.md` are thin redirect files that delegate to `AGENTS.md` with a single line:

```
@AGENTS.md
```

This means the project context is defined once and works across any coding agent that supports an automatic entry file. Adding support for a new agent requires only creating the corresponding redirect file. No changes to `AGENTS.md` or to any Steering File are needed.

Three scopes exist for the Project Agent Instruction File:

| Scope | Location | Purpose |
| --- | --- | --- |
| Project | Repository root | Rules for the entire project, shared by all team members |
| Subdirectory | Any directory within the project | Rules scoped to a specific subtree or service |
| Personal | Agent-specific user config directory | Individual developer preferences, never committed |

Shorter files are followed more reliably. The recommended length is between 50 and 200 lines. Instructions beyond that threshold are less consistently applied. Non-critical details should be moved to separate reference files linked from `AGENTS.md` using the import syntax supported by the agent in use.

---

## Purpose in ABD

In ABD projects, `AGENTS.md` is the entry point through which the coding agent accesses the entire Execution Layer context. It does not contain the Steering Files directly: it references them. The agent follows those references and loads the relevant files before starting any development task.

`AGENTS.md` in an ABD project declares:

- The project identity and the ABD layer currently active
- The location of the Steering Files and how to read them
- The rules the coding agent must follow during the Execution Layer
- The development plan derived from `action-dependencies.md`
- Any project-specific constraints not covered by the Steering Files

`AGENTS.md` is produced during the Execution Layer Setup after all Steering Files are validated. It is the last file produced before the coding agent starts generating code. Vendor-specific redirect files are produced immediately after, one per coding agent in use on the project.

---

## Template

```
# AGENTS.md

## Project

**Name:** [project name]
**ABD layer:** Execution Layer
**Last updated:** [date]

---

## Steering Files

The following Steering Files are located in `abd/` and must be read before starting any development task.

| File | Purpose |
|---|---|
| `abd/stack-config.md` | Framework, language, runtime |
| `abd/integration-config.md` | External libraries and services |
| `abd/database-schema.md` | Data model in DBML notation |
| `abd/transport-contract.md` | API endpoints and contracts |
| `abd/binding-contract.md` | Controller-to-Action binding |
| `abd/security-model.md` | Security model and libraries |
| `abd/ui-spec.md` | UI framework, components, design tokens |
| `abd/actions-list.md` | Full Actions List with domains and signatures |
| `abd/action-dependencies.md` | Dependencies between Actions and development order |

Read all Steering Files before starting. Do not infer information that is declared in a Steering File.

---

## Development rules

- Develop Actions in the order declared in `abd/action-dependencies.md`, starting from Group 1
- Do not develop an Action whose hard dependencies are not yet completed
- Use only the libraries declared in `abd/integration-config.md`
- Apply the security model declared in `abd/security-model.md` to every Action that involves identity, authentication, sessions, authorization, or event tracking
- Follow the database schema declared in `abd/database-schema.md` without introducing fields or tables not declared there
- Signal explicitly any inconsistency found between Steering Files before proceeding

---

## Project-specific constraints

[Add any constraints specific to this project that are not covered by the Steering Files]

---

## Current development status

[Optional: declare which Actions have been completed, which are in progress, and which are pending]
```

---

## Agent compatibility

Not all coding agents read `AGENTS.md` natively or support a simple redirect mechanism. The table below documents how each major coding agent integrates with `AGENTS.md` in ABD projects.

| Coding Agent | Integration method | Additional file required | Skill loading |
|---|---|---|---|
| **Codex CLI** | Reads `AGENTS.md` natively from the project root | None | `SKILL.md` via skills.sh |
| **Windsurf Cascade** | Reads `AGENTS.md` natively from the project root | None | `SKILL.md` via skills.sh |
| **Cline** | Reads `AGENTS.md` as a supported format | None | `SKILL.md` via skills.sh |
| **GitHub Copilot** | Reads `AGENTS.md` as agent instructions | None | `SKILL.md` via skills.sh |
| **Claude Code** | Reads `CLAUDE.md`; redirect to `AGENTS.md` via `@AGENTS.md` | `CLAUDE.md` with one line: `@AGENTS.md` | `SKILL.md` via skills.sh |
| **Gemini CLI** | Reads `GEMINI.md`; redirect to `AGENTS.md` via `@AGENTS.md` | `GEMINI.md` with one line: `@AGENTS.md` | To be verified |
| **Cursor** | Reads `.cursor/rules/*.mdc`; reference `AGENTS.md` from an always-apply rule | `.cursor/rules/abd.mdc` (see note below) | To be verified |
| **Aider** | No automatic file loading; manual load required | None (see note below) | To be verified |
| **Roo Code** | Reads `.roo/rules/` or `.roorules`; reference `AGENTS.md` from a rule file | `.roo/rules/abd.md` referencing `AGENTS.md` | To be verified |

**Note on Claude Code and Gemini CLI**

Create a redirect file at the repository root containing a single line:

```
@AGENTS.md
```

The agent reads the redirect file at session start and loads `AGENTS.md` as its entry point. No changes to `AGENTS.md` or to any Steering File are required when switching agents.

**Note on Cursor**

Cursor reads `AGENTS.md` in Agent mode but does not use it for Chat and Composer. For full compatibility across all Cursor modes, create a rule file at `.cursor/rules/abd.mdc` with the following content:

```
---
description: ABD project context
alwaysApply: true
---

@AGENTS.md
```

This ensures `AGENTS.md` is loaded in every Cursor session regardless of mode.

**Note on Aider**

Aider does not load any file automatically at session start. To load `AGENTS.md`, run the following command at the beginning of each session:

```
/read AGENTS.md
```

Redirect files are produced during the Execution Layer Setup immediately after `AGENTS.md` is validated, one per coding agent in use on the project. If the project changes coding agent, only the corresponding file needs to be added. No changes to `AGENTS.md` or to any Steering File are required.

---

## Compilation rules

- `AGENTS.md` is produced during the Execution Layer Setup after all Steering Files are validated
- The Steering Files table lists only the files that are applicable to the project based on the security model and stack chosen
- The development rules section references the Steering Files by their exact path in the `abd/` directory
- Project-specific constraints are added only when they represent rules not derivable from the Steering Files
- `AGENTS.md` is updated when a Steering File is added, removed, or significantly revised
- Agent-specific files (redirect files, rule files) are not updated when `AGENTS.md` changes: they always point to `AGENTS.md` and their content never changes
- The current development status section is optional but recommended for projects with long development cycles spanning multiple sessions

---

## Relationship with other Steering Files

| Steering File | Relationship |
| --- | --- |
| `actions-list.md` | Referenced directly as the authoritative source of Actions |
| `action-dependencies.md` | Referenced as the development plan; its group order determines development sequence |
| `security-model.md` | Referenced as the rule source for all security-related code |
| All other Steering Files | Referenced collectively as the context the coding agent must read before starting |