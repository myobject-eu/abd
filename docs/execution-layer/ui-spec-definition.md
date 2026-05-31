# UI Spec Definition

## Document type

This is ABD system documentation. It defines the structure and compilation
rules of the `ui-spec.md` Steering File. It is not a project artifact: it
lives in the public ABD documentation repository and is updated only when the
ABD method evolves.

The file it describes, `ui-spec.md`, is a project artifact. It lives in the
project repository.

---

## Purpose

`ui-spec.md` is the Steering File that records the user interface of a
project: how the interface is structured and how it behaves.

It does not record how data reaches the interface. That is binding, and
binding has its own files: `binding-contract.md` for the monolith,
`transport-contract.md` when there is a network. `ui-spec.md` answers what the
user sees and how the interface reacts, not where the data comes from.

---

## Location of the compiled file

The compiled `ui-spec.md` lives in the project repository, in the Steering
Files directory:

```
abd/steering/ui-spec.md
```

It sits alongside the other Steering Files.

---

## Relationship with other Steering Files

`ui-spec.md` decides UI behavior. It does not implement the consequences of
its decisions.

When `ui-spec.md` selects a server side datatable, that decision generates an
endpoint in `transport-contract.md` and an entry in `binding-contract.md`.
`ui-spec.md` declares the choice, the other two files carry its technical
consequence. The decision lives here, the implementation lives there.

---

## Structure of ui-spec.md

The file has three parts, in this order:

1. Traceability header
2. Interface structure
3. Interface behavior

---

## Part 1: Traceability header

The header is a fixed block at the top of the file.

Required fields:

- Source decisions: the ratified ADRs the specification derives from
- Actions List version: the validated Actions List version the specification
  was derived against
- Last validation date: the date the human last validated the file
- Status: Draft or Validated

The file is not considered usable by Claude Code until Status is Validated.

---

## Part 2: Interface structure

This part describes what the interface is made of:

- Views: the screens or pages that compose the interface
- Elements: the components that make up each view
- Forms: the input forms, with their fields
- Datatables: the data lists rendered as datatables
- Navigation: how the user moves between views

The interface structure describes the shape of the interface. It does not
describe the data binding of the elements.

---

## Part 3: Interface behavior

This part describes how the interface reacts:

- UI side validation: the validation performed in the interface before a
  request leaves it
- Loading states: how the interface signals a transition while it waits
- Datatable mode: the processing mode of each datatable

---

## Datatable mode

Each datatable declares its processing mode. There are two modes, with the
DataTables terminology:

- Server side processing: the browser receives only the rows of the current
  page; filtering, paging and sorting are computed by the server
- Client side processing: all rows are loaded into the browser, which handles
  filtering, paging and sorting

The ABD default is server side processing. A datatable is specified as server
side unless a deviation is declared.

Client side processing is allowed as an explicit and motivated deviation. It
is justified for small data sets, indicatively below the fifty thousand record
threshold, where loading all rows into the browser carries no significant
start cost. The deviation is declared, with its motivation, in this file.

### Consequence of the server side mode

When a datatable is specified as server side, the mode declared here generates:

- A private endpoint in `transport-contract.md`, which serves the rows
- An entry in `binding-contract.md`, which renders the datatable control

A server side datatable specified in `ui-spec.md` without the corresponding
endpoint and binding entry is incomplete.

A server side datatable is implemented with the ratified quality
requirements: loading indicators, debounce on the filter input, and fluid
paging and sorting without a perceptible refresh of the whole control.

---

## Validation rules

- The file is compiled during Execution Layer Setup, before code generation
  starts
- Every datatable declares its processing mode
- A client side datatable carries an explicit motivation for the deviation
- A server side datatable has its corresponding endpoint and binding entry in
  the transport and binding contracts
- The human validates the file before Claude Code starts code generation

---

## Template skeleton

The following is the empty skeleton to copy into the project repository as
`abd/steering/ui-spec.md`. Replace every bracketed placeholder with the
project value. In a project the Source column holds the specific ADR that
ratifies the specification.

```
# UI Spec

## Traceability

- Source decisions: [list of ratified ADRs]
- Actions List version: [version]
- Last validation date: [YYYY-MM-DD]
- Status: Draft

## Interface structure

### View: [view name]

- Elements: [elements of the view]
- Forms: [forms, with their fields]
- Datatables: [datatables in the view]
- Navigation: [navigation from this view]

## Interface behavior

### Validation

- [UI side validation rule]

### Loading states

- [loading state behavior]

### Datatable mode

- [datatable name]: [server side | client side]
- Deviation motivation: [required only when client side]

Rules:
- The ABD default datatable mode is server side processing
- A client side datatable declares an explicit motivation
- A server side datatable has its endpoint in the transport contract and its
  entry in the binding contract
```
