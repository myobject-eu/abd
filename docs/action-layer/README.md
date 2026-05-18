# Action Layer – ABD

## What it is

The Action Layer is the second level of the Action-Based Development (ABD) method. It translates the decisions ratified in the Decision Layer into an executable contract: the Actions List.

The Actions List defines what the system does, independently of how it exposes it. It is the shared reference between analysis, implementation, and separate teams. It is the precise instruction that Claude Code receives for code generation.

## Core tool

The Action Layer is entirely based on **ADEXMO (Action-Driven Execution Model)**.

The complete documentation of ADEXMO, including the conceptual model, templates, and operative rules, is available in the dedicated repository:

[myobject-eu/adexmo](https://github.com/myobject-eu/adexmo)

This document does not re-document ADEXMO. It only describes how the layer fits into the ABD method.

## Input

- Domain Analysis ADR ratified by the human
- Technical constraints ADR ratified by the human
- All architectural ADRs approved in the Decision Layer
- Decision Layer Exit Checklist passed

## Output

- ADEXMO Actions List versioned in the project repository

## How to start

The human signals the intention to begin the Action Layer. The LLM verifies that all inputs are present before proceeding. If any input is missing, the LLM blocks the session and lists what needs to be completed in the Decision Layer first.

Once all inputs are confirmed, the LLM begins the operative process without waiting for further instructions.

## Operative process

The process is iterative and validated between the LLM and the human in five phases:

1. The LLM reads the ratified ADRs and identifies the candidate Domains
2. The human validates the Domains before the LLM proceeds
3. The LLM proposes the Actions for each Domain with input, output and business rules contract
4. The human validates each Action before the LLM proceeds to the next
5. The LLM produces the final Actions List

If during phase 3 a case emerges that is not covered by any existing ADR, the process returns to the Decision Layer for the production of the relevant ADR before proceeding.

## Related decisions

- ADR-0003 – Adoption of ADEXMO as the Action Layer model
- ADR-0009 – Action Layer operative process in ABD