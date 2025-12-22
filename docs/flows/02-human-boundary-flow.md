# Human Boundary Flow

![human boundary](../assets/human-boundary-overview.png)

components referred to in doc:
- [ben_api](../components/ben_api.md)
- [benctl](../components/benctl.md)
- [bux](../components/bux.md)
- [ben_ledger](../components/ben_ledger.md)
- [ben_store_access](../components/ben_store_access.md)

## Purpose

This flow explains the **human interaction boundary** within the Ben system.

The human boundary encompasses two distinct components:
- **bux** - the human-facing inspection and testing surface
- **benctl** - the authoritative command-line interface for declaring and deploying intent

This document explains how humans interact with Ben, what data they can observe, what actions they can take, and where strict boundaries are enforced between observation, testing, and system mutation.

---

## Part 1: The Flow (What Happens When Things Go Right)

### What This Flow Explains

This flow describes how humans:
- Inspect system state and behavior
- Test schemas, policies, and intent safely
- Declare and deploy intent explicitly

It highlights the **intentional separation** between:
- Viewing and experimentation (**bux**)
- Authoritative system mutation (**benctl**)

---

### Steps of the Flow

This flow is divided into two parallel but distinct paths.

---

### Flow A: bux (Ben UX)

1. **bux** establishes a push-based connection to **ben_api** using WebSockets.
2. **ben_api** retrieves declared intent from **ben_ledger** for inspection and testing. (can also build and hand off artifacts to a "ben project" but can validate, build, verify, stage or deploy artifacts)
3. **bux** allows users to:
   - Inspect schemas, policies, playbooks, and signals
   - Test and validate intent artifacts
   - Explore potential system behavior
4. **bux** retrieves stored artifacts and metadata via **ben_api**, which accesses **ben_store_access**:
   - Policy packs
   - Versioned artifacts
   - Rich media and blob data
5. **bux** retrieves telemetry, logs, health data, and analytics via **ben_api**, which queries:
   - ClickHouse replicas for base system telemetry
   - ClickHouse replicas for Lucius-derived telemetry  
6. **bux** presents derived system state including:
   - Telemetry and logs
   - Health and liveness indicators
   - Rule violations and actions
   - Detector explanations and bounded outcomes

**bux does not deploy or activate intent.**  
All actions remain exploratory and non-authoritative.

---

### Flow B: benctl

1. **benctl** builds, inspects, and stages user intent as code.
2. **benctl** writes staged intent directly to **ben_ledger**.
3. **ben_ledger** validates and stores intent as the authoritative pre-activation record.
4. Upon deployment:
   - **ben_ledger** distributes intent artifacts to **ben_store_access**
   - **ben_store_access** stores artifacts and generates versioned keys
   - Keys and metadata are returned to **ben_ledger**
5. Once intent is successfully staged and recorded:
   - **ben_ledger** uses **NATS** to fan out change notifications
   - Runtime components pull updated intent explicitly

This flow ensures that **deployment is explicit, deliberate, and observable**.

---

### Summary

This flow establishes a strict visual and operational boundary between **bux** and **benctl**.

- **bux** exists to observe, test, and understand
- **benctl** exists to declare and activate intent

No human-facing tool performs both roles.

---

## Part 2: Failure and Stress (What Happens When Reality Intrudes)

### Failure Modes

- **ben_ledger** unavailable
- Illegal or malformed **benctl** commands
- RocksDB or MinIO unavailability
- Malformed or unexpected data passed to **ben_api**
- Incomplete or delayed ClickHouse replication
- Data migration lag between primary and replica stores

---

### Observed Consequences

- System views may degrade or become stale
- Deployments may be blocked or partially applied
- CLI errors may be confusing or prohibitive
- UI feedback may lack immediacy or clarity
- Boundaries between testing and deployment may feel opaque to users

Loss is visible and bounded, but not hidden.

---

### Open Questions and Areas Needing Clarity

- Should **benctl** functionality ever be partially exposed through **bux**?
- Where should the project draw a hard line to preserve separation of authority?
- How much testing capability can **bux** offer before redundancy or confusion arises?
- How complex can **benctl** become before it becomes burdensome?
- Where should errors be surfaced to users to avoid opaque failure states?
- What level of verbosity is appropriate for error reporting and system feedback?

These questions are intentionally documented and unresolved.

---

### Pain Points

- Building realistic testing and simulation capabilities in **bux** is complex and resource-intensive
- Simulating system behavior accurately without deployment is difficult
- Integrating **benctl** cleanly into Rust build workflows requires careful design
- Determining acceptable compile times for large intent changes is non-trivial

These pain points are acknowledged tradeoffs, not oversights.

---

## Closing Notes

This flow demonstrates Ben’s core philosophy at the human boundary:
- Observation and understanding are separated from authority
- Deployment is explicit and deliberate
- Failure is visible and explainable
- Human error is anticipated and constrained by architecture

The human boundary exists to protect both the system and the people operating it.