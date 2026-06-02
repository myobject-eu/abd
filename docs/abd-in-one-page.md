# ABD in One Page

ABD is a decision-driven software development method for coding agents. It turns architectural decisions, domain analysis, and action contracts into a structured workspace that a coding agent can use to generate code consistently and without ambiguity.

---

## The flow

```
Analysis → ADRs → Domains → Actions List → Steering Files → Coding Agent → Code
```

**Analysis** Traditional software analysis: macro analysis, micro analysis, use cases, domain modeling. Must be completed before ABD begins. ABD does not replace this step.

**ADRs** Architectural decisions captured in Architecture Decision Records. Each ADR declares context, problem, decision, alternatives considered, and consequences. ADRs live in the project repository, versioned alongside the code.

**Domains** The system decomposed into functional areas. Each Domain groups the Actions that belong to it.

**Actions List** The complete inventory of Actions that constitute the functional contract of the system. Each Action is atomic, single-responsibility, and has a defined signature. Produced by the Action Layer following the ADEXMO pattern. Validated by the human before the Execution Layer starts.

**Steering Files** A set of structured Markdown documents that together constitute the complete operational context for code generation: stack, integrations, database schema, API contracts, security model, UI specifications, and Action dependencies. Produced during Execution Layer Setup. Stored in `abd/` in the project repository.

**Coding Agent** Reads `AGENTS.md`, loads all referenced Steering Files, and develops the Actions one by one following the dependency order declared in `action-dependencies.md`. Each Action is developed, tested, and validated before the next one starts.

**Code** Consistent, traceable, verifiable. Every line of generated code is derivable from a Steering File, which is derivable from the Actions List, which is derivable from ratified ADRs.

---

## What ABD separates

| What | Where it lives |
| --- | --- |
| Method documentation | ABD repository (`myobject-eu/abd`) |
| Project decisions (ADRs) | Project repository (`project/abd/decisions/`) |
| Steering Files and project artifacts | Project repository (`project/abd/`) |
| Reusable agent instructions | Skills directory of the coding agent in use |

---

## The three layers

| Layer | Input | Output | Gate |
| --- | --- | --- | --- |
| Decision Layer | Completed analysis | Ratified ADRs | Exit Checklist |
| Action Layer | Ratified ADRs | Validated Actions List | Human validation |
| Execution Layer | Actions List + Steering Files | Validated code | Per-Action validation |

---

## The core principle

Every layer produces explicit, verifiable artifacts that constrain the next layer. Ambiguity is eliminated progressively. The coding agent never infers what has been declared: if it is in a Steering File, it is law.

---

For the full method documentation, see the [ABD repository](https://github.com/myobject-eu/abd).
