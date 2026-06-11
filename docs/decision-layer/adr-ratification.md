# ADR Ratification and Gap Management

In ABD, an ADR is not a memo. It is a ratified decision: a formal record of what was decided, why, and what the consequences are. Ratification is the act by which the human confirms that the decision is correct, complete, and authoritative for the project.

This document describes how ADRs are produced and ratified in the Decision Layer, and how the gap management process works when the Action Layer discovers a case not covered by any existing ADR.

---

## How an ADR is Produced

The LLM produces ADRs. The human does not write them from scratch.

The process is a guided conversation between the human and the LLM. The human expresses an intent, raises a problem, or presents a constraint. The LLM analyzes the context, identifies the decision that needs to be made, proposes options with their trade-offs, and drafts the ADR. The human reviews, corrects, and ratifies.

The effort shifts from writing to deciding. The LLM handles formalization. The human handles judgment.

---

## ADR Structure

Every ADR in an ABD project follows a consistent structure. The LLM produces each section and the human validates it before ratification.

**Context**: the situation that makes a decision necessary. What is the current state, what is the objective, what are the constraints. The context is not an argument for the decision: it is a neutral description of why the decision exists.

**Problem**: the specific question the decision must answer. A well-formed problem statement is precise enough that a reader could independently evaluate whether the decision below resolves it.

**Decision**: what was decided. Stated directly and unambiguously. Not "we considered several options" but "we adopt X because Y". Alternatives considered are listed separately, not inside the decision statement.

**Alternatives considered**: the options that were evaluated and rejected, with the reason for rejection. This section is not bureaucratic padding: it is the record that prevents the team from reopening a decision that was already made and re-evaluating options that were already discarded.

**Consequences**: what changes as a result of the decision. Positive consequences, negative consequences, and constraints introduced. A decision with no negative consequences or constraints was probably not a real decision.

**Risks and mitigations**: the risks introduced by the decision and the measures adopted to reduce them. If a risk has no mitigation, the ADR states that explicitly rather than omitting it.

---

## ADR States

An ADR moves through three states.

**Proposed**: the LLM has drafted the ADR and it is under review by the human. A Proposed ADR is not authoritative: it represents a candidate decision, not a ratified one. No subsequent work should be based on a Proposed ADR.

**Approved**: the human has reviewed the ADR, confirmed that it correctly captures the decision, and ratified it. An Approved ADR is authoritative: it is the source of truth for the decision it covers. Every subsequent decision and every artifact produced in the Action Layer and Execution Layer must be consistent with it.

**Superseded**: the ADR has been replaced by a new ADR that revises or reverses the original decision. A Superseded ADR is not deleted: it remains in the repository as a record of what was decided and why it was later changed. The new ADR references the superseded one explicitly.

The LLM does not treat a Proposed ADR as authoritative. If the human refers to a Proposed ADR as if it were ratified, the LLM surfaces the distinction and requests explicit ratification before proceeding.

---

## Ratification

Ratification is an explicit act. The human reads the ADR, confirms that it correctly captures the decision, and changes its state from Proposed to Approved.

The LLM does not infer ratification from the flow of conversation. An ADR is not ratified because the human said "looks good" in chat or moved on to the next topic. Ratification requires an explicit confirmation that the human is satisfied with the ADR as written and that it can be treated as authoritative.

If the human modifies an Approved ADR after ratification, the LLM treats the modification as a revision and requires re-ratification before treating the updated version as authoritative.

---

## Gap Management

A gap is a case discovered during the Action Layer that is not covered by any existing Approved ADR and cannot be resolved by logical derivation from ratified decisions.

Gaps are expected. They are not failures of the Decision Layer: they are information that the Decision Layer did not yet have. The gap management process exists to handle them in a traceable and controlled way.

**Step 1: the LLM identifies the gap.**
During Action definition, the LLM encounters a case where a decision is required that has no basis in any Approved ADR. The LLM does not resolve the case autonomously. It interrupts Action definition and surfaces the gap to the human with a description of:

- The Action being defined when the gap was identified
- The specific case that cannot be resolved without a new decision
- The candidate decision or options the LLM proposes, if any

**Step 2: the process returns to the Decision Layer.**
Action definition is suspended. The human and the LLM enter a Decision Layer session to address the gap. The LLM produces a new ADR that covers the missing case. The human ratifies it.

**Step 3: the Exit Checklist is re-executed.**
Every new Approved ADR is a change to the decision foundation of the project. Before the Action Layer resumes, the Exit Checklist is re-executed in full to confirm that the new ADR is consistent with all existing Approved ADRs and that the Decision Layer is still complete.

**Step 4: Action definition resumes.**
With the new ADR ratified and the Exit Checklist passed, the Action Layer resumes from the point where it was suspended. The LLM applies the new decision to the Action that triggered the gap and continues.

---

## What the LLM Will Not Do

The LLM will not resolve a gap autonomously. A gap that is resolved without a ratified ADR is a silent decision: it exists in the Actions List or in the code, but it has no declared origin and no traceable rationale. Silent decisions are exactly what ABD is designed to prevent.

The LLM will not treat a gap as minor or obvious and proceed without surfacing it. There are no minor gaps. A case that seems obvious to the LLM may conflict with a business constraint the LLM does not have in context. The human decides what is minor: the LLM surfaces everything.

The LLM will not resume Action definition after a gap without re-executing the Exit Checklist. A new ADR changes the decision foundation. The Exit Checklist exists to verify that the foundation is consistent after every change.
