# ABD Tasselli

A Tassello is a completeness condition that must be reached before the current phase can be considered concluded. A Tassello is not just formally present when the artifact exists: it is **mature** when the artifact is sufficiently defined to support the next phase without transmitting gaps or ambiguities.

The LLM evaluates both presence and maturity. For Selective artifacts, the LLM does not impose production but signals the opportunity when the context requires it.

Marking a Tassello as Fixed in `tasselli-status.md` requires the corresponding artifact, where the Approval requirements reference one, to exist in the project repository and to be ratified by the human where ratification applies. Updating `tasselli-status.md` without the corresponding artifact is a false representation of project state.

---

## Tasselli Status Format

`abd/tasselli-status.md` is a project-level artifact, versioned in the project repository alongside `AGENTS.md`, that tracks progress against the sequence defined above. It is the persistent record a session reads to know where the project stands, and the dashboard a developer or analyst can read without querying the agent.

The file has a **Current position** header, recording the active layer, the code and name of the current Tassello, and the date of the last update.

Below the header, every Tassello from the tables above is represented as a `###` heading (`### <code> – <artifact name>`), with two fixed fields and one variable field:

- **Status**: one of `Pending`, `In progress`, `Fixed`, `Skipped`
  - `Pending`: the Tassello has not yet been addressed
  - `In progress`: work has started but the Tassello has not yet reached maturity
  - `Fixed`: the Tassello is mature and has been ratified by the human
  - `Skipped`: the Tassello is Selective or Optional and was not produced
- **Fixed on**: the ratification date, populated only when Status is `Fixed`
- **Notes**: for `Pending`, `Fixed`, and `Skipped`, a short free-text line, used in particular to record the reason for `Skipped`. For `In progress`, Notes is structured as three sub-fields: **Findings** (what has been produced so far and what is missing or ambiguous), **Impact** (which subsequent Tasselli depend on resolving this), and **Next step** (the concrete action the next session should take).

Example of a Tassello left open across sessions:

```markdown
### 1.09 – Domain Map
- Status: In progress
- Fixed on:
- Notes:
  - Findings: Three Domains drafted. Boundary between Billing and Order Management unresolved.
  - Impact: Blocks Tassello 2.01 until the boundary is decided.
  - Next step: Propose two boundary options to the human for ratification.
```

For the Tasselli that iterate per Action (3.10, 3.11), the entry in `tasselli-status.md` represents the aggregate state of the layer, not the per-Action detail. The per-Action detail remains in the status column of `actions-list.md`.

`tasselli-status.md` does not duplicate the Reference or Approval requirements columns above: for those, the agent and the human consult this document. The status file records only progress against it.

No per-row "last updated" field is kept: row-level change history is delegated to the repository's version control. `Fixed on` records a specific event, ratification, not a generic last-modified timestamp.

---

## Layer 1 – Decision Layer

| Code | Artifact | Reference | Type | Approval requirements |
|------|----------|-----------|------|------------------------|
| 1.01 | Goals & Scope Statement | ADEXMO skill – Recommended Path #01 | Required | System goal declared. Target users identified. In-scope and out-of-scope responsibilities explicit. Main assumptions and external dependencies listed. The boundary is clear enough to reject out-of-scope candidates. |
| 1.02 | Glossary | ADEXMO skill – Recommended Path #02 | Required | Key domain terms defined with unambiguous meanings. Terms used in Actions, Domains, inputs, outputs, and rules are stable. No recurring term with conflicting meanings across the team. |
| 1.03 | Actor Map | ADEXMO skill – Recommended Path #03 | Required | All primary initiators of system behavior identified. Human and system actors listed with responsibility description and main interaction type. |
| 1.04 | Context Diagram / System Map | ADEXMO skill – Recommended Path #04 | Required | System boundary declared. External actors and systems identified. Main interactions described. Clear separation between what belongs to the system and what belongs externally. |
| 1.05 | Use Case List | ADEXMO skill – Recommended Path #05 | Required | Main business operations listed by actor. Each use case has an identifier, actor, name, and expected result. Edge cases not required at this stage. The list is stable enough to proceed to Domain mapping. |
| 1.06 | Use Case Detail | ADEXMO skill – Recommended Path #08 | Selective | Produced only for use cases with complex alternative flows, state-dependent rules, ambiguous responsibility boundaries, or unclear inputs and outputs. The LLM signals the need for this artifact when a use case cannot be translated into candidate Actions without ambiguity. |
| 1.07 | Sequence Diagrams | ADEXMO skill – Recommended Path #09 | Selective | Produced only for flows involving multiple Domains, external systems, asynchronous processes, or complex responsibility boundaries. The LLM signals the need when Action boundaries or external responsibilities cannot be determined from the Use Case Detail alone. |
| 1.08 | State Diagram | ADEXMO skill – Recommended Path #10 | Selective | Produced only for entities with lifecycle constraints that affect which Actions are allowed or forbidden. The LLM signals the need when state transitions influence Action preconditions or rules. |
| 1.09 | Domain Map | ADEXMO skill – Recommended Path #06 | Required | Domains defined by business or system responsibility, not by technical layer. Each domain has a name, responsibility description, included behaviors, and excluded behaviors where relevant. Every relevant use case can be assigned to one Domain. No use case left without a Domain unless explicitly out of scope. |
| 1.10 | Business Rules Summary | ADEXMO skill – Recommended Path #07 | Required | Business rules that influence system behavior collected with identifier, rule statement, and affected Domain. Rules needed to define predictable Actions are visible and not scattered across use cases. |
| 1.11 | Constraint Summary | ADEXMO skill – Recommended Path #12 | Required | Business, security, technical, integration, and data constraints documented. Constraints that affect Action behavior, input limits, output expectations, or execution requirements are explicit. |
| 1.12 | ADR – Domain Analysis | [Domain Analysis](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/domain-analysis.md) | Required | Domain Analysis ADR ratified by the human. Domains, actors, use cases, feasibility, and impact are crystallized in a ratified decision. The corresponding ADR file exists in `abd/decisions/` and is in Approved state. |
| 1.13 | ADR – Technical Constraints | [Technical Constraints](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/technical-constraints.md) | Required | Technical constraints ADR ratified by the human. Stack, database, UI type, security model, and integration constraints declared as ratified decisions. The corresponding ADR file exists in `abd/decisions/` and is in Approved state. |
| 1.14 | Architectural ADRs | [ADR Process](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/adr-process.md) | Required | All relevant architectural decisions crystallized in approved ADRs. No ADR in Proposed state not yet ratified by the human. Each relevant ADR file exists in `abd/decisions/` and is in Approved state. |
| 1.15 | Exit Checklist | [Exit Checklist](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/exit-checklist.md) | Required | All Exit Checklist conditions satisfied for the Decision Layer. Human has explicitly ratified the decision to proceed to the Action Layer. |
| 1.16 | Gap ADRs | [ADR Ratification and Gap Management](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/adr-ratification.md) | Selective | Produced when the Action Layer discovers a case not covered by any existing ADR and not derivable from ratified decisions. The LLM interrupts and signals the gap as an ADR candidate. The process returns to the Decision Layer before proceeding. The Exit Checklist is re-executed after each significant ADR addition. Where produced, the corresponding ADR file exists in `abd/decisions/` and is in Approved state. |

