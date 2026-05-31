# CLAUDE-definition.md

## What is CLAUDE.md

`CLAUDE.md` is a Markdown file that Claude Code reads automatically at the start of every session. Before the first prompt, before any code is touched, Claude Code loads this file and treats its contents as ground truth for the entire session. It is the primary mechanism through which a project communicates its rules, structure, and context to Claude Code.

`CLAUDE.md` is not a prompt. It is the project's constitution: instructions declared here take precedence over anything said in conversation. If a rule in `CLAUDE.md` conflicts with a temporary instruction given during a session, `CLAUDE.md` wins.

Claude Code loads `CLAUDE.md` files by walking up the directory tree from the current working directory toward the repository root, collecting every `CLAUDE.md` file it encounters. This means a project can have multiple `CLAUDE.md` files at different levels of the directory structure, each scoped to its own subtree.

Three scopes exist:

| Scope | Location | Purpose |
|---|---|---|
| Project | Repository root | Rules for the entire project, shared by all team members |
| Subdirectory | Any directory within the project | Rules scoped to a specific subtree or service |
| Personal | `~/.claude/CLAUDE.md` | Individual developer preferences, never committed |

A personal file named `CLAUDE.local.md` placed alongside any `CLAUDE.md` applies personal overrides without affecting teammates. It is never committed to the repository.

Shorter files are followed more reliably. The recommended length is between 50 and 200 lines. Instructions beyond that threshold are less consistently applied. Non-critical details should be moved to separate reference files linked from `CLAUDE.md` using the import syntax:

```
See @abd/actions-list.md for the full Actions List.
See @abd/security-model.md for the security model.
```

Both relative and absolute paths are supported. Imports can be nested up to five levels deep. Imports inside code blocks are not evaluated.

HTML comments in `CLAUDE.md` are stripped before the content is injected into Claude Code's context. Use them to leave notes for human maintainers without consuming context tokens:

```markdown
<!-- This section is reviewed at every sprint boundary -->
```

---

## Purpose in ABD

In ABD projects, `CLAUDE.md` is the entry point through which Claude Code accesses the entire Execution Layer context. It does not contain the Steering Files directly: it references them. Claude Code follows those references and loads the relevant files before starting any development task.

`CLAUDE.md` in an ABD project declares:

- The project identity and the ABD layer currently active
- The location of the Steering Files and how to read them
- The rules Claude Code must follow during the Execution Layer
- The development plan derived from `action-dependencies.md`
- Any project-specific constraints not covered by the Steering Files

`CLAUDE.md` is produced during the Execution Layer Setup after all Steering Files are validated. It is the last file produced before Claude Code starts generating code.

---

## Template

```markdown
# CLAUDE.md

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

## Compilation rules

- `CLAUDE.md` is produced during the Execution Layer Setup after all Steering Files are validated
- The Steering Files table lists only the files that are applicable to the project based on the security model and stack chosen
- The development rules section references the Steering Files by their exact path in the `abd/` directory
- Project-specific constraints are added only when they represent rules not derivable from the Steering Files
- `CLAUDE.md` is updated when a Steering File is added, removed, or significantly revised
- The current development status section is optional but recommended for projects with long development cycles spanning multiple sessions

---

## Relationship with other Steering Files

| Steering File | Relationship |
|---|---|
| `actions-list.md` | Referenced directly as the authoritative source of Actions |
| `action-dependencies.md` | Referenced as the development plan; its group order determines development sequence |
| `security-model.md` | Referenced as the rule source for all security-related code |
| All other Steering Files | Referenced collectively as the context Claude Code must read before starting |


