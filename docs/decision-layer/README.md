# Decision Layer

The Decision Layer is the first layer of ABD. It is the foundation on which everything else is built. Its job is to produce permanent, versioned, ratified memory for the project before a single Action is defined or a single line of code is written.

A project that skips the Decision Layer does not save time. It defers the cost of thinking to the moment when the cost of changing is highest.

---

## What the Decision Layer Produces

The Decision Layer produces three categories of output:

**A domain map.** A ratified description of what the system does, what it does not do, who interacts with it, and what the principal operations are. This is the foundation that every subsequent decision is built upon.

**A set of technical constraints.** Ratified decisions on platform, persistence, and security that define the perimeter within which the Action Layer and the Execution Layer operate.

**A set of ADRs.** Architecture Decision Records that capture every significant decision made during the analysis, with context, alternatives considered, consequences, and risks. These are the permanent memory of the project.

---

## The Four Steps

The Decision Layer follows a mandatory sequence of four steps. No step is optional. No step can be skipped without a ratified decision that documents the accepted risk.

### Step 1 — Domain Analysis

The human and the LLM conduct a guided conversation that maps the domain of the system. Six areas are covered: system objectives, system domains, actors, high-level use cases, feasibility, and impact.

The output is a dedicated ADR ratified by the human. No architectural ADR is produced before this ADR exists and is approved.

Full documentation: [domain-analysis.md](domain-analysis.md)

### Step 2 — Technical Constraints

The human and the LLM define the technical boundaries of the system across three mandatory areas: platform, database and persistence, and security.

The output is one or more ADRs ratified by the human. The choice of structure is left to the human based on project complexity. All three areas must be covered.

Full documentation: [technical-constraints.md](technical-constraints.md)

### Step 3 — ADR Production

Every significant architectural and process decision is captured in an ADR. The LLM proposes options with explicit pros and cons. The human evaluates and ratifies. Every ratified ADR becomes a binding constraint on all subsequent decisions.

No ADR remains in Proposed state when the Decision Layer is declared complete.

Full documentation: [adr-process.md](adr-process.md)

### Step 4 — Exit Checklist

Before any work begins on the Action Layer, the LLM runs a mandatory Exit Checklist. Every condition must be satisfied. If any condition is not met, the LLM does not proceed — it surfaces the gap and proposes the actions required to close it.

The gate can only be passed with every condition satisfied, or with a ratified ADR that explicitly documents the accepted gap and its associated risks.

Full documentation: [exit-checklist.md](exit-checklist.md)

---

## The Role of the LLM in the Decision Layer

The LLM is not a passive recorder in the Decision Layer. It is an active participant with specific responsibilities:

- It drives the Domain Analysis conversation, asking explicit questions and refusing to advance on vague answers
- It consults all existing ADRs before proposing new options, to avoid contradicting ratified decisions
- It surfaces contradictions between proposals and existing ADRs explicitly
- It enforces the Exit Checklist gate without exception

The LLM does not make decisions. The human makes decisions. The LLM ensures that the decisions are grounded, complete, and recorded before the project advances.

---

## The Role of the Human in the Decision Layer

The human is the decision maker. Every ADR is ratified by the human before it becomes binding. The domain map, the technical constraints, and the decision to advance to the Action Layer are all human decisions.

The human is also responsible for the quality of the input. A domain described vaguely produces a domain map that is vague. A feasibility assessment conducted superficially produces a project built on optimistic assumptions. The LLM asks the right questions — but the answers come from the human.

---

## Why the Sequence Is Mandatory

Each step in the Decision Layer depends on the output of the previous one.

Technical constraints cannot be defined without a domain map. You cannot decide where a system runs or how it stores data without knowing what the system does and who uses it.

ADRs cannot be produced without a domain map and technical constraints. Architectural decisions made without these foundations are decisions made in a vacuum. They will be revisited when the foundations are eventually defined.

The Exit Checklist cannot be passed without ratified ADRs. A decision in Proposed state is not a decision. It is a hypothesis. The Action Layer is built on decisions, not hypotheses.

Violating this sequence does not accelerate the project. It relocates the cost of incompleteness to a later stage where it is more expensive to resolve.

---

## ADRs as Project Memory

ADRs are the mechanism that makes ABD sustainable over time. They are the answer to the questions that would otherwise return in every new session:

- Why did we choose this architecture?
- Why did we reject that alternative?
- What constraints are we operating within?
- What decisions have already been made?

When an LLM loads the project ADRs at the start of a new session, it does not start from zero. It starts from the accumulated memory of every decision the project has made. This is what makes coordination across sessions, across team members, and across companies possible.

---

## Reference

- ADR-0001 – Adoption of the Action-Based Development method
- ADR-0002 – Adoption of ADRs as the memory tool of the Decision Layer
- ADR-0005 – Extension of the Decision Layer with the Domain Analysis phase
- ADR-0006 – Exit Checklist as mandatory gate before the Action Layer
- ADR-0007 – Technical Constraints as a prerequisite of the Decision Layer
- ADR-0008 – Separation between system documentation and project ADRs