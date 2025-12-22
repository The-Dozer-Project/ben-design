# ben_store_access

## Purpose

ben_store_access is the data access intermediary for Ben’s persistent storage systems.

Its purpose is to provide controlled, secure, and efficient access to stored data across RocksDB and the MinIO cluster, without embedding business logic, authority, or long-lived state into the data plane.

---

## Authority and Boundaries

ben_store_access is authoritative only for:
- Mediating access to underlying storage systems
- Streaming stored data to authorized requesting components
- Enforcing access boundaries and security constraints during retrieval

ben_store_access is **not** authoritative for:
- Policy definition or evaluation
- Intent declaration or versioning decisions
- Telemetry interpretation
- Signal generation
- Runtime coordination

It does not infer meaning from data. It only moves data.

---

## Storage Responsibilities

ben_store_access provides access to two distinct storage backends:

### RocksDB
RocksDB is used to store:
- Policy pack keys
- MinIO object references
- Versioning metadata and lookup indices

RocksDB acts as the structured index and key-value anchor for stored artifacts.

### MinIO Cluster
MinIO is used to store:
- Policy packs and serialized artifacts
- Blob and rich media data
- Inert storage for potentially malicious or adversarial inputs

MinIO is treated as an inert object store. Data housed here is not executed, interpreted, or trusted by default.

---

## Data Access and Streaming

ben_store_access is optimized for:
- High-concurrency access
- Streaming data to multiple consumers
- Efficient fan-out to requesting components

It does not retain data after transfer and does not maintain per-client state beyond the lifetime of an active request.

All data access is pull-based and explicit.

---

## Security Model

ben_store_access enforces:
- Strict access boundaries
- Controlled exposure of stored data
- No implicit trust of retrieved artifacts

Data is delivered as-is. Validation, execution, and interpretation occur downstream in components designed for that purpose.

---

## Interaction With Other Components

- **ben_api** queries ben_store_access to inspect stored artifacts, versioning information, and object metadata.
- **Lucius** retrieves artifacts and blob data for sanitization and detonation workflows.
- **Other system components** may retrieve stored data through ben_store_access when explicitly authorized.

ben_store_access does not initiate data movement and does not push updates.

---

## State Model

ben_store_access is intentionally stateless.

It maintains no durable or derived state beyond what is required to service active requests. All long-lived state resides in the underlying storage systems.

Restarting ben_store_access does not affect stored data or system intent.

---

## What This Component Does Not Do

ben_store_access does not:
- Store or mutate intent
- Execute or sanitize data
- Interpret policy or schemas
- Cache long-lived artifacts
- Participate in orchestration or coordination
- Act as a system of record

It exists solely to move data safely from storage to consumers.

