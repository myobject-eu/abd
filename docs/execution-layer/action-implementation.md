# Action Implementation

Action implementation is the core operative cycle of the Execution Layer. It is a supervised, iterative process between the coding agent and the developer, structured in explicit phases. It is not a one-shot generation: every Action is implemented, tested, reviewed, and committed individually before the agent moves to the next.

---

## Prerequisites

Before implementation begins, the Execution Layer Setup must be complete. All Steering Files must be present, consistent with each other, and ratified by the human. The Validated Actions List must be stable. The Action Dependencies Map must declare the implementation sequence.

The agent reads all Steering Files referenced in `AGENTS.md` and the full Action Dependencies Map before starting any implementation. It does not begin until the reading is complete.

---

## Implementation Cycle

The agent implements Actions in the order defined in `action-dependencies.md`, one Action at a time.

### Phase 1: Read the Action contract

For each Action, the agent reads the contract from the Validated Actions List:

- Domain and Action name
- Intent: what the Action does in business terms
- Inputs: typed parameters with constraints
- Outputs: typed results with structure
- Business rules: the conditions and transformations that govern the Action
- Constraints: security, performance, integration requirements

The contract is the only source of truth for the implementation. The agent does not infer behavior not declared in the contract.

### Phase 2: Generate the code

The agent generates the code required to implement the Action across one or more files, maintaining consistency with all Steering Files. Every choice not in the contract defaults to the patterns declared in the Steering Files: stack conventions, security model, transport binding, naming rules, error handling. Nothing is inferred from context outside the declared artifacts.

### Phase 3: Test via CLI

ADEXMO defines every Action as independently executable regardless of the interface, CLI included. The agent has direct terminal access. This combination produces a closed verification loop: the agent invokes the Action via CLI, reads the output, and compares it against the contract without needing to start a server, browser, or graphical interface.

If the tests pass, the agent signals completion and waits for developer review.

If the tests fail, the agent iterates autonomously up to a maximum of three attempts. After three failed attempts without resolution, the agent stops and reports to the developer with:

- A description of the failure
- The attempts made and what was tried
- A hypothesis on the root cause

The three-attempt limit is not a technical constraint: it is the boundary between what the agent can resolve autonomously and what requires human judgment.

### Phase 4: Developer review and approval

The developer reviews the generated code before any commit. Approval is an explicit act: the developer confirms that the code correctly implements the contract and is consistent with the codebase.

If the developer requests modifications, the agent applies them and re-runs the tests before requesting approval again.

The agent does not proceed to the next Action without explicit developer approval of the current one.

### Phase 5: Commit and tracking

After approval, the developer commits the verified code to the repository and marks the Action as implemented in the Actions List with the commit reference. The Actions List is the tracking artifact: every Action has a status (to implement, in progress, implemented) and, once implemented, a commit reference.

---

## When an Action Cannot Be Implemented as Defined

If the agent determines that an Action cannot be implemented as defined in its contract, it does not make assumptions and does not modify the contract. It stops and surfaces the problem to the developer as a candidate for Actions List revision, with:

- The Action that triggered the issue
- The specific aspect of the contract that cannot be implemented
- The options available, if any

The process returns to the Action Layer to revise the contract. Once the Actions List is updated and re-validated, the agent resumes from the Action in question.

If the revision affects Steering Files, only the impacted Steering Files are re-validated before implementation resumes. A full Execution Layer Setup re-execution is not required unless the revision is structural.

---

## The Execution Layer and Action Layer Cycle

The boundary between the Execution Layer and the Action Layer is not irreversible. The Execution Layer can return to the Action Layer at any point when it discovers an Action that cannot be implemented as defined.

Each return produces an explicit revision of the Actions List. The developer decides whether the revision requires a new ADR or whether it is a minor contract correction. Frequent returns to the Action Layer are a signal, not an anomaly: they indicate that the Actions List quality was insufficient going into the Execution Layer and warrant a review of the Execution Layer Setup before continuing.

---

## What the Agent Will Not Do

The agent will not implement Actions in an order other than the one defined in `action-dependencies.md`.

The agent will not modify the contract of an Action. If the contract cannot be implemented, the agent surfaces the problem and waits.

The agent will not iterate on test failures beyond three attempts without involving the developer.

The agent will not commit code. Commits are the developer's responsibility.

The agent will not proceed to the next Action without explicit developer approval of the current one.
