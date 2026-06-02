# Integration Config Definition

## Document type

This is ABD system documentation. It defines the structure, fields and
compilation rules of the `integration-config.md` Steering File. It is not a
project artifact: it lives in the public ABD documentation repository and is
updated only when the ABD method evolves.

The file it describes, `integration-config.md`, is a project artifact. It
lives in the project repository.

---

## Purpose

`integration-config.md` is the Steering File that records the external
dependencies of a project: the third party libraries, external APIs,
authentication services and document generators that the Integration Level
wraps.

It does not record the technological foundations of the project. Language,
runtime, framework, platform and persistence engine belong to
`stack-config.md`.

`integration-config.md` answers a single question: what the project uses along
the way.

---

## Location of the compiled file

The compiled `integration-config.md` lives in the project repository, in the
Steering Files directory:

```
abd/steering/integration-config.md
```

It sits alongside `stack-config.md`. The two files are kept separate because
they have opposite lifecycles: the foundations are decided once, the external
dependencies accumulate.

---

## Why this file exists

The Integration Level isolates external dependencies from the Service Level.
If a library is replaced, the change stays localized in its wrapper. But the
Integration Level on its own is code: nothing declares, in a verifiable form,
which external dependencies the project has adopted and why.

`integration-config.md` is that declaration. It is the explicit, traceable
registry of the Integration Level.

It also enforces consistency. Without a registry, the coding agent can adopt one
library for a capability in one Action and a different library for the same
capability in another Action, producing two wrappers for the same function.
The registry makes one library per capability a verifiable rule.

---

## Structure of integration-config.md

The file has two parts, in this order:

1. Traceability header
2. Dependencies registry

---

## Part 1: Traceability header

The header is a fixed block at the top of the file.

Required fields:

- Source decisions: the ratified ADRs the dependencies derive from
- Actions List version: the validated Actions List version the file was
  initialized against
- Last update date: the date of the most recent validated addition
- Status: Initialized or Active

Unlike other Steering Files, this header does not record a single validation
date. The file grows over time, so each entry carries its own validation date
in the registry.

---

## Part 2: Dependencies registry

External dependencies are recorded in a table.

| Column | Content |
|---|---|
| Capability | The discrete operation the dependency covers |
| Library or service | The third party library, API or service adopted |
| Wrapper | The Integration Level wrapper that isolates the dependency |
| Used by Actions | The Actions that use this dependency |
| Date validated | The date the human validated this entry |

### Rules for the registry

- One library per capability. Every Action that performs the same operation
  uses the same dependency. If QR code reading is covered by library A in one
  Action, it is covered by library A in every Action.
- Before introducing a library for a capability already in the registry,
  the coding agent adopts the dependency already recorded. It does not add a second
  one.
- The Used by Actions column makes consistency verifiable: it shows, for each
  dependency, every Action that depends on it.
- A library compatible with the project foundations only. A PHP library cannot
  enter a Python project. The dependency must be available for the language
  recorded in `stack-config.md`.

---

## Incremental lifecycle

This is the only Steering File that is not fully compiled during Execution
Layer Setup.

External dependencies emerge as Actions are implemented. A QR reader is needed
only when an Action that reads QR codes is built. The file is therefore:

- Initialized during Execution Layer Setup with the dependencies already known
  from the Actions List
- Updated incrementally during the Execution Layer, as new capabilities surface

Every addition is a change to a Steering File. It requires explicit human
validation, consistent with the iterative and supervised nature of the
Execution Layer. The coding agent does not adopt a third party library until its
registry entry has been validated by the human.

The AGENTS.md references this file. Incremental additions to
`integration-config.md` do not require updating the AGENTS.md: the reference is
to the file, not to its content.

---

## The boundary with stack-config.md

The single most common compilation mistake is placing a dependency in the
wrong file. The rule is fixed and must not be left to judgment:

- A choice that constrains everything else and is decided once belongs to
  `stack-config.md`
- A choice that covers a discrete capability and accumulates as Actions are
  implemented belongs to `integration-config.md`

A base UI framework belongs to `stack-config.md`. A specialized UI library for
a single capability belongs to `integration-config.md`. A QR code reader, a
document generator, an OAuth provider belong to `integration-config.md`.

---

## Validation rules

- The file is initialized during Execution Layer Setup with the dependencies
  known from the Actions List
- The file is updated incrementally during the Execution Layer
- Every addition requires explicit human validation before the coding agent adopts
  the library
- One library is adopted per capability
- Every entry carries the Actions that use it, keeping consistency verifiable

---

## When the file is not required

A project with no external dependencies does not need
`integration-config.md`. The file is required only when the project adopts
external dependencies.

---

## Template skeleton

The following is the empty skeleton to copy into the project repository as
`abd/steering/integration-config.md`. Replace every bracketed placeholder with
the project value. In a project the Source column holds the specific ADR that
ratifies each dependency.

```
# Integration Config

## Traceability

- Source decisions: [list of ratified ADRs]
- Actions List version: [version]
- Last update date: [YYYY-MM-DD]
- Status: Initialized

## Dependencies registry

| Capability | Library or service | Wrapper | Used by Actions | Date validated |
|---|---|---|---|---|
| [capability] | [library or service] | [wrapper name] | [actions] | [YYYY-MM-DD] |

Rules:
- One library per capability
- Every Action performing the same operation uses the same dependency
- A new dependency is added only after explicit human validation
- The coding agent adopts a dependency already in the registry before adding a new
  one for the same capability
```
