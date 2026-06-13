# Working with LLM Agents

ABD is designed to be used with a coding agent. Without one, the method introduces overhead that is difficult to justify. With one, that overhead becomes negligible and the value of structure increases dramatically.

This document explains why structure matters when working with coding agents on medium to large projects, what problems emerge without it, and how ABD addresses those problems in a way that keeps both the human and the agent on track.

---

## The problems coding agents face on real projects

Coding agents perform exceptionally well on isolated, well-defined tasks. A single feature, a clearly scoped bug fix, a self-contained module: these are contexts where an agent can produce high-quality output with minimal guidance.

Medium to large projects are a different environment entirely. The following problems emerge reliably when a coding agent operates without a structured method.

### Noise accumulation

A coding agent builds its understanding of the project from the context available in the session. On a complex project, that context is never complete. The agent fills the gaps with reasonable inferences, and those inferences become implicit decisions. Over time, the codebase accumulates a layer of choices that nobody made explicitly and nobody can trace back to a rationale. This is noise: information that exists in the code but has no declared origin.

Noise is not a quality problem in the immediate sense. The code works. The problem surfaces later, when the team needs to modify a behavior, extend a module, or onboard a new member. The implicit decisions are invisible and their justifications are gone.

### Contradiction across sessions

A coding agent has no persistent memory between sessions. Each session starts from whatever context the human provides. On a long project, the human cannot reliably reconstruct the full decision history at the start of every session. The agent receives partial context, makes reasonable assumptions, and proceeds.

The result is that decisions made in session three get quietly contradicted in session seven. The security model adopted early gets eroded by convenience choices later. The naming conventions established at the start drift by the time the fifth module is developed. Nobody intended any of this. It happens because there is no mechanism to enforce consistency across sessions.

### Redundancy and regeneration

Without a clear contract for what has been decided and what has been built, the agent tends to regenerate rather than extend. Asked to implement a new feature, it may reproduce patterns already present elsewhere in the codebase with minor variations, producing redundant code that diverges over time. Asked to solve a problem it has already solved in a different part of the system, it produces a second solution rather than recognizing and reusing the first.

This is not a failure of the agent. It is a structural problem: the agent lacks the information it needs to recognize what already exists and why it exists in the form it does. In the absence of an explicit contract, the agent does what it is designed to do: it recalls the best solution it knows for the problem at hand. That solution may be technically correct in isolation. But it was conceived in a different context — a different project, a different stack, a different set of constraints. Applied without awareness of the application's existing decisions, it introduces patterns that are locally reasonable but globally inconsistent. The codebase begins to accumulate solutions that work independently but do not belong to the same architectural conversation. Homogeneity erodes not through negligence but through the agent doing its job well without the context it needs to do it right.

### Loss of architectural coherence

The three problems above compound. Noise, contradictions, and redundancy do not accumulate linearly: they interact. A noisy codebase produces more contradictions because implicit decisions are harder to track. Contradictions produce more redundancy because the agent cannot rely on existing patterns that may themselves be inconsistent. Architectural coherence, which is the property that makes a codebase comprehensible and maintainable, degrades faster than the project grows.

On a small project, this degradation is manageable. On a medium to large project, it becomes the dominant cost of development.

---

## What a structured method needs to do

The problems above suggest what a method for coding agents must accomplish. It is not enough to define good practices or coding conventions. Those define limits but do not enforce them. A method that only sets boundaries relies entirely on discipline to stay within them.

What is needed is a method that is also self-regulating: one where the agent itself is guided back toward the method when it risks deviating, and where the structure of the method makes deviation visible before it causes damage.

This means two things in practice.

First, the method must produce artifacts that give the agent an unambiguous source of truth to consult at every step. Not guidelines the agent interprets, but explicit contracts the agent follows. If something is declared in the contract, it is not up for inference.

Second, the method must keep the human in the loop at the points where decisions are made, not after the code has been written. The human guides the agent through decision-making; the agent formalizes and expands. When the agent encounters a gap, it does not fill it autonomously: it surfaces it and waits for a decision. The discipline is built into the process, not left to individual vigilance.

Self-correction is structured, not ad hoc. When the agent identifies a problem with an artifact, it responds in one of three ways, applied consistently across all three layers. If the flaw is local and resolvable, such as an ambiguous boundary between two Domains, the agent proposes a corrected version before asking the human to decide. If the problem is structural, meaning no ratified decision covers the case, the agent does not attempt a local fix: it escalates immediately, and the gap becomes a new decision to ratify. If a decision is being iterated repeatedly without converging, the agent proposes crystallizing the current state into a ratified decision rather than continuing to iterate indefinitely. The test-and-iterate loop used for code is the same mechanism applied to implementation: a resolvable failure is refined locally, up to a limit, before escalating to the developer.

