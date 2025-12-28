# Domain Use Cases

This document defines the **core business use cases** of Dwarapalak.
These use cases describe *what the system does*, not *how it is implemented*.

All use cases are expressed using the shared domain language defined in
[`vocabulary.md`](./vocabulary.md).

---

## Scope

- Focuses only on **authorization**
- Framework-agnostic
- No API, persistence, or transport concerns
- Authentication is assumed to be handled externally

---

## Core Principle

Dwarapalak is an **authorization decision engine**.

Every use case ultimately exists to support the evaluation of an
**authorization request** and the production of an
**authorization decision**.

---

## UC-01: Evaluate Authorization Request

### Description
Determine whether a subject is permitted to perform a specific action on
a resource under a given context.

### Primary Actor
- External system (application, service, gateway)

### Inputs
- Subject
- Action
- Resource
- Context

### Outcome
- Authorization decision (Allow or Deny)

### Notes
This is the **foundational use case**.
All other use cases exist to support or extend this capability.

---

## UC-02: Explain Authorization Decision

### Description
Provide a human-readable explanation describing why an authorization
decision was reached.

### Primary Actor
- Developer
- Operator
- Auditing system

### Inputs
- Authorization request
- Authorization decision

### Outcome
- Decision explanation

### Notes
This use case supports debugging, transparency, and compliance.

---

## UC-03: Audit Authorization Decision

### Description
Record authorization decisions for traceability, auditing, and compliance
purposes.

### Primary Actor
- Authorization engine (internal)

### Inputs
- Authorization request
- Authorization decision

### Outcome
- Audit event emitted

### Notes
Auditing is a side effect of decision evaluation and must not influence
the decision outcome.

---

## UC-04: Consume Authenticated Subject

### Description
Accept subject identity information that has already been authenticated
by an external authentication system.

### Primary Actor
- External authentication provider

### Inputs
- Authenticated subject information

### Outcome
- Subject accepted for authorization evaluation

### Notes
Dwarapalak does **not** authenticate subjects.
It only consumes authenticated identity data.

---

## UC-05: Resolve Authorization Policies

### Description
Retrieve and apply authorization policies defined outside the core
authorization engine.

### Primary Actor
- Policy author
- External system

### Inputs
- Policy definitions

### Outcome
- Policies available for evaluation

### Notes
Policy lifecycle management is external to the core domain.

---

## Explicitly Out of Scope

The following concerns are intentionally excluded:

- User management
- Role assignment
- Credential validation
- Request enforcement
- Transport security

These responsibilities belong to external systems.

---

## Summary

Dwarapalak provides a focused, extensible authorization engine centered on
decision evaluation, explanation, and auditability — without coupling to
authentication, persistence, or infrastructure concerns.