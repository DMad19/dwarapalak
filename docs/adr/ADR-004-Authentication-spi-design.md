# ADR-004: Authentication SPI Design

## Status
Accepted

## Context

Dwarapalak is designed as a domain-driven authorization system built using a
Service Provider Interface (SPI) + Adapter architecture.

Authorization decisions require an authenticated identity (`Principal`).
However, authentication mechanisms vary significantly across environments,
including but not limited to:

- JWT-based authentication
- OAuth2 / OpenID Connect
- API keys
- mTLS
- Enterprise SSO systems
- Custom identity providers

Embedding authentication logic directly into the Dwarapalak core would tightly
couple the system to specific authentication strategies and reduce its
adoptability.

At the same time, the core must be able to consume authentication results in a
consistent and domain-oriented manner to perform authorization.

---

## Decision

Authentication in Dwarapalak will be provided via a **pluggable SPI**.

Key decisions:

- The Dwarapalak core **does not implement authentication**
- Authentication logic is delegated to external implementations
- The core depends only on authentication contracts
- Authentication is modeled as a single atomic operation

Authentication is treated as a **black-box operation** that returns a verdict
and, if successful, an authenticated identity.

---

## Authentication SPI Components

### AuthenticationProvider

Represents an external authentication mechanism.

Responsibilities:
- Validate credentials
- Perform identity verification
- Return authentication outcome

Contract:
- Accept an authentication request
- Return an authentication result

---

### AuthenticationRequest

Represents all input required to perform authentication.

Characteristics:
- Transport-agnostic
- Contains raw credentials (token, API key, etc.)
- Contains optional metadata (headers, tenant, client info)

The core treats this object as opaque and does not interpret its contents.

---

### AuthenticationResult

Represents the outcome of an authentication attempt.

Characteristics:
- Explicit success or failure
- Provides an authenticated principal only on success
- Provides an optional failure reason

AuthenticationResult represents a **verdict**, not an identity.

---

### AuthenticatedPrincipal

Represents a successfully authenticated subject.

Characteristics:
- Stable subject identifier
- Attribute map used for authorization
- Optional scopes or claims

AuthenticatedPrincipal becomes the **input to authorization decisions** in the
core domain.

---

## Core Responsibilities

The Dwarapalak core:

- Invokes the configured AuthenticationProvider
- Consumes AuthenticationResult
- Uses AuthenticatedPrincipal as input for authorization
- Does not perform credential validation
- Does not parse tokens or credentials
- Does not depend on any authentication mechanism

The core must never:
- Validate passwords
- Verify JWT signatures
- Call external identity providers
- Assume how authentication is implemented

---

## Adapter Responsibilities

Authentication adapters:

- Implement AuthenticationProvider
- Perform credential validation
- Integrate with external identity systems
- Translate external identity data into AuthenticatedPrincipal
- Explicitly handle authentication failures

---

## Consequences

### Positive

- Authentication is fully pluggable
- Supports heterogeneous identity systems
- Clear separation of authentication and authorization
- Enables Dwarapalak to be used as:
    - Authorization-only engine
    - Combined authentication and authorization system
- Simplifies testing through mock implementations

### Negative

- Requires adopters to supply an authentication adapter
- Incorrect adapter implementations can introduce security risks
- Requires clear SPI documentation and versioning discipline

---

## Alternatives Considered

### Embedding Authentication in Core

Rejected due to:
- Tight coupling to specific authentication strategies
- Reduced extensibility
- Increased maintenance complexity

### Exposing Credential Validation Methods

Rejected due to:
- Leakage of internal authentication steps
- Loss of abstraction
- Inflexibility for non-token-based authentication systems

---

## Notes

- Authentication SPI is optional; adopters may authenticate externally and
  supply principals directly to the core.
- This ADR builds upon the SPI + Adapter principles defined in ADR-002.