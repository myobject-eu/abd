# Database Schema Definition

## Document type

This is ABD system documentation. It defines the structure, notation and
compilation rules of the `database-schema.md` Steering File. It is not a
project artifact: it lives in the public ABD documentation repository and is
updated only when the ABD method evolves.

The file it describes, `database-schema.md`, is a project artifact. It lives
in the project repository and is compiled once per project.

---

## Purpose

`database-schema.md` is the Steering File that records the database schema of
a project: the tables, columns, keys, relationships and constraints that the
persistence layer is built on.

It describes the schema. It does not apply it. The Steering File is
documentation that guides code generation, not a migration tool.

---

## Location of the compiled file

The compiled `database-schema.md` lives in the project repository, in the
Steering Files directory:

```
abd/steering/database-schema.md
```

It sits alongside `stack-config.md` and `integration-config.md`.

---

## Relationship with stack-config.md

The persistence engine is recorded in `stack-config.md`, not here.
`database-schema.md` describes the schema in a form that is neutral with
respect to the engine. The same schema is valid whether the project runs on
MySQL or PostgreSQL.

The translation from the neutral schema to the native dialect of the engine is
not part of this file. It is a responsibility of the Service Level and the
model.

---

## Notation

The schema is expressed in DBML (Database Markup Language). DBML is a
human readable notation designed for documenting schemas. It is not a
migration language.

A table is declared as a `Table` block. Columns carry a name, a neutral type
and optional settings such as primary key, not null, unique and default.
Relationships are declared with references.

Example of the notation:

```
Table users {
  id bigint [pk, increment]
  email string(255) [not null, unique]
  display_name string(120) [not null]
  is_active boolean [not null, default: true]
  created_at timestamp [not null]
}
```

---

## Structure of database-schema.md

The file has two parts, in this order:

1. Traceability header
2. Schema in DBML

---

## Part 1: Traceability header

The header is a fixed block at the top of the file.

Required fields:

- Source decisions: the ratified ADRs the schema derives from
- Actions List version: the validated Actions List version the schema was
  derived against
- Last validation date: the date the human last validated the file
- Status: Draft or Validated

The file is not considered usable by the the coding agent until Status is Validated.

---

## Part 2: Schema in DBML

The schema is written in DBML. Columns are declared exclusively with the
neutral type vocabulary defined below. Native engine types are not used.

---

## The neutral type vocabulary

Columns are declared with these thirteen logical types only. Each type
translates deterministically to MySQL and PostgreSQL.

| Neutral type | Meaning | MySQL | PostgreSQL |
|---|---|---|---|
| `string(n)` | Bounded text | `VARCHAR(n)` | `VARCHAR(n)` |
| `text` | Free text | `TEXT` | `TEXT` |
| `smallint` | Short integer | `SMALLINT` | `SMALLINT` |
| `integer` | Integer | `INT` | `INTEGER` |
| `bigint` | Long integer | `BIGINT` | `BIGINT` |
| `decimal(p,s)` | Fixed precision exact number | `DECIMAL(p,s)` | `NUMERIC(p,s)` |
| `boolean` | Logical value | `TINYINT(1)` | `BOOLEAN` |
| `date` | Date | `DATE` | `DATE` |
| `time` | Time | `TIME` | `TIME` |
| `timestamp` | Point in time | engine mapping | engine mapping |
| `binary` | Raw byte sequence | `BLOB` | `BYTEA` |
| `json` | Structured data | `JSON` | `JSONB` |
| `uuid` | Unique identifier | `CHAR(36)` or `BINARY(16)` | `UUID` |

### Notes on specific types

- `timestamp`: a single logical type. Time zone handling is not delegated to
  the type but to the data registration procedure. The schema does not
  distinguish between instants with and without time zone.
- `uuid`: kept despite the asymmetric mapping between engines. The model
  handles the translation to the native type.
- `json`: a type in its own right, not a fallback. A `json` column declares
  the presence of structured data, for example the serialization of a class.
  This status is analysis information and must be treated as such.
- `binary`: the only binary type. There is no separate `blob` type.

### Excluded types

- `enum`: not allowed. MySQL and PostgreSQL handle enumerations in
  structurally different ways. Constrained values are modeled as a `string`
  with a constraint, or as a lookup table.
- `float` and `double`: not in the base vocabulary. Floating point is unfit
  for exact values and `decimal` covers most cases. A project with a genuine
  need introduces them as an exception motivated by a ratified technical
  constraint.

---

## Primary key identity

Auto incrementing primary key identity is not a type. It is an attribute. It
is declared as a `bigint` column with the increment setting. The translation
resolves the setting to `AUTO_INCREMENT` on MySQL and to `IDENTITY` or
`BIGSERIAL` on PostgreSQL. There is no dedicated `id` type: it would confuse
the type of the data with its role.

---

## File attachment pattern

A column representing an attached file, an image or a document is not a data
type. It is a modeling pattern.

Storing large binary files inside a column is an architectural decision with
consequences on database size, backups and performance. The ABD default is
the external reference: the file resides on a filesystem or object storage and
the database holds a `string` reference. Inline storage in a `binary` column
is allowed only as an exception motivated by a ratified technical constraint.

The file attachment pattern is modeled with several columns: the reference or
the bytes, the mime type, the original name and the size.

---

## Validation rules

- The file is compiled during Execution Layer Setup, before code generation
  starts
- Columns are declared exclusively with the thirteen neutral types
- Native engine types are not used in the schema
- The human validates the file before the the coding agent starts code generation
- A new type outside the vocabulary requires a method level decision, not a
  local choice in this file
- The coding agent warns the developer when it detects a type outside the
  ratified vocabulary

---

## Template skeleton

The following is the empty skeleton to copy into the project repository as
`abd/steering/database-schema.md`. Replace every bracketed placeholder with
the project value. In a project the Source column holds the specific ADR that
ratifies the schema.

```
# Database Schema

## Traceability

- Source decisions: [list of ratified ADRs]
- Actions List version: [version]
- Last validation date: [YYYY-MM-DD]
- Status: Draft

## Schema

Table [table_name] {
  id bigint [pk, increment]
  [column_name] [neutral_type] [settings]
}

Rules:
- Columns use the thirteen neutral types only
- Native engine types are not used
- File attachments follow the external reference pattern by default
- The translation to the native dialect is a Service Level responsibility
```
