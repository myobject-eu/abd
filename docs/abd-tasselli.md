# ABD Tasselli

A Tassello is a completeness condition that must be reached before the current phase can be considered concluded. A Tassello is not just formally present when the artifact exists: it is **mature** when the artifact is sufficiently defined to support the next phase without transmitting gaps or ambiguities.

The LLM evaluates both presence and maturity. For Selective artifacts, the LLM does not impose production but signals the opportunity when the context requires it.

---

## Layer 1 – Decision Layer

| Code | Artifact | Reference | Type | Approval requirements |
|------|----------|-----------|------|-----------------------|
| 1.01 | Goals & Scope Statement | ADEXMO Recommended Path #01 | Required | System goal declared. Target users identified. In-scope and out-of-scope responsibilities explicit. Main assumptions and external dependencies listed. The boundary is clear enough to reject out-of-scope candidates. |
| 1.02 | Glossary | ADEXMO Recommended Path #02 | Required | Key domain terms defined with unambiguous meanings. Terms used in Actions, Domains, inputs, outputs, and rules are stable. No recurring term with conflicting meanings across the team. |
| 1.03 | Actor Map | ADEXMO Recommended Path #03 | Required | All primary initiators of system behavior identified. Human and system actors listed with responsibility description and main interaction type. |
| 1.04 | Context Diagram / System Map | ADEXMO Recommended Path #04 | Required | System boundary declared. External actors and systems identified. Main interactions described. Clear separation between what belongs to the system and what belongs externally. |
| 1.05 | Use Case List | ADEXMO Recommended Path #05 | Required | Main business operations listed by actor. Each use case has an identifier, actor, name, and expected result. Edge cases not required at this stage. The list is stable enough to proceed to Domain mapping. |
| 1.06 | Use Case Detail | ADEXMO Recommended Path #08 | Selective | Produced only for use cases with complex alternative flows, state-dependent rules, ambiguous responsibility boundaries, or unclear inputs and outputs. The LLM signals the need for this artifact when a use case cannot be translated into candidate Actions without ambiguity. |
| 1.07 | Sequence Diagrams | ADEXMO Recommended Path #09 | Selective | Produced only for flows involving multiple Domains, external systems, asynchronous processes, or complex responsibility boundaries. The LLM signals the need when Action boundaries or external responsibilities cannot be determined from the Use Case Detail alone. |
| 1.08 | State Diagram | ADEXMO Recommended Path #10 | Selective | Produced only for entities with lifecycle constraints that affect which Actions are allowed or forbidden. The LLM signals the need when state transitions influence Action preconditions or rules. |
| 1.09 | Domain Map | ADEXMO Recommended Path #06 | Required | Domains defined by business or system responsibility, not by technical layer. Each domain has a name, responsibility description, included behaviors, and excluded behaviors where relevant. Every relevant use case can be assigned to one Domain. No use case left without a Domain unless explicitly out of scope. |
| 1.10 | Business Rules Summary | ADEXMO Recommended Path #07 | Required | Business rules that influence system behavior collected with identifier, rule statement, and affected Domain. Rules needed to define predictable Actions are visible and not scattered across use cases. |
| 1.11 | Constraint Summary | ADEXMO Recommended Path #12 | Required | Business, security, technical, integration, and data constraints documented. Constraints that affect Action behavior, input limits, output expectations, or execution requirements are explicit. |
| 1.12 | ADR – Domain Analysis | [Domain analysis](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/domain-analysis.md) | Required | Domain Analysis ADR ratified by the human. Domains, actors, use cases, feasibility, and impact are crystallized in a ratified decision. |
| 1.13 | ADR – Technical Constraints | [Tecnical Constraints](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/technical-constraints.md) | Required | Technical constraints ADR ratified by the human. Stack, database, UI type, security model, and integration constraints declared as ratified decisions. |
| 1.14 | Architectural ADRs | [ADR Process](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/adr-process.md) | Required | All relevant architectural decisions crystallized in approved ADRs. No ADR in Proposed state not yet ratified by the human. |
| 1.15 | Exit Checklist | [Exit Checklist](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/exit-checklist.md) | Required | All Exit Checklist conditions satisfied for the Decision Layer. Human has explicitly ratified the decision to proceed to the Action Layer. |
| 1.16 | Gap ADRs | [ADR Ratification and Gap Management](https://github.com/myobject-eu/abd/blob/main/docs/decision-layer/adr-ratification.md) | Selective | Produced when the Action Layer discovers a case not covered by any existing ADR and not derivable from ratified decisions. The LLM interrupts and signals the gap as an ADR candidate. The process returns to the Decision Layer before proceeding. The Exit Checklist is re-executed after each significant ADR addition. |

---

## Layer 2 – Action Layer

| Code | Artifact | Reference | Type | Approval requirements |
|------|----------|-----------|------|-----------------------|
| 2.01 | Domain candidates validated | [Domain Validation](https://github.com/myobject-eu/abd/blob/main/docs/action-layer/domain-validation.md) | Required | LLM has proposed Domain candidates with boundaries and motivations derived from ratified ADRs. Human has validated, modified, or rejected each Domain. No Action defined before all Domains are validated. |
| 2.02 | Draft Actions List | ADEXMO Recommended Path #14 | Required | Every relevant use case represented by one or more candidate Actions. Each Action has Domain, name, intent, input, output, rules, constraints, and signature. Every candidate Action belongs to a validated Domain. No Action with ambiguous input/output, overlapping rules with another Action, or unresolved responsibility boundaries. The LLM signals the need for deeper analysis on any Action that cannot be defined without ambiguity. |
| 2.03 | Validated Actions List | ADEXMO Recommended Path #15 | Required | All Actions validated by the human. Domains ownership verified. No technical Actions. No duplicates. Inputs and outputs clear. Rules and constraints attached. Each Action independent from interface. Stable enough to be used by implementation teams. |

---

## Layer 3 – Execution Layer

| Code | Artifact | Reference | Type | Approval requirements |
|------|----------|-----------|------|-----------------------|
| 3.01 | Action Dependencies Map | [Action Dependencies Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/action-dependencies-definition.md) | Required | Dependencies between Actions declared in `action-dependencies.md`. Implementation sequence defined. No circular dependencies. |
| 3.02 | AGENTS.md | [AGENTS definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/AGENTS-definition.md) | Required | Project Agent Instruction File present and complete. Vendor-specific files (CLAUDE.md, etc.) are single-line redirects to AGENTS.md. |
| 3.03 | Stack Config Steering File | [Stack Config Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/stack-config-definition.md)] | Required | Technology stack declared: language, framework, database, runtime. No ambiguity in the tools the agent must use. |
| 3.04 | Integration Config Steering File | [Integration Config Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/integration-config-definition.md) | Required | External integrations declared: third-party services, APIs, environment variables, secrets management. Separated from stack config. |
| 3.05 | Database Schema Steering File |[Database Schema Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/database-schema-definition.md) | Required | Database schema declared in DBML with neutral type vocabulary. Schema consistent with the Actions List contracts. |
| 3.06 | Security Model Steering File | [Security Model Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/security-model-definition.md) | Required | Application security model declared: authentication, authorization, role definitions, permission rules per Action. Consistent with constraints declared in Layer 1. |
| 3.07 | Architectural Presets Steering File | [Presets](https://github.com/myobject-eu/abd/blob/main/presets/README.md) | Required | Reusable architectural patterns declared as presets: response structures, error handling, validation conventions, naming rules. |
| 3.08 | Transport Binding Steering File | [Transport Contract Definition](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/transport-contract-definition.md) | Required | API contracts declared: transport model, binding between Actions and endpoints, request/response mapping. |
| 3.09 | Execution Layer Setup complete | [Execution Layer](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/README.md) | Required | All Steering Files present, consistent with each other, and consistent with the Validated Actions List. Human has reviewed and ratified the setup before code generation begins. |
| 3.10 | Action implemented and tested | [Action Implementation](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/action-implementation.md) | Required | Per each Action in dependency order: code generated, CLI test executed, output matches contract. Maximum three autonomous attempts on test failures before escalation to developer. |
| 3.11 | Action approved and committed | [Action Implementation](https://github.com/myobject-eu/abd/blob/main/docs/execution-layer/action-implementation.md) | Required | Developer has reviewed generated code, approved or requested modifications, committed verified code, and marked the Action as implemented in the Actions List with commit reference. |
