# Exit Checklist

The Exit Checklist is the mandatory gate at the end of the Decision Layer. No work begins on the Action Layer until every condition on this checklist has been satisfied and ratified by the human.

The checklist is not a formality. It is the mechanism that enforces the sequencing guarantee of ABD. A Decision Layer that passes the Exit Checklist is complete. One that does not is not complete, regardless of how much work has been done.

---

## Purpose

The Exit Checklist exists because the cost of an incomplete Decision Layer is not paid in the Decision Layer. It is paid in the Action Layer, when the Actions List inherits the gaps. It is paid in the Execution Layer, when the generated code implements a contract that was built on incomplete analysis. It is paid when the project returns to the Decision Layer after weeks of work to fix something that could have been resolved in an hour at the start.

The LLM is responsible for running this checklist every time the human signals the intention to advance to the Action Layer. The LLM does not proceed until every condition is met.

---

## How It Works

When the human indicates readiness to move to the Action Layer, the LLM runs the checklist and produces an explicit report with the status of every condition: satisfied or not satisfied.

For every condition that is not satisfied, the LLM does not proceed. It identifies the gap, explains what is missing, and proposes the specific actions required to close it before the gate can be passed.

The gate can only be passed in two ways:

1. Every condition is satisfied.
2. The human makes an explicit and ratified decision to proceed with one or more unsatisfied conditions, documented in a dedicated ADR that records the accepted gap and the associated risks.

The second path is not a bypass. It is a conscious decision with a permanent record. The LLM surfaces the risks associated with every accepted gap and does not minimize them.

---

## The Checklist

### Domain Analysis

- [ ] A Domain Analysis ADR has been produced and ratified by the human
- [ ] System objectives are defined: what the system does and what it does not do
- [ ] System domains are defined with explicit boundaries, based on areas of responsibility not technical layers
- [ ] Principal actors are identified with their roles and their needs
- [ ] High-level use cases are mapped and ratified
- [ ] Feasibility has been evaluated and ratified
- [ ] Impact has been evaluated and ratified

### Technical Constraints

- [ ] Platform constraints have been defined and ratified in an ADR: deployment environment, scalability requirements, availability requirements
- [ ] Database and persistence constraints have been defined and ratified in an ADR: persistence model, consistency requirements, migration strategy
- [ ] Security constraints have been defined and ratified in an ADR: authentication model, authorization strategy, data sensitivity classification, compliance requirements

### ADR State

- [ ] All architectural decisions relevant to the project scope have been captured in ADRs
- [ ] No ADR is in Proposed state: every decision is either Approved, Superseded, or explicitly deferred with a documented reason
- [ ] No open contradiction exists between ratified ADRs
- [ ] The ADR index is up to date

### Readiness

- [ ] The human has explicitly confirmed the decision to proceed to the Action Layer
- [ ] The domain map is sufficient to support the definition of ADEXMO domains and Actions without ambiguity
- [ ] The technical constraints are sufficient to guide implementation decisions in the Execution Layer

---

## Reporting Format

When the LLM runs the checklist, it produces a report in the following format:
```
EXIT CHECKLIST REPORT
Date: [date]
Status: PASSED / BLOCKED
DOMAIN ANALYSIS
[condition]: SATISFIED / NOT SATISFIED
...
TECHNICAL CONSTRAINTS
[condition]: SATISFIED / NOT SATISFIED
...
ADR STATE
[condition]: SATISFIED / NOT SATISFIED
...
READINESS
[condition]: SATISFIED / NOT SATISFIED
...
RESULT
[If PASSED]: All conditions satisfied. The project may advance to the Action Layer.
[If BLOCKED]: [n] conditions not satisfied. The following actions are required before the gate can be passed: [list of specific actions]
```
---

## After the Gate

Passing the Exit Checklist is a point in time, not a permanent state. If the Decision Layer is modified after the gate has been passed, the checklist is re-executed before any further work on the Action Layer proceeds.

Modifications that trigger re-execution:
- A new ADR is produced that changes the domain map or the technical constraints
- An existing ratified ADR is superseded
- A previously deferred decision is resolved with an outcome that affects the Actions List

Modifications that do not trigger re-execution:
- Corrections to ADR text that do not change the substance of the decision
- Addition of related decisions references
- State updates that reflect already-ratified decisions

---

## What the LLM Will Not Do

The LLM will not begin any work on ADEXMO or the Actions List before the Exit Checklist has been passed.

The LLM will not treat a partially complete checklist as sufficient to proceed, regardless of how much of the Decision Layer work has been done.

The LLM will not accept a verbal bypass of the gate. If the human insists on proceeding with unsatisfied conditions, the LLM requires a ratified ADR documenting the decision before it advances.

---

## Reference

- ADR-0006 – Exit Checklist as mandatory gate before the Action Layer
- ADR-0005 – Extension of the Decision Layer with the Domain Analysis phase
- ADR-0007 – Technical Constraints as a prerequisite of the Decision Layer
- ADR-0003 – Adoption of ADEXMO as the model of the Action Layer