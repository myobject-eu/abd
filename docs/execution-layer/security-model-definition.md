# security-model-definition.md

## Purpose

This document defines the template for the `security-model.md` Steering File in ABD projects.

`security-model.md` declares the application security choices of the project: for each area covered, the adopted system and the library that implements it. The file is produced and validated during the Execution Layer Setup.

The coding agent reads `security-model.md` before generating any code related to identity, authentication, sessions, authorization, access control, or event tracking. The library declared for each area is the one the coding agent uses in the implementation, consistently with `integration-config.md`.

---

## Areas covered

The template covers seven areas of application security. Each area declares the adopted system and the corresponding library.

The first three areas (Identity Management, Authentication, Session Management) are tightly interdependent: the authentication system constrains session management, which in turn depends on how identities are represented. These three areas are compiled together.

The remaining four areas (Authorization, Access Control, Auditing, Accountability) depend on the first three but have more frequent update cycles: roles grow with the project, events to track accumulate as Actions are implemented.

---

## Template

```markdown
# security-model

## Security model

<!-- Insert the chosen model:
- No authentication
- Single user without roles
- Multi-user with fixed roles
- Multi-user with granular permissions
- Multi-tenant with groups -->

**Model:** [chosen model]

---

## Identity Management

**System:** [description of the identity management system, e.g.: native user management, external OAuth provider, LDAP directory]

**Library:** [library or service name and version]

**Notes:** [implementation-relevant details, such as the configured OAuth provider or the user identifier field]

---

## Authentication

**System:** [description of the authentication mechanism, e.g.: username and password with hash, API token, OAuth 2.0, magic link]

**Library:** [library name and version]

**Notes:** [relevant details, such as the adopted hash algorithm or the configured OAuth flow]

---

## Session Management

**System:** [description of session management, e.g.: server-side session with cookie, stateless JWT token, opaque token with refresh]

**Library:** [library name and version]

**Token or session duration:** [configured duration]

**Refresh strategy:** [description of the renewal strategy, if applicable]

**Notes:** [relevant details, such as the revocation strategy or logout handling]

---

## Authorization

**System:** [description of the authorization model, e.g.: RBAC with fixed roles, ABAC with attributes, granular per-resource permissions]

**Library:** [library name and version, or: framework native]

**Defined roles:**

| Role | Description |
|---|---|
| [role name] | [description of the role and its responsibilities] |

**Notes:** [relevant details, such as role hierarchy or default role management]

---

## Access Control

**System:** [description of the access control mechanism, e.g.: per-resource policy, authorization middleware, framework gates and policies]

**Library:** [library name and version, or: framework native]

**Access matrix:**

| Resource or Action | [Role 1] | [Role 2] | [Role N] |
|---|---|---|---|
| [Action or resource name] | [permission] | [permission] | [permission] |

<!-- Allowed permission values: allowed, denied, conditional -->
<!-- Conditional means access depends on a specific condition, to be described in the notes -->

**Notes:** [details on conditional permission conditions or exceptions to the matrix]

---

## Auditing

**System:** [description of the security event logging system, e.g.: application log, dedicated database table, external service]

**Library:** [library name and version, or: framework native]

**Tracked events:**

| Event | Recorded data |
|---|---|
| [event name, e.g.: successful login] | [recorded fields, e.g.: user_id, timestamp, ip] |
| [event name, e.g.: failed login] | [recorded fields] |
| [event name, e.g.: permission change] | [recorded fields] |

**Notes:** [details on log retention, audit trail protection, or regulatory requirements]

---

## Accountability

**System:** [description of the mechanism for attributing actions to users, e.g.: created_by and updated_by fields in tables, change log, partial event sourcing]

**Library:** [library name and version, or: framework native]

**Standard tracking fields:**

| Field | Type | Tables where present |
|---|---|---|
| [field name, e.g.: created_by] | [type, e.g.: bigint FK users] | [table list] |
| [field name, e.g.: updated_by] | [type] | [table list] |
| [field name, e.g.: deleted_by] | [type] | [table list, if soft delete] |

**Notes:** [implementation details, such as the soft delete strategy or system user handling]
```

---

## Compilation rules

- The library declared for each area must be consistent with `integration-config.md`: if the library is not yet present in `integration-config.md`, it must be added at the same time.
- If the native framework covers an area without additional external libraries, the Library field contains the framework name and version.
- The access matrix is updated with every new Action added to the project that introduces a resource or permission not yet covered.
- Auditing events are updated during the Execution Layer with every implemented Action that generates a relevant security event.
- Areas not applicable to the chosen model are omitted. A project without authentication does not compile the Authentication, Session Management, Authorization, or Access Control sections. The Auditing and Accountability sections always apply.

---

## Relationship with other Steering Files

| Steering File | Relationship |
|---|---|
| `stack-config.md` | The declared framework constrains the available libraries for each area |
| `integration-config.md` | Every library declared in `security-model.md` must have a corresponding entry |
| `database-schema.md` | Accountability fields and Auditing tables are derived from the schema |
| `transport-contract.md` | Private authentication endpoints derive from the Authentication and Session Management systems |
| `binding-contract.md` | The controller-to-Action binding includes the authorization middleware declared here |

