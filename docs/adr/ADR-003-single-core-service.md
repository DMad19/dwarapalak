# ADR-003: Single Core Service with Evolvable Microservice Boundaries

## Status
Accepted

---

## Context

Dwarapalak is an open-source authorization system intended to be
embedded into many types of client applications.

While microservice architectures offer scalability and isolation,
starting with multiple services introduces:
- Operational overhead
- Complex deployments
- Increased cognitive load for contributors

At the same time, Dwarapalak must be able to evolve as usage grows
and as new concerns (audit, messaging, analytics) emerge.

---

## Decision

Dwarapalak will start as a **single deployable core service**.

Within this service:
- Clear internal boundaries will be enforced
- Modules will communicate via interfaces, not direct implementations
- Infrastructure concerns remain isolated via SPI and adapters

These boundaries are designed so that:
- Any internal module can later be extracted into a standalone service
- No core domain rewrite is required during extraction

---

## Consequences

### Positive
- Simple local development and onboarding
- Faster iteration during early stages
- Lower operational and deployment complexity
- Easier contribution for new collaborators

### Negative
- Limited independent scaling in early stages
- Requires discipline to maintain clean boundaries
- Some refactoring may be needed during service extraction

---

## Notes

Potential future service boundaries include:
- Audit logging service
- Policy evaluation service
- Notification / event processing service

This decision aligns with earlier ADRs:
- ADR-001: No user management
- ADR-002: SPI + Adapter architecture