---

## Layer 2 – Action Layer

| Code | Artifact | Reference | Type | Approval requirements |
|------|----------|-----------|------|------------------------|
| 2.01 | Domain candidates validated | [Domain Validation](https://github.com/myobject-eu/abd/blob/main/docs/action-layer/domain-validation.md) | Required | LLM has proposed Domain candidates with boundaries and motivations derived from ratified ADRs. Human has validated, modified, or rejected each Domain. No Action defined before all Domains are validated. |
| 2.02 | Draft Actions List | ADEXMO skill – Recommended Path #14 | Required | Every relevant use case represented by one or more candidate Actions. Each Action has Domain, name, intent, input, output, rules, constraints, and signature. Every candidate Action belongs to a validated Domain. No Action with ambiguous input/output, overlapping rules with another Action, or unresolved responsibility boundaries. The LLM signals the need for deeper analysis on any Action that cannot be defined without ambiguity. `actions-list.md` exists in the project repository with status Draft. |
| 2.03 | Validated Actions List | ADEXMO skill – Recommended Path #15 | Required | All Actions validated by the human. Domains ownership verified. No technical Actions. No duplicates. Inputs and outputs clear. Rules and constraints attached. Each Action independent from interface. Stable enough to be used by implementation teams. `actions-list.md` exists in the project repository with status Validated. |

---

## Layer 3 – Execution Layer

| Code | Artifact | Reference | Type | Approval requirements |
|------|----------|-----------|------|------------------------|
| 3.01 | Action Dependencies Map | [Action Dependencies Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/action-dependencies-definition.md) | Required | Dependencies between Actions declared in `action-dependencies.md`. Implementation sequence defined. No circular dependencies. |
| 3.02 | AGENTS.md | [AGENTS Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/AGENTS-definition.md) | Required | Project Agent Instruction File present and complete. Vendor-specific files (CLAUDE.md, etc.) are single-line redirects to AGENTS.md. |
| 3.03 | Stack Config Steering File | [Stack Config Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/stack-config-definition.md) | Required | Technology stack declared: language, framework, database, runtime. No ambiguity in the tools the agent must use. |
| 3.04 | Integration Config Steering File | [Integration Config Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/integration-config-definition.md) | Required | External integrations declared: third-party services, APIs, environment variables, secrets management. Separated from stack config. |
| 3.05 | Database Schema Steering File | [Database Schema Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/database-schema-definition.md) | Required | Database schema declared in DBML with neutral type vocabulary. Schema consistent with the Actions List contracts. |
| 3.06 | Security Model Steering File | [Security Model Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/security-model-definition.md) | Required | Application security model declared: authentication, authorization, role definitions, permission rules per Action. Consistent with constraints declared in Layer 1. |
| 3.07 | Architectural Presets Steering File | [Presets](https://github.com/myobject-eu/abd/blob/main/presets/README.md) | Required | Reusable architectural patterns declared as presets: response structures, error handling, validation conventions, naming rules. |
| 3.08 | Transport Binding Steering File | [Transport Contract Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/transport-contract-definition.md) | Required | API contracts declared: transport model, binding between Actions and endpoints, request/response mapping. |
| 3.09 | Execution Layer Setup complete | [Execution Layer](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/README.md) | Required | All Steering Files present, consistent with each other, and consistent with the Validated Actions List. Human has reviewed and ratified the setup before code generation begins. |
| 3.10 | Action implemented and tested | [Action Implementation](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/action-implementation.md) | Required | Per each Action in dependency order: code generated, CLI test executed, output matches contract. Maximum three autonomous attempts on test failures before escalation to developer. The Action's implementation status is recorded in `actions-list.md`. |
| 3.11 | Action approved and committed | [Action Implementation](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/action-implementation.md) | Required | Developer has reviewed generated code, approved or requested modifications, committed verified code, and marked the Action as implemented in `actions-list.md` with commit reference. |
