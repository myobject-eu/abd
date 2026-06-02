# action-dependencies-definition.md

## Purpose

This document defines the template for the `action-dependencies.md` Steering File in ABD projects.

`action-dependencies.md` declares the dependencies between Actions using an adjacency list format. It serves two purposes: it maps the relationships between Actions across the entire system, and it provides the coding agent with an explicit development plan derived from the topological ordering of those relationships.

The coding agent reads `action-dependencies.md` before starting the development of any Action. It uses the file to determine which Actions are immediately developable, which are blocked by hard dependencies, and which order to follow across the development session.

---

## Dependency classification

Dependencies between Actions are classified as **hard** or **soft**.

A dependency is **hard** when an Action cannot be correctly implemented without the dependency existing in the system. Hard dependencies are blocking: a dependent Action is not developed until all its hard dependencies are completed.

A dependency is **soft** when an Action benefits from the presence of the dependency but can be implemented without it. Soft dependencies are not blocking: they signal a preferred relationship that the coding agent takes into account but that does not prevent development.

---

## Domain as informational attribute

Dependencies are transversal across domains. An Action can depend on any other Action regardless of its domain of origin. The domain is declared as an informational attribute next to the Action name to make cross-domain coupling visible, not to constrain dependencies.

---

## Topological ordering

Actions are grouped in the file according to topological sort order. This is the standard algorithm used by package managers and build systems to determine execution order in a directed acyclic graph. The coding agent reads the group order as a development plan and proceeds from top to bottom.

| Group | Description | Developable |
|---|---|---|
| 1 | Actions with no dependencies | Immediately, in any order |
| 2 | Actions with soft dependencies only | Soon, soft dependencies are not blocking |
| 3 | Actions with completed hard dependencies | As soon as their hard dependencies are done |
| 4 | Actions with pending hard dependencies | Blocked, to be planned after group 3 |

A circular hard dependency is a design error. The coding agent flags it and does not proceed until it is resolved.

---

## Template

```markdown
# action-dependencies

## Group 1 -- No dependencies

[ActionName] ([Domain])
  *(no dependencies)*

[ActionName] ([Domain])
  *(no dependencies)*

---

## Group 2 -- Soft dependencies only

[ActionName] ([Domain])
  soft: [ActionName], [ActionName]

---

## Group 3 -- Hard dependencies completed

[ActionName] ([Domain])
  hard: [ActionName], [ActionName]
  soft: [ActionName]

---

## Group 4 -- Hard dependencies pending

[ActionName] ([Domain])
  hard: [ActionName], [ActionName]
```

---

## Notation rules

- Each Action is declared on its own line with its domain in parentheses
- `hard:` lists Actions that must exist before this Action can be developed
- `soft:` lists Actions whose presence is beneficial but not blocking
- If an Action has no dependencies of a given type, that line is omitted
- Dependencies reference the exact Action name as declared in `actions-list.md`
- Groups with no Actions are omitted from the file

---

## Criteria for classifying hard vs soft

A dependency is hard when at least one of the following conditions applies:

- The Action calls a method or function defined in the dependency
- The Action reads or writes data produced or managed by the dependency
- The Action cannot be tested in isolation without the dependency being implemented

A dependency is soft when:

- The Action is more complete or consistent with the dependency present, but a meaningful implementation exists without it
- The dependency provides optional enrichment of the data the Action works with
- The two Actions share a domain concept but do not exchange data or control flow directly

When in doubt, classify as hard. A hard dependency that turns out to be soft is easier to relax than a soft dependency that turns out to be blocking.

---

## Compilation rules

- `action-dependencies.md` is compiled during the Execution Layer Setup after `actions-list.md` is validated
- Every Action declared in `actions-list.md` must appear in `action-dependencies.md`
- Actions not present in `actions-list.md` must not appear in `action-dependencies.md`
- The file is updated whenever `actions-list.md` is modified: new Actions are added, removed Actions are deleted, and the topological ordering is recalculated
- The coding agent verifies consistency between `action-dependencies.md` and `actions-list.md` before starting development of any Action

---

## Relationship with other Steering Files

| Steering File | Relationship |
|---|---|
| `actions-list.md` | The authoritative source of Action names and domains referenced in this file |
| `security-model.md` | Authorization dependencies between Actions may surface as hard dependencies |
| `database-schema.md` | Actions that read or write shared entities often produce hard dependencies between them |
| `binding-contract.md` | The controller-to-Action binding reflects the execution order implied by the dependency groups |

