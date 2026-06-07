# Domain Analysis

Domain Analysis is the mandatory first step of the Decision Layer in ABD. No architectural decision is made, and no ADR is written, until the domain has been explicitly mapped and ratified by the human.

This is not optional. A project that skips Domain Analysis does not save time — it borrows it at a high interest rate, payable when contradictions surface later in the process.

---

## Purpose

Domain Analysis answers the foundational questions that every subsequent decision depends on:

- What must the system do, and what must it not do?
- What are the distinct areas of responsibility within the system?
- Who interacts with the system, and in what role?
- What are the high-level operations the system must support?
- Is the project feasible, technically and operationally?
- Does the project justify the investment?

These questions are not answered by the human alone, nor by the LLM alone. They are answered through a guided discussion between the two, where the LLM asks explicit questions, surfaces ambiguities, and challenges assumptions until every element is covered.

---

## How It Works

The LLM opens the Domain Analysis by asking the human to describe the system they want to build. From that starting point, the LLM drives the conversation through six mandatory areas, in sequence.

The LLM does not proceed to the next area until the current one is sufficiently defined. It does not accept vague answers. It asks follow-up questions until the boundaries are explicit and ratified.

At the end of the conversation, the LLM produces a dedicated ADR that crystallizes the results of the analysis. This ADR is the first document produced for every new ABD project.

---

## The Six Mandatory Areas

### 1. System Objectives

What the system must accomplish, stated in concrete terms. What it must not do, stated with equal clarity.

The LLM asks:
- What problem does this system solve?
- Who benefits from the solution and how?
- What is explicitly out of scope?

A system without explicit boundaries will grow in every direction. Defining what the system does not do is as important as defining what it does.

### 2. System Domains

The distinct areas of responsibility within the system, each with explicit boundaries.

The LLM asks:
- What are the main areas of responsibility in this system?
- Where does one area end and another begin?
- Are there areas that overlap? If so, how is the overlap resolved?

Domains in ABD are not technical layers. They are not "controller", "repository", or "database". They are business areas: "order management", "user identity", "billing", "notifications". A domain groups behavior that belongs together for business reasons, not for technical ones.

This distinction is critical. Domains defined on technical criteria produce an Actions List with structural problems that propagate all the way to the generated code.

### 3. Actors

Who interacts with the system and in what role.

The LLM asks:
- Who uses the system directly?
- Who depends on the system without interacting with it directly?
- What external systems interact with this system?
- What does each actor need from the system?

Actors are not user interface roles. They are entities with distinct needs and distinct permissions. Identifying them early prevents authorization and access control problems from being discovered in the Execution Layer.

### 4. High-Level Use Cases

The principal operations the system must support, at a level of abstraction that is independent of any interface.

The LLM asks:
- What are the main things the system must allow actors to do?
- Are there operations that happen without direct actor input, such as scheduled jobs or event-driven processes?
- Are there dependencies between use cases?

Use cases at this stage are not ADEXMO Actions. They are not yet formal contracts. They are the raw material from which Actions will be defined in the Action Layer. Their purpose here is to confirm that the domain map is complete and that no significant area of behavior has been overlooked.

### 5. Feasibility

Whether the project is technically and operationally achievable with the available resources.

The LLM asks:
- Are the required technologies available and mature enough?
- Does the team have the skills to build and maintain this system?
- Are there known technical risks that could block delivery?
- Is the timeline realistic given the scope?

Feasibility is not pessimism. It is the minimum due diligence required before committing resources to a direction. A project that fails the feasibility check does not necessarily get abandoned — it gets rescoped, rephased, or redirected. But it does not proceed unchanged.

### 6. Impact

Whether the project justifies the investment, either as a business case or as a service capacity.

The LLM asks:
- What value does this system produce when it is operational?
- Who benefits and how does the benefit compare to the cost?
- What happens if the system is not built?
- Is there a measurable outcome that defines success?

Impact is the answer to the question the human asked at the start of this guide: does the game justify the candle? If the answer is not clear after Domain Analysis, it will not become clearer during development.

---

## Output

The output of Domain Analysis is a single dedicated ADR, ratified by the human, that covers all six areas above.

This ADR is not a summary of a conversation. It is a formal record of what was decided. It has the same structure as every other ADR in the project: context, problem, decision, consequences, risks, mitigations.

The domain map it contains is the reference that every subsequent architectural decision is built upon. If a later ADR contradicts the domain map, either the later ADR is wrong or the domain map needs to be updated — and that update requires an explicit revision of the Domain Analysis ADR.

---

## What the LLM Will Not Do

The LLM will not proceed to produce architectural ADRs until the Domain Analysis ADR is ratified.

If the human attempts to skip this step, the LLM surfaces the missing areas and proposes how to complete them before moving forward.

If the human insists on proceeding without completing the Domain Analysis, the LLM requires a ratified ADR that explicitly documents the decision to proceed with an incomplete domain map and the risks accepted.
