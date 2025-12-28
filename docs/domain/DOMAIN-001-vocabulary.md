# Dwarapalak Domain Vocabulary

## Purpose

This document defines the **ubiquitous language** used throughout the
Dwarapalak project.

All domain models, APIs, documentation, and discussions must use the terms
defined here **consistently and precisely**.

This document intentionally avoids:
- Implementation details
- Data structures
- Framework-specific concepts

It serves as the **foundation for all subsequent domain design**.

---

## Subject

An authenticated entity that attempts to perform an action on a resource.

Characteristics:
- Identity is already established prior to authorization
- Represents a user, service, or system
- Immutable during the authorization process

The authorization domain assumes the subject is trusted and does not perform
authentication.

---

## Resource

A target upon which an action is attempted.

Characteristics:
- Represents a protected entity or capability
- Can be logical or physical
- Interpreted by policies, not by the core engine

Examples:
- Document
- API endpoint
- Account
- Order

---

## Action

An operation that a subject attempts to perform on a resource.

Characteristics:
- Expresses intent, not implementation
- Domain-specific
- Evaluated by policies

Examples:
- Read
- Write
- Delete
- Approve

---

## Context

Supplementary information provided with an authorization request.

Characteristics:
- Influences policy evaluation
- Optional but always available
- Does not alter the subject identity

Examples:
- Environment (prod, staging)
- Time of access
- Request origin
- Tenant information

---

## Policy

A rule that governs whether an action on a resource is permitted for a subject
under specific conditions.

Characteristics:
- Declarative in nature
- Evaluated by the authorization engine
- Independent of storage or transport mechanisms

Policies express **authorization intent**, not execution logic.

---

## Authorization

The process of determining whether a subject is allowed to perform an action on
a resource under a given context.

Characteristics:
- Deterministic
- Side-effect free
- Independent of authentication

Authorization produces a decision but does not enforce it.

---

## Decision

The outcome of an authorization process.

Characteristics:
- Explicit and finite
- Represents either permission or denial
- Always returned for a valid authorization request

Possible values:
- Allow
- Deny

---

## Authorization Engine

The core component responsible for evaluating policies and producing
authorization decisions.

Characteristics:
- Operates only on domain concepts
- Depends on abstractions, not implementations
- Free of infrastructure concerns

---

## Non-Goals

The following concepts are intentionally outside the scope of the authorization
domain:

- User management
- Credential storage
- Authentication mechanisms
- Transport protocols
- Enforcement of decisions

---

## Notes

This vocabulary must remain stable. Any change to these definitions requires
explicit review and, if necessary, a corresponding Architectural Decision
Record (ADR).