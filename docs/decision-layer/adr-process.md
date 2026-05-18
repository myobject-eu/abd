# ADR Process

The ADR process is the third step of the Decision Layer in ABD. Once the domain has been mapped and the technical constraints have been ratified, every significant architectural and process decision is captured in an Architecture Decision Record before the project advances to the Action Layer.

ADRs are not documentation produced after the fact. They are the decisions themselves, in permanent and versioned form.

---

## Purpose

An ADR answers three questions that every serious project must be able to answer at any point in its lifecycle:

- Why was this decision made?
- What alternatives were considered and rejected?
- What are the consequences, risks, and mitigations accepted?

Without this record, decisions evaporate. The reasoning behind a choice disappears with the session that produced it. New team members inherit conclusions without context. LLMs in new sessions contradict decisions made in previous ones. Problems that were already solved get reopened.

ADRs are the mechanism that prevents this. They are the permanent memory of the project, versioned alongside the code, readable by humans and LLMs alike.

---

## How It Works

Every significant decision emerges from a guided discussion between the human and the LLM. The LLM proposes options with explicit pros and cons. The human evaluates, challenges, and ratifies. The ratified decision is crystallized in an ADR.

The LLM consults all existing ADRs before proposing new options. A proposal that contradicts a ratified decision is not made without explicitly surfacing the contradiction and the reason for revisiting it.

An ADR in Proposed state is not yet binding. It becomes binding only after the human ratifies it. An ADR that has been superseded by a later decision is marked as Superseded with an explicit reference to the ADR that replaces it.

---

## ADR Structure

Every ADR in an ABD project follows this structure:

**Header**
- Title: a short, specific description of the decision
- Status: Proposed, Approved, Superseded, or Deprecated
- Date: the date of ratification

**Context**
The situation that made this decision necessary. What is the current state, what is the objective, and what are the constraints. This section answers: why did this decision need to be made at all?

**Problem**
The specific problem that this decision addresses. What breaks or degrades if this problem is not resolved? This section is concrete and specific, not generic.

**Decision**
What was decided, stated clearly and without ambiguity. If the decision has operational rules — behaviors that must be followed for the decision to be effective — they are listed here explicitly.

**Alternatives Considered**
Every alternative that was evaluated and the specific reasons it was not chosen. This section is not a formality. It is the record that prevents the same alternatives from being re-proposed in future sessions without awareness of why they were previously rejected.

**Consequences**
The positive and negative consequences of the decision, stated honestly. A decision with only positive consequences has not been analyzed carefully enough.

**Risks and Mitigations**
The principal risks associated with the decision and the specific actions taken to reduce them.

**Related Decisions**
References to other ADRs that this decision depends on, extends, or affects.

---

## Numbering and States

ADRs are numbered progressively starting from ADR-0001. Numbers are never reused. The sequence is global across the project, not per domain or per layer.

Valid states:

- **Proposed**: the decision has been drafted but not yet ratified by the human. Not binding.
- **Approved**: the decision has been ratified by the human. Binding.
- **Superseded**: the decision has been replaced by a later ADR. The superseded ADR remains in the repository with a reference to its replacement. It is never deleted.
- **Deprecated**: the decision is no longer applicable but has not been replaced by a specific new decision.

The Exit Checklist requires that no ADR is in Proposed state at the time the gate is run. Every decision must be either ratified or explicitly deferred with a documented reason.

---

## What Qualifies as an ADR

Not every choice requires an ADR. The criterion is significance: does this decision have consequences that extend beyond a single file or a single session? If yes, it qualifies.

Decisions that qualify:
- Architectural choices that shape the structure of the system
- Technology selections with long-term implications
- Process decisions that affect how the team or the LLM operates
- Decisions that resolve significant trade-offs between competing approaches
- Decisions that define constraints for other decisions

Decisions that do not qualify:
- Variable naming and code formatting
- Temporary or experimental choices not yet ready for ratification
- Implementation details that follow directly from a ratified architectural decision without introducing new trade-offs

When in doubt, produce the ADR. The cost of an unnecessary ADR is lower than the cost of a missing one.

---

## ADRs and the LLM

When an LLM begins a new session on an ABD project, it loads the existing ADRs as part of its mandatory context. This is how the project memory persists across sessions.

The LLM uses the ADRs to:
- Understand the decisions already made and the reasoning behind them
- Avoid proposing options that were previously evaluated and rejected
- Identify contradictions between new proposals and ratified decisions
- Verify the Exit Checklist conditions before advancing to the Action Layer

The LLM does not treat ADRs as suggestions. Ratified ADRs are binding constraints on every subsequent proposal.

---

## Versioning

ADRs are plain `.md` files versioned in the project repository alongside the code. They follow the same review process as code: they are not merged without human approval.

An ADR index file `ADR-INDEX.md` is maintained in the repository root of the ADR directory. It lists every ADR with its number, title, status, and date. This index is the first file the LLM reads when loading project context.

---

## What the LLM Will Not Do

The LLM will not propose a new architectural direction without checking it against existing ratified ADRs.

The LLM will not treat a Proposed ADR as binding. It will surface the fact that ratification is pending and will not advance on the basis of an unratified decision.

The LLM will not allow the Exit Checklist to pass while any ADR remains in Proposed state.

---

## Reference

- ADR-0002 – Adoption of ADRs as the memory tool of the Decision Layer
- ADR-0005 – Extension of the Decision Layer with the Domain Analysis phase
- ADR-0006 – Exit Checklist as mandatory gate before the Action Layer