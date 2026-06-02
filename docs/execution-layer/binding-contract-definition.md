# Binding Contract Definition

## Document type

This is ABD system documentation. It defines the structure and compilation
rules of the `binding-contract.md` Steering File. It is not a project
artifact: it lives in the public ABD documentation repository and is updated
only when the ABD method evolves.

The file it describes, `binding-contract.md`, is a project artifact. It lives
in the project repository.

---

## Purpose

`binding-contract.md` is the Steering File that records the in process binding
of a monolith with server side rendering: how controllers bridge requests to
Actions, and how views receive the data the Actions produce.

It describes an in process binding. It does not describe HTTP endpoints. When
there is a network between the UI and the Action Layer, that case belongs to
`transport-contract.md`.

---

## When the file is required

`binding-contract.md` is required only when the project adopts server side
rendering, for example a Laravel application with Blade views.

A backend with a fully separate UI and no server side rendering does not need
this file.

---

## Location of the compiled file

The compiled `binding-contract.md` lives in the project repository, in the
Steering Files directory:

```
abd/steering/binding-contract.md
```

It sits alongside the other Steering Files.

---

## Relationship with other Steering Files

- `transport-contract.md` describes HTTP endpoints. The two files are
  complementary: a monolith that also exposes endpoints uses both.
- The UI specification decides UI behavior. When the UI specification selects
  a datatable with client side pagination, the controller passes all records
  to the view and the binding stays entirely in this file. When it selects a
  server side datatable, the data is served by an endpoint declared in
  `transport-contract.md` and this file describes only the rendering of the
  control.

---

## The ABD rule the file enforces

The controller is boundary code, not application code. It is the permanent
adaptation layer between the framework and the Action Layer. A controller
method receives a request, invokes an Action and passes the result to a view.
It contains no application logic. The logic lives in the Action.

A controller that contains queries, business rules or validation logic is a
violation. Those belong to the Action.

---

## Structure of binding-contract.md

The file has two parts, in this order:

1. Traceability header
2. Binding registry

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

## Part 2: Binding registry

Each binding is declared as its own block. A binding carries an input mapping
and a view data shape that do not fit a single table cell.

Each binding block has a properties table and two mapping sections.

### Binding properties

| Property | Content |
|---|---|
| Route | The route the controller method handles |
| Verb | The HTTP verb of the route: GET, POST |
| Action | The Action the controller method invokes |
| View | The view template that renders the result |

### Input and output

Each binding block describes:

- Input mapping: how request data, for example form fields, maps to the
  parameters the Action expects
- View data: what data the view receives from the Action result

A binding handles one of two common cases:

- Data display: the controller invokes an Action that returns data, and passes
  it to the view
- Form submission: the controller maps the submitted fields to an Action,
  invokes it, and renders the outcome

---

## Datatable binding

The datatable mode is decided in the UI specification, not here. Its
consequence for this file depends on the mode:

- Client side pagination: the controller invokes an Action that returns all
  records and passes them to the view. The binding is entirely described here.
- Server side datatable: this file describes only the binding that renders the
  datatable control. The rows are served by an endpoint declared in
  `transport-contract.md`. The binding entry references that endpoint.

---

## Validation rules

- The file is compiled during Execution Layer Setup, before code generation
  starts
- Every binding names the Action the controller invokes
- A controller contains no application logic
- The human validates the file before the coding agent starts code generation
- When the UI specification selects a server side datatable, the binding
  references the endpoint declared in the transport contract

---

## Template skeleton

The following is the empty skeleton to copy into the project repository as
`abd/steering/binding-contract.md`. Replace every bracketed placeholder with
the project value. In a project the Source column holds the specific ADR that
ratifies the contract.

```
# Binding Contract

## Traceability

- Source decisions: [list of ratified ADRs]
- Actions List version: [version]
- Last validation date: [YYYY-MM-DD]
- Status: Draft

## Bindings

### [binding name]

| Property | Value |
|---|---|
| Route | [/route] |
| Verb | [GET | POST] |
| Action | [action invoked] |
| View | [view template] |

Input mapping:
- [request field] maps to [action parameter]

View data:
- [field the view receives]

Rules:
- The controller invokes an Action and contains no application logic
- A server side datatable references its endpoint in the transport contract
```
