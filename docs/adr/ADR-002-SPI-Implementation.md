## Decision

Dwarapalak will adopt a **Service Provider Interface (SPI) + Adapter architecture**.

### Key principles
- The core authorization engine depends **only** on SPI contracts
- All infrastructure concerns are implemented as **adapters**
- Adapters can be:
    - Replaced
    - Extended
    - Custom-built by integrators

### Examples of SPI contracts
- **PolicyRepository** – fetch authorization policies
- **RoleRepository** – resolve roles and permissions
- **AuditLogWriter** – record authorization decisions
- **SubjectResolver** – map external identity to internal subject

### Adapter responsibilities
- Implement SPI interfaces
- Translate between Dwarapalak’s domain model and client systems
- Handle persistence, caching, or external calls

### The core module must never
- Import adapter classes
- Contain database-specific annotations
- Know how data is stored or retrieved

---

## Consequences

### Positive
- Strong separation of concerns
- True pluggability for adopters
- Works with existing client databases
- Enables clean SDK-based integration
- Simplifies testing via mock SPI implementations

### Negative
- Slightly higher initial learning curve for contributors
- Requires careful versioning and backward compatibility of SPI interfaces
- Adapter misimplementation can affect runtime behavior if not validated