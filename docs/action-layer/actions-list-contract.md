# Actions List Contract

## Purpose

This document defines how the Implementation Agent must treat the `actions-list.md` file inside an ABD workspace.

It does not redefine the ADExMo Actions List model.

The Actions List concept is defined by ADExMo and is produced during the ABD Action Layer phase.

## Source

The `actions-list.md` file is derived from:

- accepted ADRs
- domain definitions
- ADExMo action modeling rules
- ABD Action Layer analysis

## Role in ABD

Within ABD, the Actions List is the executable behavior contract of the target application.

It defines what the system must be able to do.

It is consumed by the Implementation Agent during code generation.

## File

The canonical Actions List file is:

```text
/abd/actions/actions-list.md