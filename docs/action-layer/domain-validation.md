# Domain Validation

Domain Validation is the first gate of the Action Layer. Before any Action is defined, every Domain proposed by the LLM must be explicitly validated by the human.

This step exists because Domains are the structural foundation of the Actions List. A Domain defined incorrectly propagates its problems through every Action that belongs to it, and those problems compound through the Execution Layer until they surface as architectural contradictions in the generated code.

---

## What a Valid Domain Looks Like

A Domain is valid when it satisfies all of the following conditions.

**Responsibility is defined in business terms, not technical terms.**
A Domain groups behavior that belongs together for business reasons. "Order Management", "User Identity", "Billing", and "Notifications" are Domains. "Controllers", "Repositories", "Services", and "Database" are not Domains: they are technical layers. A Domain defined on technical criteria produces Actions that mirror the technology stack instead of the business model, and the Actions List becomes a technical specification rather than a business contract.

**The boundary is explicit.**
A valid Domain has a declared boundary: what behavior belongs to it and, where relevant, what behavior is explicitly excluded. A Domain without a declared boundary will absorb ambiguous cases during Action definition, producing an Actions List where responsibilities overlap without anyone having decided where the line is.

**Every relevant use case from the Domain Analysis can be assigned to exactly one Domain.**
If a use case cannot be assigned to any Domain, the Domain map has a gap. If a use case can be assigned to more than one Domain with equal justification, the boundaries overlap and at least one Domain needs to be redefined. The LLM verifies this assignment for every use case before Domain Validation is considered complete.

**The Domain does not contain technical orchestration behavior.**
Actions like "initialize the database connection", "configure the middleware stack", or "route the HTTP request" do not belong to any Domain. They are infrastructure concerns, not business behavior. A Domain that contains them will produce Actions that cannot be tested independently of the execution environment, which violates the ADEXMO principle of interface independence.

**The Domain is cohesive.**
The behaviors grouped in a Domain belong together because they share the same business purpose, not because they happen to be implemented in the same module or accessed by the same actor. If two behaviors in the same Domain would never be modified for the same reason, they probably belong in separate Domains.

---

## What the LLM Proposes and Why

The LLM derives Domain candidates from the Domain Analysis ADR. Specifically, it reads:

- The system domains declared in the Domain Analysis ADR
- The high-level use cases and their assigned areas of responsibility
- The actors and their distinct needs

The LLM presents each candidate Domain with:

- **Name**: a noun phrase that names the business area, not a technical layer
- **Responsibility**: one or two sentences that describe what behavior belongs to this Domain
- **Boundary note**: what is explicitly excluded, where the boundary with an adjacent Domain is not obvious
- **Use cases covered**: the use cases from the Domain Analysis that this Domain encompasses

The human validates, modifies, or rejects each Domain before the LLM proceeds to Action definition.

---

## Common Problems and How to Resolve Them

**The Domain mirrors a technical layer.**
Rename and redefine. Ask what business purpose the behaviors in this Domain serve. If "Service" contains order creation, order cancellation, and order status retrieval, the Domain is "Order Management" and the word "Service" disappears.

**Two Domains share responsibility for the same use case.**
Define the boundary explicitly. Decide which Domain owns the operation and which Domain, if any, is called upon by the first. The dependency becomes explicit at the Action level, not at the Domain level.

**A use case does not fit any Domain.**
Either a Domain is missing, or the use case is out of scope. The LLM surfaces this as a gap and the human decides: add a Domain, extend the boundary of an existing Domain, or declare the use case out of scope with an explicit rationale.

**A Domain is too broad.**
A Domain that contains more than ten to fifteen candidate Actions is likely covering more than one business area. The LLM signals this when it produces the Action candidates for a Domain. The Domain should be split at the business boundary, not at a technical one.

**A Domain is too narrow.**
A Domain with a single Action is not necessarily wrong, but it warrants scrutiny. The LLM asks whether the behavior belongs to an adjacent Domain or whether it is genuinely a standalone business area.

---

## The LLM's Role During Validation

The LLM does not passively present Domain candidates and wait. It actively evaluates each candidate against the criteria above before proposing it.

If the LLM identifies a candidate that does not satisfy the criteria, it flags the problem and proposes a corrected version rather than presenting a candidate it considers invalid.

During the validation conversation, the LLM:

- Challenges Domain definitions that use technical terminology
- Verifies that every use case from the Domain Analysis is covered
- Surfaces overlapping boundaries before the human ratifies the Domain map
- Signals when a Domain appears too broad or too narrow

The LLM does not proceed to Action definition for any Domain until the human has explicitly ratified it.

---

## Output

The output of Domain Validation is a ratified Domain map: the complete list of Domains with their names, responsibility descriptions, and boundary notes, confirmed by the human before Action definition begins.

This Domain map is the reference for the entire Action Layer. If a later Action cannot be assigned to any Domain, or if its assignment is ambiguous, the Domain map has a gap that must be resolved before the Action is defined. The process returns to Domain Validation, not to the Domain Analysis ADR, unless the gap reveals a missing area of business responsibility that was not identified during Domain Analysis.
