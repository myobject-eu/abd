# Execution Layer

The Execution Layer is the phase of ABD in which the coding agent generates the project code. It follows the Action Layer, which produces the Actions List, and the Decision Layer, which ratifies the architectural choices. By the time the Execution Layer starts, all decisions have been made and validated. The Execution Layer translates them into working code.

The Execution Layer has two distinct moments.

**Execution Layer Setup** is the preparation phase. Starting from the Actions List and the ratified architectural choices, the human and the coding agent produce the Steering Files: a set of structured Markdown documents that together constitute the complete operational context for code generation. No code is written during Setup. The output is a fully populated `abd/` directory in the development project, capped by an `AGENTS.md` that references all applicable Steering Files.

**Execution Layer Process** is the generation phase. The coding agent reads `AGENTS.md`, loads all referenced Steering Files, and develops the Actions one by one following the order declared in `action-dependencies.md`. Each Action is developed, tested, and validated before the next one starts. The human reviews and approves each output before the coding agent proceeds.

---

## What is in this directory

| File or directory | Contents |
| --- | --- |
| `steering-files-table.md` | Authoritative list of all Steering Files: function, applicability, and reference to each definition document |
| `actions-list-definition.md` | Template and compilation rules for `actions-list.md` |
| `stack-config-definition.md` | Template and compilation rules for `stack-config.md` |
| `integration-config-definition.md` | Template and compilation rules for `integration-config.md` |
| `database-schema-definition.md` | Template and compilation rules for `database-schema.md` |
| `transport-contract-definition.md` | Template and compilation rules for `transport-contract.md` |
| `binding-contract-definition.md` | Template and compilation rules for `binding-contract.md` |
| `security-model-definition.md` | Template and compilation rules for `security-model.md` |
| `ui-spec-definition.md` | Template and compilation rules for `ui-spec.md` |
| `action-dependencies-definition.md` | Template and compilation rules for `action-dependencies.md` |
| `AGENTS-definition.md` | Template and compilation rules for `AGENTS.md`, with agent compatibility reference and notes on vendor-specific redirect files |

The `presets/` directory at the repository root contains precompiled Steering Files for common stack combinations, ready to be used as a starting point in a development project.

---

## Where Steering Files live

The definition documents in this directory describe the template and the rules. The actual Steering Files compiled with project-specific data live in the `abd/` directory of the development project, not in this repository.

```
ABD repository          Development project
docs/execution-layer/   abd/
  stack-config-           stack-config.md
    definition.md   →     (compiled from template)
  security-model-         security-model.md
    definition.md   →     (compiled from template)
  ...                     ...
                          AGENTS.md
```

---

## Starting point

If you are setting up the Execution Layer for a project, start from `steering-files-table.md`. It tells you which Steering Files apply to your project and points to the definition document for each one.

If your stack matches one of the available Presets, start from `presets/` instead. Copy the relevant Preset into your `abd/` directory and customize it with your project-specific details before validation.
