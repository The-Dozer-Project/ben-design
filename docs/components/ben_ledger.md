# ben_ledger

## Purpose

ben_ledger is the authoritative registry for declared system intent.

Its purpose is to store, version, and stage schemas, policy packs, playbooks, and signals prior to activation, and to serve as the recovery anchor for intent across restarts, failures, and topology changes.

ben_ledger preserves what the system *meant* to do, not what it happened to observe.

---

## Authority and Boundaries

ben_ledger is authoritative for:
- Declared schemas and contracts
- Policy packs and rule definitions
- Playbooks and signal definitions
- Versioning and activation epochs of intent

ben_ledger is **not** authoritative for:
- Telemetry or analytics
- Runtime coordination
- Component presence or liveness
- Data plane execution
- Storage of observational data

ben_ledger does not infer intent. All entries are explicitly authored and versioned.

---

## Staging and Deployment Role

ben_ledger acts as the staging boundary for intent prior to deployment.

Schemas, policy packs, playbooks, and signals are written to ben_ledger before activation. Deployment consists of publishing these versioned declarations for consumption by runtime components.

ben_ledger does not execute deployment actions itself. It records declared intent and exposes it for reconciliation and activation by other system components.

---

## Recovery and Rehydration

ben_ledger serves as the source of truth during catastrophic failure.

On restart or redeployment:
- Runtime components rehydrate by pulling declared intent from ben_ledger
- System behavior is reconstructed by re-observing reality and reconciling it against stored intent

Snapshotting ben_ledger is sufficient to recover system intent. Loss of observational data does not invalidate the ledger.

---

## Interaction With Other Components

- **Orchestrator** consults ben_ledger to reconcile observed component presence against declared intent.
- **Spines, Proxies, Sidecars, and Detectors** retrieve schemas, playbooks, and signals from ben_ledger during startup, restart, or reconfiguration.
- **ben_api** reads from ben_ledger to expose schemas, playbooks, signals, and configurations for inspection and testing.
- **data_migrator** consumes schema and versioning information derived from ben_ledger to perform required data-plane updates, including ClickHouse schema operations, without granting ben_ledger execution authority.

ben_ledger exposes intent but does not participate in execution, coordination, or data migration.

---

## Versioning Model

All entries in ben_ledger are versioned.

Changes to schemas, policies, playbooks, or signals are additive and explicit. Runtime behavior changes only when a new version is activated. Drift between declared and active intent is treated as an error condition to be reconciled, not silently corrected.

---

## What This Component Does Not Do

ben_ledger does not:
- Store telemetry or analytics
- Execute schema migrations or data operations
- Cause changes itself
- Detect anomalies or emit signals
- Act as a message bus or control plane

It is a ledger in the literal sense: a durable record of declared intent and contracts.

