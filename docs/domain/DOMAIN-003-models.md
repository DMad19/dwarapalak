# Domain Models

This document defines the **core domain models** of Dwarapalak.
These models represent the fundamental concepts required to evaluate
authorization decisions.

All models are **framework-agnostic**, **immutable by design**, and use
the shared language defined in [`vocabulary.md`](./vocabulary.md).

---

## Design Principles

- Domain models express **what the system is**, not how it is built
- No persistence, transport, or framework concerns
- No dependency on SPI or adapters
- Models may enforce invariants but contain no orchestration logic

---

## AuthorizationRequest

### Description
Represents a request to evaluate whether a subject is allowed to perform
an action on a resource under a given context.

### Responsibilities
- Encapsulate all inputs required for authorization evaluation
- Act as the single unit of decision evaluation

### Conceptual Composition
- Subject
- Action
- Resource
- Context
- Request metadata (optional)

### Constraints
- Must contain exactly one subject, action, and resource
- Context may be empty but never null
- Immutable once created

---

## AuthorizationDecision

### Description
Represents the outcome of evaluating an authorization request.

### Responsibilities
- Express whether access is allowed or denied
- Provide an explainable result
- Support auditability

### Possible Results
- ALLOW
- DENY

### Conceptual Composition
- Decision result
- Decision reason (human-readable)
- Evaluation metadata
- Correlation to authorization request

### Constraints
- Deterministic for the same input
- Immutable once produced

---

## Subject

### Description
Represents the entity requesting access.

### Responsibilities
- Provide identity information for authorization
- Carry attributes relevant to policy evaluation

### Examples
- User
- Service account
- API client
- Device

### Conceptual Composition
- Subject identifier
- Subject type
- Subject attributes

### Constraints
- Must be identifiable
- Does not contain credentials
- Authentication state is external

---

## Action

### Description
Represents the operation being attempted by the subject.

### Responsibilities
- Define what is being requested

### Examples
- READ
- WRITE
- DELETE
- APPROVE

### Conceptual Composition
- Action name
- Action attributes (optional)

### Constraints
- Must be comparable
- Should be domain-specific

---

## Resource

### Description
Represents the object on which an action is being performed.

### Responsibilities
- Identify what is being accessed
- Provide attributes relevant for authorization

### Examples
- Document
- Order
- Account
- API endpoint

### Conceptual Composition
- Resource identifier
- Resource type
- Resource attributes

### Constraints
- Must be uniquely identifiable within its domain

---

## Context

### Description
Represents environmental or situational data that may influence
authorization decisions.

### Responsibilities
- Provide dynamic, request-scoped information

### Examples
- Time of request
- Location
- Client IP
- Request origin

### Conceptual Composition
- Key–value attribute map

### Constraints
- Optional
- Must not contain identity information
- Must not override subject attributes

---

## Model Relationships
AuthorizationRequest

├── Subject

├── Action

├── Resource

└── Context

AuthorizationDecision

└── AuthorizationRequest (correlated)

---

## Explicitly Out of Scope

- Authentication logic
- Policy storage formats
- Persistence models
- API representations
- Transport-layer concerns

---

## Summary

These domain models form the **stable core** of Dwarapalak.
They are designed to remain valid even as APIs, storage mechanisms,
and deployment architectures evolve.