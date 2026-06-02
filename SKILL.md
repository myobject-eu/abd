---
name: abd
description: Action-Based Development (ABD) is a structured methodology for AI-assisted software development. Use this skill whenever you are operating on a project that follows ABD, or when the user asks you to apply ABD to a new project. Covers all three layers: Decision Layer (ADR-based decision tracking), Action Layer (ADEXMO Actions List production), and Execution Layer (supervised code generation). Trigger on phrases like "follow ABD", "apply ABD", "we're using ABD", "start the Decision Layer", "produce the Actions List", "start the Execution Layer", or any reference to ADRs, Actions List, Steering Files, or AGENTS.md in a project context.
---

# Action-Based Development (ABD)

ABD structures AI-assisted software development into three sequential layers. Each layer produces a mandatory artifact that becomes the input for the next. The human is the decision-maker and supervisor at every layer; the LLM proposes, the human ratifies.

Full documentation and ADR repository: https://github.com/myobject-eu/abd

---

## Layers Overview

```
Decision Layer  →  Action Layer  →  Execution Layer
    (ADRs)        (Actions List)       (Code)
```

The sequence is enforced. No layer starts before the previous one is complete and gated.

---

## Decision Layer

**Purpose:** Capture every architectural and process decision as a permanent, versioned artifact.

**Artifact:** ADR files (`ADR-NNNN-title.md`), numbered progressively, stored in the project repository.

**Process:**
1. The human expresses intent or raises a problem.
2. The LLM proposes options with trade-offs.
3. The human selects and ratifies. The LLM produces the ADR.
4. Every ADR includes at minimum: Context, Problem, Decision, Consequences.
5. Superseded ADRs are marked "Sostituito" with a reference to the replacing ADR.

**Gate before Action Layer (Exit Checklist — ADR-0006):**

Before any Action Layer work begins, verify all conditions are met:
- ADR of Domain Analysis ratified (ADR-0005 equivalent)
- System domains defined with explicit boundaries
- All known technical constraints ratified (ADR-0007 equivalent)
- No open ADRs in draft state blocking architectural decisions
- Human has explicitly confirmed readiness to proceed

If any condition is unmet, block the transition and report the gaps. The gate is not bypassable.

---

## Action Layer

**Purpose:** Translate ratified ADRs into a vendor-neutral, interface-independent contract: the Actions List.

**Artifact:** `actions-list.md` — the executable contract of the system, produced via ADEXMO.

**Tool:** ADEXMO (Action-Driven Execution Model). See: https://github.com/myobject-eu/adexmo

**Process (ADR-0009):**

1. **Read ADRs** — Read all ratified ADRs. Identify candidate Domains from responsibility areas. Do not proceed without completing this read.
2. **Validate Domains** — Propose candidate Domains with boundaries and ADR references. Wait for human validation of each Domain before defining any Action.
3. **Define Actions per Domain** — For each validated Domain, propose Actions with input, output, and business rules derived from ADRs. Wait for human validation of each Action before proceeding.
4. **Gap handling (Phase 3.5)** — If a case is found that no existing ADR covers and cannot be derived by logic from ratified ADRs: stop, signal the gap to the human as an ADR candidate, return to the Decision Layer. Do not resolve gaps autonomously. Resume from Phase 3 on the current Domain after the new ADR is ratified.
5. **Produce Actions List** — Only after all Domains and Actions are validated, produce the complete ADEXMO-conformant Actions List. Version it in the repository.

**Ping-pong rule:** The Action Layer can return to the Decision Layer at any point when gaps are discovered. Each return produces an explicit ADR. The Exit Checklist gate is re-executed after any significant ADR change before resuming the Action Layer.

---

## Execution Layer

**Purpose:** Generate working, tested, human-supervised code from the Actions List.

**Tool:** Any coding agent with terminal access (Claude Code, Codex CLI, Gemini CLI, etc.).

**Setup phase (ADR-0010, ADR-0013):**

Before any code generation, produce and validate the Steering Files:

| File | When required |
|---|---|
| `AGENTS.md` | Always — Project Agent Instruction File, references all Steering Files |
| `actions-list.md` | Always |
| `stack-config.md` | Always — language, runtime, framework, platform, tooling |
| `integration-config.md` | If the project uses external dependencies |
| `database-schema.md` | If the project uses persistence |
| `api-spec.md` | If the project exposes APIs |
| `ui-spec.md` | If the project has a UI |
| `security-model.md` | If the project has user management |
| `action-dependencies.md` | If dependencies exist between Actions |

Vendor-specific redirect files (`CLAUDE.md`, `GEMINI.md`, etc.) contain a single line: `@AGENTS.md`

Every Steering File is validated by the human before code generation begins.

**Generation process (ADR-0011):**

1. **Read Steering Files** — Read `AGENTS.md` and all referenced files. Read `action-dependencies.md` for implementation sequence. Do not begin implementation before completing this read.
2. **Implement one Action at a time** — Follow the sequence in `action-dependencies.md`. For each Action: implement, run tests via CLI, wait for human approval before proceeding to the next.
3. **Test failures** — Iterate autonomously up to three attempts. After three failed attempts, stop and report: failure description, attempts made, hypothesis on cause. The human decides whether the problem is implementative or requires a contract revision.
4. **Non-implementable Actions** — If an Action cannot be implemented as specified in the contract, do not make assumptions and do not modify the contract. Stop, signal the problem to the human, return to the Action Layer for contract revision. Resume from the current Action after the revised Actions List is received.
5. **Approval and commit** — For each successfully implemented and tested Action: human reviews, approves or requests changes, commits, marks the Action as implemented in the Actions List with a reference to the commit.

**Ping-pong rule:** The Execution Layer can return to the Action Layer at any point when Actions are found non-implementable as defined. The human decides whether the revision requires a new ADR or is a minor contract correction.

---

## Key Principles

- The Action is the fundamental unit of ABD. It carries its ADR (why), its contract (what), and its implementation (how).
- All artifacts are plain text files, versioned with Git, independent of any vendor or tool.
- The human is the decision-maker at every layer. The LLM proposes; the human ratifies.
- No layer produces its artifact before the previous layer's gate is passed.
- Gaps discovered in any layer are never resolved autonomously. They are escalated to the appropriate layer as explicit decisions.
