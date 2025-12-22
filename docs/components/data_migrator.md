# data_migrator

## Purpose

data_migrator is the database migration and replication engine for Ben.

Its purpose is to safely and deterministically apply user-declared data-plane changes, manage schema evolution, and replicate data from authoritative backends into front-end-facing replicas without placing database mutation logic on the hot path.

data_migrator exists to execute complex database operations so that no other component needs to.

---

## Authority and Boundaries

data_migrator is authoritative only for:
- Executing database schema changes explicitly declared by user intent
- Managing replication from backend ClickHouse instances to front-end replicas
- Performing controlled data migration and backfill operations
- Monitoring and reporting migration and replication health

data_migrator is **not** authoritative for:
- Declaring intent
- Inferring schema changes
- Modifying policy, playbooks, or signals
- Runtime coordination or signal emission
- Telemetry ingestion or analysis

data_migrator never invents changes. It executes only what has been explicitly declared elsewhere.

---

## Schema and Migration Role

data_migrator is the sole component responsible for implementing database-level changes derived from user intent.

When schemas or data contracts declared by the user require database modification:
- data_migrator performs the required DDL or migration operations
- changes are applied in a controlled, ordered manner
- execution is decoupled from runtime telemetry flow

This isolates database mutation from the rest of the system and prevents accidental or implicit schema drift.

---

## Replication and Data Flow

data_migrator manages data movement between:
- Backend ClickHouse instances used by Ben’s runtime and analysis components
- Front-end-facing ClickHouse replicas used for inspection, visualization, and exploration

Replication is designed to:
- Protect hot ingestion paths from resource-heavy queries
- Allow front-end systems to operate without impacting runtime performance
- Support selective backfill and migration without blocking ongoing ingestion

---

## Hierarchy and Intent Priority

data_migrator operates under a strict hierarchy of intent.

User-declared schema and data changes take precedence over:
- Replication convenience
- Backfill completeness
- Operational optimization

All migration behavior is driven by explicit, versioned declarations. Operational decisions never override declared intent.

---

## Health and Safety Model

data_migrator monitors:
- Migration progress
- Replication lag
- Schema application status
- Failure and retry conditions

Failures are surfaced explicitly. Partial application is treated as a reportable state, not silently corrected.

---

## State Model

data_migrator maintains only the minimal state required to:
- Track in-flight migrations
- Resume interrupted operations
- Report execution status

All state is derived, bounded, and discardable. Restarting data_migrator does not alter declared intent or system behavior.

---

## What This Component Does Not Do

data_migrator does not:
- Declare or modify user intent
- Infer required schema changes
- Perform runtime query serving
- Participate in orchestration or coordination
- Act as a system of record for telemetry or policy

It exists solely to execute declared data-plane changes and replication safely.

