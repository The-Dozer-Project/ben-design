# Sidecar

## Purpose

Sidecars are the application-adjacent components responsible for translating application-specific data into Ben’s global schema and for delivering signals back into the application.

Their purpose is to act as a narrow, explicit boundary between an application and the rest of the Ben system.

Sidecars exist to adapt applications to Ben; not to adapt Ben to applications.

---

## Authority and Boundaries

Sidecars are authoritative only for:
- Translating application-specific data into Ben’s global schema
- Accepting signals destined for the application
- Delivering signals to the application through a dedicated control channel
- Maintaining the integrity of the application-facing boundary

Sidecars are **not** authoritative for:
- Policy or rule evaluation
- Signal generation
- Telemetry interpretation
- Routing decisions beyond their immediate upstream connection
- Orchestration or lifecycle management

Sidecars do not decide *what* happens in the system. They execute what is explicitly directed.

---

## Application Boundary Role

Sidecars form the hard boundary between the application and Ben.

They:
- Intake application-specific telemetry and events
- Normalize and translate data into Ben’s global schema
- Forward translated data upstream to proxies
- Receive signals emitted by detectors and deliver them to the application

This boundary prevents application internals from leaking into Ben’s internal systems and prevents Ben from assuming application-specific behavior.

---

## Signal Delivery Model

Sidecars receive signals exclusively from detectors via NATS.

Signal flow is strictly downstream: 
```
Detector → NATS → Sidecar → Application
```
Sidecars do not generate signals and do not communicate directly with detectors.

Signals are delivered through a dedicated control channel, isolated from telemetry and general information flows.

---

## Configuration Model

Sidecars operate under a single, minimal configuration specific to the application and deployment context.

This configuration:
- Is code-as-configuration
- Is intentionally narrow in scope
- Avoids runtime mutation
- Is designed to simplify deployment and reasoning

Uniformity and minimalism are favored over flexibility.

---

## Attestation and Trust Establishment

Sidecars use **BEE / BEE Keeper** to establish attested communication with applications and upstream systems.

Attestation ensures:
- The sidecar is authorized to represent the application
- Communication channels are explicit and verifiable
- Trust is established without implicit assumptions

Sidecars do not infer trust from network topology or proximity.

---

## State Model

Sidecars maintain only the minimal state required to:
- Manage active connections
- Perform data translation
- Deliver signals reliably

All state is local, bounded, and discardable. Restarting a sidecar does not alter system intent or historical data.

---

## Interaction With Other Components

- **Applications** communicate only with their attached sidecar(s).
- **Proxies** receive normalized data from sidecars.
- **Detectors** indirectly influence sidecars through signal emission.
- **BEE / BEE Keeper** handles attestation and secure attachment.
- **Orchestrator** manages sidecar lifecycle but does not influence behavior.

Sidecars do not interact directly with spines, detectors, or data stores.

---

## What This Component Does Not Do

Sidecars do not:
- Evaluate rules or policies
- Emit signals
- Interpret telemetry
- Perform routing or buffering beyond their immediate upstream
- Store authoritative data
- Participate in intent declaration or deployment

They exist to translate, transmit, and obey.

