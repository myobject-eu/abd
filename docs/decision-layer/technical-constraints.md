# Technical Constraints

Technical Constraints is the mandatory second step of the Decision Layer in ABD. Once the domain has been mapped and the Domain Analysis ADR has been ratified, the project must define the technical boundaries within which every subsequent decision will operate.

Defining technical constraints after the Action Layer has begun is not a shortcut. It is a guarantee of rework.

---

## Purpose

Technical constraints answer the questions that determine whether an Actions List is implementable in the real environment where the system will run:

- Where does the system execute?
- How does it store and retrieve data?
- How does it authenticate, authorize, and protect what it handles?

These are not implementation details. They are the perimeter within which every Action in ADEXMO must operate. A constraint discovered after the Actions List is written forces a revision of the contract. A constraint discovered after the Execution Layer has begun forces a refactoring of the code.

The cost of defining constraints early is a few hours of structured conversation. The cost of defining them late is measured in days of rework.

---

## How It Works

As with Domain Analysis, Technical Constraints are defined through a guided discussion between the human and the LLM. The LLM asks explicit questions across three mandatory areas and does not mark an area as complete until the answers are sufficiently precise to guide implementation decisions.

The results are ratified in one or more dedicated ADRs. The choice of whether to use a single ADR covering all three areas or separate ADRs for each is left to the human, based on the complexity of the project. What is not optional is that all three areas are covered before the Exit Checklist is run.

---

## The Three Mandatory Areas

### 1. Platform

Where the system runs and under what operational conditions.

The LLM asks:
- Where will the application be deployed? Cloud provider, on-premise, serverless, containerized, mobile, web, or a combination?
- Are there constraints on the operating system or runtime environment?
- What are the scalability requirements? Does the system need to handle variable load, and if so, at what scale?
- What availability is required? Is downtime acceptable, and if so, how much?
- How many environments are needed? Development, staging, production — are they identical or do they differ in meaningful ways?

Platform decisions shape the Actions List in ways that are not always visible until late in the Execution Layer. A system designed for a serverless environment has different constraints on execution time, state management, and cold start behavior than one running on a dedicated server. These differences affect how Actions are defined, not just how they are implemented.

### 2. Database and Persistence

How the system stores, retrieves, and manages its data.

The LLM asks:
- What persistence model is appropriate for this system? Relational, document, graph, key-value, or a hybrid?
- Will the database be managed by a cloud provider or self-hosted?
- What are the consistency requirements? Does the system require strong consistency or is eventual consistency acceptable?
- What are the availability requirements for the data layer?
- How will schema changes be managed over time? Is there a migration strategy?
- Are there data retention or archival requirements?

Persistence decisions affect domain boundaries. A system that uses a relational model with strong foreign key constraints has different domain boundary options than one that uses a document store. Defining the persistence model before the Actions List ensures that domain boundaries are drawn with awareness of the data model they imply.

### 3. Security

How the system controls access, protects data, and meets its compliance obligations.

The LLM asks:
- What authentication model will the system use? Username and password, OAuth, SSO, API keys, or a combination?
- What authorization strategy will the system apply? Role-based, attribute-based, or a custom model?
- How sensitive is the data the system handles? Does it include personal data, financial data, health data, or other regulated categories?
- Are there compliance requirements? GDPR, HIPAA, PCI-DSS, or others?
- Are there specific security requirements imposed by the deployment environment or by the client?

Security constraints defined after the Action Layer has produced its contract are almost always expensive to retrofit. Authentication and authorization touch every Action that operates on protected resources. Discovering a compliance requirement in the Execution Layer means revisiting the Actions List, the domain boundaries, and potentially the platform decisions.

---

## Output

The output of this step is one or more ADRs, ratified by the human, that cover all three areas above.

These ADRs become part of the mandatory context that is provided to the coding agent before the Execution Layer begins. The coding agent does not make assumptions about stack and environment when explicit constraints are available. It operates within the defined perimeter.

If a technical constraint changes after the Exit Checklist has been passed, the change requires an explicit ADR documenting the modification and its impact on existing ADRs and the Actions List. The Exit Checklist is then re-executed.

---

## What the LLM Will Not Do

The LLM will not mark the technical constraints as complete until all three areas have been covered with sufficient precision to guide implementation decisions.

Vague answers are not accepted. "We will use a cloud provider" is not a platform decision. "We will use a database" is not a persistence decision. The LLM asks follow-up questions until the constraints are specific enough to be actionable.

If the human cannot answer a constraint question at this stage, that uncertainty is itself recorded in the ADR as a known open point, with a decision on how and when it will be resolved before the Action Layer begins.