---

## How ABD addresses these problems

ABD approaches the problem through three layers, each of which eliminates a specific class of ambiguity before it can propagate.

### The Decision Layer eliminates ambiguity of intent

Before any contract or code is produced, the architectural decisions of the project are made explicit and ratified. Each decision is captured in an Architecture Decision Record: a structured document that declares not only what was decided but why, what alternatives were considered, and what the consequences are.

ADRs are not written manually by the team. The human expresses an intent or raises a problem; the agent analyzes the context, proposes options with their trade-offs, and drafts the ADR. The human reviews, corrects, and ratifies. The effort shifts from writing to deciding.

The result is a memory that persists across sessions. At the start of any session, the agent reads the ratified ADRs and operates within the decisions they contain. Contradictions between sessions become detectable: if a new proposal conflicts with a ratified ADR, the conflict is visible.

### The Action Layer eliminates ambiguity of contract

Ratified decisions are translated into an executable system contract following the ADEXMO pattern. The system is decomposed into Actions: atomic, single-responsibility units of business logic, each with a defined domain, typed inputs and outputs, and explicit business rules. The Actions List is the complete functional contract of the system.

Again, the agent produces this artifact. The human validates each Domain and each Action before the process continues. If the agent discovers a case not covered by any existing ADR, it does not resolve it autonomously: it surfaces the gap and the process returns to the Decision Layer to produce a new ADR before proceeding.

The Actions List eliminates the redundancy problem. Every unit of business logic is declared once, with its contract. The agent does not regenerate: it implements what is declared.

### The Execution Layer eliminates ambiguity of implementation

Before a single line of code is written, the agent produces the Steering Files: a set of structured documents that together constitute the complete operational context for code generation. Stack, database schema, security model, API contracts, UI specifications, integration dependencies: all declared explicitly.

The agent then generates code Action by Action, in dependency order, with the Steering Files as its unambiguous reference. If a Steering File says something, it is not inferred: it is law. Architectural coherence is maintained not by discipline but by the structure of the input the agent receives.

---

## How artifacts are produced

This point deserves to be stated directly, because it changes how the method is perceived.

The artifacts of ABD — ADRs, the Actions List, the Steering Files — are not written manually. They are generated by the coding agent and validated by the human.

Without this understanding, ABD looks like a documentation-heavy methodology that asks the team to produce large volumes of structured text before writing any code. That reading is incorrect and leads to abandonment.

With this understanding, the picture is different. The human's role is to make decisions and validate their formalization. The agent's role is to analyze, draft, structure, and expand. The effort that would otherwise go into writing documentation goes instead into thinking and deciding. The documentation is a byproduct of that process, not a prerequisite the team must produce from scratch.

In practice, the coding agent generates 70 to 80 percent of each artifact. The human's contribution is the judgment that the artifact correctly captures the decision, and the authority to ratify or reject it.

---

## A note on analysts

In a project developed with ABD, analysts interact with the method primarily through the Decision Layer. They do not need to access the development repository or work directly with the coding agent.

In practice, what changes is not the analyst's role but the form of their output. An analyst working with ABD guides the LLM through the domain analysis and the architectural decision process, and validates the ADRs that come out of those sessions. The analytical work — understanding the domain, identifying constraints, evaluating options — remains entirely theirs. What the agent handles is the formalization of that work into structured artifacts.

For analysts accustomed to producing documentation from scratch, this is a meaningful shift in how time is spent. The thinking stays with the analyst. The writing moves to the agent.

---

## Common misuse

ABD is not suitable for the following uses:

**Manually writing artifacts from scratch.** If the team writes ADRs, Actions Lists, or Steering Files by hand without agent assistance, the process becomes significantly heavier than it needs to be. The method is designed around agent generation and human validation. Reversing that ratio defeats the purpose.

**Replacing analysis with generated artifacts.** The agent formalizes decisions; it does not make them. A project that skips genuine domain analysis and expects the agent to infer the domain from vague inputs will produce well-structured artifacts that reflect poorly understood requirements. ABD amplifies the quality of the analysis it receives. It does not compensate for analysis that was not done.

**Applying ABD to small or trivial projects.** The structure ABD introduces has a cost that is justified by the complexity it manages. On a simple, short-lived project, that cost may exceed the benefit. ABD is designed for projects where noise, contradiction, and coherence degradation are real risks.
