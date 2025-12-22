# Orchestrator

## Purpose

The Orchestrator is Ben’s coordination and reconciliation engine.

Its purpose is to observe system presence and health, reconcile that observation against declared intent, and manage the lifecycle of runtime components such as sidecars, spines, proxies, and detectors.

The Orchestrator exists to ensure the system converges toward declared intent, not to interpret data or make policy decisions.

---

## Authority and Boundaries

The Orchestrator is authoritative only for:
- Service discovery
- Component presence tracking
- Component health assessment
- Spin-up and tear-down of runtime components
- Maintaining an authoritative routing and topology view

The Orchestrator is **not** authoritative for:
- Declaring or distributing intent
- Telemetry ingestion or interpretation
- Policy evaluation
- Signal emission
- Data storage or mutation
- Sanitization or detonation workflows

Authority is limited strictly to topology and liveness.

---

## Service Discovery and Health Model

The Orchestrator is the source of truth for:
- Which components exist
- Which components are reachable
- Which components are healthy enough to participate

Components announce presence and health through the system’s presence plane. The Orchestrator observes these announcements and maintains an authoritative, reconciled view of system topology.

Health is treated as a first-class signal for lifecycle decisions, not as telemetry.

---

## Lifecycle Management

The Orchestrator is responsible for:
- Spinning up components when declared intent requires them
- Tearing down components when they are no longer needed
- Replacing unhealthy or failed components
- Coordinating detector availability with the rest of the system

Lifecycle actions are driven by reconciliation between:
- Declared intent (from authoritative sources)
- Observed presence and health

The Orchestrator does not push configuration or policy during lifecycle events.

---

## Interaction With Other Components

The Orchestrator interacts with:
- NATS for presence observation
- **Runtime components** for lifecycle management
- **ben_ledger** indirectly by observing declared intent outcomes
- components for health information, lifecycles, and component readiness as well as service discovery.

The Orchestrator does **not**:
- Interact with Lucius
- Interpret telemetry
- Act as a message broker
- Participate in data pipelines

---

## State Model

The Orchestrator maintains minimal, explicit state required for coordination.

This includes:
- An atomic routing and topology table
- Component presence records
- Health status snapshots

All state is:
- Derived
- Ephemeral
- Reconstructible from presence signals and declared intent

Restarting the Orchestrator does not alter system intent.

---

## Failure Model

If the Orchestrator fails:
- Runtime components continue operating in their last known state
- Telemetry ingestion is unaffected
- Intent remains preserved elsewhere

Upon restart, the Orchestrator rebuilds its view by re-observing presence and reconciling against declared intent.

---

## What This Component Does Not Do

The Orchestrator does not:
- Store telemetry or artifacts
- Route data
- Execute policy
- Emit signals
- Perform sanitization
- Act as a message queue

It coordinates *components*, not *behavior*.

