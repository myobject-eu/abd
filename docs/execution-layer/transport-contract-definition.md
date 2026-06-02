# Transport Contract Definition

## Document type

This is ABD system documentation. It defines the structure and compilation
rules of the `transport-contract.md` Steering File. It is not a project
artifact: it lives in the public ABD documentation repository and is updated
only when the ABD method evolves.

The file it describes, `transport-contract.md`, is a project artifact. It
lives in the project repository.

---

## Purpose

`transport-contract.md` is the Steering File that records the network
transport contract of a project: the HTTP endpoints through which a UI or an
external application communicates with the Action Layer.

It describes a transport contract. It is not an in process binding. When the
UI is rendered by the backend in the same process, there is no network and no
endpoint: that case belongs to `binding-contract.md`.

---

## When the file is required

`transport-contract.md` is required only when the project exposes HTTP
endpoints. This covers three cases:

- A backend consumed by a separate UI, for example a mobile application
- A monolith with server side rendering that also exposes endpoints, for
  example to feed a server side datatable
- A backend that exposes endpoints to third party applications

A monolith with server side rendering and no HTTP endpoints does not need this
file.

---

## Location of the compiled file

The compiled `transport-contract.md` lives in the project repository, in the
Steering Files directory:

```
abd/steering/transport-contract.md
```

It sits alongside the other Steering Files.

---

## Relationship with other Steering Files

- `stack-config.md` records the platform and execution environment. The
  transport contract assumes the platform is already decided there.
- `binding-contract.md` describes the controller to Action to view binding in
  the monolith. The two files are complementary: a mixed project uses both.
- The UI specification decides UI behavior. When the UI specification selects
  a server side datatable, that decision generates an endpoint here. The
  datatable mode is decided in the UI specification, the endpoint that serves
  it is declared in this file.

---

## The ABD rule the file enforces

Every endpoint maps to an Action. An endpoint is transport: it receives a
request, invokes an Action and returns the result. An endpoint contains no
application logic. The logic lives in the Action.

An endpoint that does not name the Action it invokes is incomplete.

---

## Structure of transport-contract.md

The file has two parts, in this order:

1. Traceability header
2. Endpoints registry

---

## Part 1: Traceability header

The header is a fixed block at the top of the file.

Required fields:

- Source decisions: the ratified ADRs the contract derives from
- Actions List version: the validated Actions List version the contract was
  derived against
- Last validation date: the date the human last validated the file
- Status: Draft or Validated

The file is not considered usable by the coding agent until Status is Validated.

---

## Part 2: Endpoints registry

Each endpoint is declared as its own block, not as a single table row. An
endpoint carries a request shape and a response shape that do not fit a cell.

Each endpoint block has a properties table and two payload sections.

### Endpoint properties

| Property | Content |
|---|---|
| Path | The endpoint path, for example `/api/customers` |
| Verb | The HTTP verb: GET, POST, PUT, PATCH, DELETE |
| Visibility | `public` or `private` |
| Authentication | The authentication scheme required for this endpoint |
| Action | The Action the endpoint invokes |

### Visibility attribute

Every endpoint declares a visibility attribute. Visibility is not a separate
file and not a category: it is a property of each endpoint.

- `private`: the endpoint is reserved for the application UI, for example the
  endpoints that feed a server side datatable or the endpoints consumed by a
  separate mobile UI
- `public`: the endpoint is open to third party applications

The authentication scheme follows from the visibility. A private endpoint and
a public endpoint do not share the same authentication.

### Request and response

Each endpoint block describes:

- Request: the parameters and the request payload, as a list of fields
- Response: the response payload, as a list of fields, and the relevant status
  outcomes

Field types in payloads use the neutral type vocabulary where they correspond
to schema data, for consistency with `database-schema.md`.

---

## Validation rules

- The file is compiled during Execution Layer Setup, before code generation
  starts
- Every endpoint names the Action it invokes
- Every endpoint declares a visibility attribute and an authentication scheme
- An endpoint contains no application logic
- The human validates the file before the coding agent starts code generation
- When the UI specification selects a server side datatable, the corresponding
  endpoint is declared here

---

## Template skeleton

The following is the empty skeleton to copy into the project repository as
`abd/steering/transport-contract.md`. Replace every bracketed placeholder with
the project value. In a project the Source column holds the specific ADR that
ratifies the contract.

```
# Transport Contract

## Traceability

- Source decisions: [list of ratified ADRs]
- Actions List version: [version]
- Last validation date: [YYYY-MM-DD]
- Status: Draft

## Endpoints

### [endpoint name]

| Property | Value |
|---|---|
| Path | [/path] |
| Verb | [GET | POST | PUT | PATCH | DELETE] |
| Visibility | [public | private] |
| Authentication | [authentication scheme] |
| Action | [action invoked] |

Request:
- [field name] [neutral type] [required or optional]

Response:
- [field name] [neutral type]
- Outcomes: [relevant status outcomes]

Rules:
- Every endpoint invokes an Action and contains no application logic
- Every endpoint declares a visibility attribute
- A private endpoint and a public endpoint use different authentication
```
