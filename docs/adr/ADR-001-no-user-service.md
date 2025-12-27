# ADR-001: Dwarapalak does NOT manage users

## Status
Accepted

## Context
Most authentication systems tightly couple authorization with user
storage and identity management. This creates strong dependency on
user databases and makes integration harder for adopters.

Dwarapalak aims to be a pluggable authorization system that can be
used by systems which already manage users in their own databases
and identity providers.

## Decision
Dwarapalak will NOT manage users or user lifecycle.

- No user creation
- No password storage
- No profile management
- No identity verification

Dwarapalak operates purely on **subjects**, provided by the client
application.

## Consequences
### Positive
- Loose coupling with client systems
- Easy integration with existing user databases
- Works with any identity provider (OAuth, SAML, custom)

### Negative
- Client must resolve identity before authorization
- Slightly more responsibility on integrators