# SPI Contracts

This document defines the **Service Provider Interface (SPI) contracts**
used by Dwarapalak.

SPI contracts represent **capabilities that must be implemented by
integrators (users of Dwarapalak)** so that the authorization engine can
operate in heterogeneous environments.

The Dwarapalak core **never implements SPI interfaces**.

---

## SPI Scope Rule (Authoritative)

A contract belongs in SPI **if and only if**:

- It must be implemented by the integrator
- Its behavior is environment-specific
- It cannot be safely defaulted by the core

If the core can provide a correct default implementation, the contract
**must not be SPI**.

---

## Design Constraints

All SPI contracts must adhere to the following constraints:

- Domain-language only (no infrastructure models)
- No framework or library dependencies
- No persistence or transport abstractions
- No business logic
- Explicit failure semantics

Breaking these rules is considered an architectural defect.

---

## Final SPI Contract Set

The following SPI contracts are the **only valid SPI interfaces** in
Dwarapalak.

---

## 1. AuthenticationProvider

### Purpose
Provide an authenticated subject to the authorization engine.

Dwarapalak does not perform authentication but must consume authentication
results from external systems in a consistent, domain-safe manner.

---

### Implemented By
- Integrators
- Platform owners
- Application teams

---

### Responsibilities
- Resolve and return a Subject that has already been authenticated
- Ensure subject identity integrity

---

### Inputs
- Authentication context (implementation-defined)

### Outputs
- Subject

---

### Failure Semantics
- Must fail explicitly if the subject cannot be resolved
- Must not fabricate, default, or infer subject identities

---

### Non-Responsibilities
- Credential validation
- Token parsing
- Session management
- Identity lifecycle control

---

## 2. PolicyRepository

### Purpose
Provide authorization policies applicable to a given authorization request.

Policies are owned, stored, and managed by the integrator.

---

### Implemented By
- Integrators
- Platform owners
- Policy management systems

---

### Responsibilities
- Resolve all policies relevant to an AuthorizationRequest

---

### Inputs
- AuthorizationRequest

### Outputs
- Collection of policies

---

### Failure Semantics
- Must fail explicitly if policies cannot be retrieved
- Returning an empty policy set is valid and results in DENY

---

### Non-Responsibilities
- Policy evaluation
- Policy conflict resolution
- Policy enforcement
- Policy lifecycle management

---

## Explicitly Not SPI

The following concerns are intentionally **not SPI contracts**:

- Policy evaluation
- Decision explanation generation
- Audit event publishing
- Logging or monitoring
- Persistence or transport

These responsibilities either:
- Have safe defaults
- Are internal to the core
- Are infrastructure concerns handled by adapters

---

## Dependency Direction

Domain Core


↓ depends on

 dwarapalak-spi

↓ implemented by

Integrator Code

--- 
Adapters may also implement **core-owned extension points**, but those are
not SPI and must not live in `dwarapalak-spi`.

---

## Summary

The SPI surface of Dwarapalak is intentionally **small and stable**.

Only two contracts are SPI:
- AuthenticationProvider
- PolicyRepository

This ensures:
- Clear ownership
- Minimal breaking changes
- Strong domain isolation
- True pluggability without architectural drift
