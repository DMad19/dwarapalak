# Domain Services

This document defines the **core domain services** of Dwarapalak and
describes how authorization decisions are evaluated.

Domain services coordinate domain models to perform business operations
that do not naturally belong to a single entity.

---

## Design Principles

- Domain services contain **business behavior**, not infrastructure logic
- No framework, persistence, or transport concerns
- No dependency on SPI or adapters
- Deterministic and side-effect free decision making

---

## AuthorizationEngine

### Description
The AuthorizationEngine is responsible for evaluating authorization
requests and producing authorization decisions.

It represents the **central decision-making service** of Dwarapalak.

---

### Responsibilities

- Accept an AuthorizationRequest
- Validate request invariants
- Resolve applicable authorization policies
- Evaluate policies against the request
- Produce an AuthorizationDecision
- Attach explanation and audit metadata

---

### Inputs
- AuthorizationRequest

### Outputs
- AuthorizationDecision

---

### Explicit Non-Responsibilities

The AuthorizationEngine must never:

- Authenticate subjects
- Load users, roles, or credentials directly
- Persist authorization data
- Enforce access control
- Communicate with external systems

These responsibilities belong outside the domain core.

---

## Authorization Decision Flow

The authorization process follows a conceptual, deterministic flow.

---

### Step 1: Request Acceptance

- Receive AuthorizationRequest
- Verify presence of subject, action, and resource
- Reject invalid or incomplete requests

---

### Step 2: Policy Resolution

- Identify policies applicable to the request
- Policy types may include:
    - Attribute-based policies
    - Role-based policies
    - Context-based policies
- Resolution strategy is abstract and implementation-agnostic

---

### Step 3: Policy Evaluation

- Evaluate each resolved policy against the request
- Combine results using a deterministic strategy
- Conflict resolution favors restrictive outcomes

---

### Step 4: Decision Production

- Produce an AuthorizationDecision
- Attach explanation metadata
- Attach audit metadata

---

## Decision Strategy

Dwarapalak follows a **fail-safe authorization model**:

- Default outcome is **DENY**
- Explicit permission is required to **ALLOW**
- Missing or ambiguous information must not grant access

---

## Determinism Guarantee

Given the same:
- AuthorizationRequest
- Set of resolved policies

The AuthorizationEngine must always produce the same AuthorizationDecision.

This guarantees:
- Auditability
- Debuggability
- Decision replayability

---

## Conceptual Extension Points

The following extension points are identified but not yet formalized:

- Policy resolution strategy
- Policy evaluation logic
- Decision explanation generation
- Audit event emission

These extension points will later map to SPI contracts.

---

## Summary

The AuthorizationEngine defines the **core behavior** of Dwarapalak.
It orchestrates domain models to deliver consistent, explainable, and
auditable authorization decisions while remaining isolated from
infrastructure concerns.