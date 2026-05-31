# Stack Config Definition

## Document type

This is ABD system documentation. It defines the structure, fields and
compilation rules of the `stack-config.md` Steering File. It is not a project
artifact: it lives in the public ABD documentation repository and is updated
only when the ABD method evolves.

The file it describes, `stack-config.md`, is a project artifact. It lives in
the project repository and is compiled once per project.

---

## Purpose

`stack-config.md` is the Steering File that records the technological
foundations of a project: the stable, mutually constraining choices that hold
the project together for its whole duration.

It does not record third party libraries that cover discrete capabilities.
Those belong to `integration-config.md`, the Integration Level registry.

`stack-config.md` answers a single question: what holds this project up.

---

## Location of the compiled file

The compiled `stack-config.md` lives in the project repository, in the
Steering Files directory:

```
abd/steering/stack-config.md
```

It is not placed under `abd/integration/`. The technological foundations are
kept separate from the Integration Level, and `abd/integration/` is reserved
for Integration Level code. Steering Files are not code.

---

## Structure of stack-config.md

The file has three parts, in this order:

1. Traceability header
2. Foundations table
3. abd/ structure section

A single table is not enough. The three parts have different shapes and
forcing them into one table makes each one harder to read.

---

## Part 1: Traceability header

Every Steering File must carry an explicit reference to the ratified decisions
and the Actions List version it derives from, plus the date of its last
validation. The header is a fixed block at the top of the file.

Required fields:

- Source decisions: the ratified ADRs the foundations derive from, primarily
  the technical constraints decided in the Decision Layer
- Actions List version: the validated Actions List version this file was
  derived against
- Last validation date: the date the human last validated the file
- Status: Draft or Validated

The file is not considered usable by Claude Code until Status is Validated.

---

## Part 2: Foundations table

The foundations are recorded in a table with three columns.

| Column | Content |
|---|---|
| Field | The name of the foundation field |
| Value | The chosen value for the project |
| Source | The ratified decision the value derives from |

The Source column is not decoration. ABD requires that Steering Files derive
from ratified decisions, not from invention. If the Source column cannot be
filled for a field, the corresponding technical constraint is missing. That is
a gap to close in the Decision Layer, not here.

### Foundation fields

| Field | What goes in it | What does not go in it |
|---|---|---|
| Programming language | The language the project is written in | Library or framework names |
| Runtime | The runtime and its version | Build or package tooling |
| Application framework | The application framework and its version | Specialized libraries for single capabilities |
| Platform and execution environment | Cloud provider, on premise, serverless, containerized, web, mobile, plus the dev, staging and production environments | Deployment scripts or pipeline detail |
| Persistence engine | The database engine and its persistence model | Schema detail, which belongs to the database schema Steering File |
| Base UI framework or library | The base UI framework or library that shapes the whole interface | Specialized UI libraries for a single capability, which belong to integration-config.md |
| Test runner | The test runner used to verify the code | Test cases |
| Linter | The linter and style ruleset | |
| Build tool | The build tool | |
| Package manager | The package manager | The packages themselves |

Foundation values are mutually coherent by construction. A framework
constrains the language that supports it: Laravel implies PHP, it cannot sit
on a Python project. When compiling the table, verify that every value is
compatible with the others.

---

## Part 3: abd/ structure section

The Implementation Levels and the `abd/` directory structure are a method
standard. This section reproduces the structure as a directory tree, not as a
table row. A tree inside a table cell is unreadable.

The recommended structure, placed at the project root alongside the native
framework structure:

```
abd/
├── actions/
│   └── <domain>/
├── service/
└── integration/
```

The section states that Claude Code generates code respecting the three
Implementation Levels, that Actions call only Service Level methods, and that
the Service Level delegates external dependencies to the Integration Level.

---

## The boundary with integration-config.md

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

- The file is compiled during Execution Layer Setup, before code generation
  starts
- Every Source entry must point to a ratified decision
- The human validates the file before Claude Code starts code generation
- Once validated, the foundations stay stable for the whole project
- A change to the foundations requires re-evaluating the technical constraints
  in the Decision Layer and re-running the Decision Layer exit gate

---

## Template skeleton

The following is the empty skeleton to copy into the project repository as
`abd/steering/stack-config.md`. Replace every bracketed placeholder with the
project value. In a project the Source column holds the specific ADR that
ratifies each value.

```
# Stack Config

## Traceability

- Source decisions: [list of ratified ADRs]
- Actions List version: [version]
- Last validation date: [YYYY-MM-DD]
- Status: Draft

## Foundations

| Field | Value | Source |
|---|---|---|
| Programming language | [value] | [ratified decision] |
| Runtime | [value] | [ratified decision] |
| Application framework | [value] | [ratified decision] |
| Platform and execution environment | [value] | [ratified decision] |
| Persistence engine | [value] | [ratified decision] |
| Base UI framework or library | [value] | [ratified decision] |
| Test runner | [value] | [ratified decision] |
| Linter | [value] | [ratified decision] |
| Build tool | [value] | [ratified decision] |
| Package manager | [value] | [ratified decision] |

## abd/ structure

The project adopts the standard Implementation Levels.

abd/
├── actions/
│   └── <domain>/
├── service/
└── integration/

Rules:
- Actions in the Action Level call only Service Level methods
- Service Level methods contain no direct external dependencies
- External dependencies are delegated to the Integration Level
- Claude Code warns the developer when it detects a level violation
```
