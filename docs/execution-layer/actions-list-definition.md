# actions-list-definition.md

## What is actions-list.md

`actions-list.md` is the executable contract of the system. It is the final output of the Action Layer and the primary input of the Execution Layer. It contains the complete list of Actions the coding agent must implement, organized by domain, each with its signature, mode, and any relevant constraints.

`actions-list.md` is not produced during the Execution Layer. It arrives as a validated artefact from the Action Layer. The Execution Layer receives it, reads it, and treats it as immutable unless a gap or inconsistency is found that requires returning to the Action Layer.

---

## Origin

`actions-list.md` is produced by the Action Layer following the ADEXMO process. ADEXMO defines the iterative LLM-human cycle through which domains are identified, validated, and translated into a structured list of Actions. The output of that process is `actions-list.md`.

For the template, compilation rules, and the full definition of the Actions List format, refer to the ADEXMO documentation in the Action Layer.

---

## Role in the Execution Layer

Within the Execution Layer, `actions-list.md` serves three functions:

- It is the source from which all other Steering Files are derived during Execution Layer Setup. Stack choices, database schema, API contracts, security model, and UI structure are all derived from the Actions declared in this file.
- It is the authoritative reference the coding agent consults throughout code generation to verify that every Action is implemented as declared.
- It is the baseline against which the human validates each completed Action before the coding agent proceeds to the next.

`actions-list.md` is placed in the `abd/` directory of the development project alongside the other Steering Files. It is referenced explicitly in `AGENTS.md` so the coding agent loads it at session start.

---

## Immutability rule

`actions-list.md` is not modified during the Execution Layer. If the coding agent identifies a gap, an ambiguity, or an inconsistency in the Actions List, it signals the issue to the human before proceeding. Resolution requires returning to the Action Layer to produce a revised `actions-list.md`. The Execution Layer resumes only after the revised file is validated.

---

## Relationship with other Steering Files

| Steering File | Relationship |
| --- | --- |
| `AGENTS.md` | References `actions-list.md` as the authoritative source of Actions |
| `action-dependencies.md` | Derived from `actions-list.md`; defines the implementation order |
| `stack-config.md` | Derived in part from the technical requirements implicit in the Actions |
| `database-schema.md` | Derived from the data structures required by the Actions |
| `transport-contract.md` | Derived from the Actions that expose or consume API endpoints |
| `security-model.md` | Derived from the Actions that involve identity, authentication, and authorization |
| `ui-spec.md` | Derived from the Actions that produce or interact with UI components |
