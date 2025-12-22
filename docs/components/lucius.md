# Lucius

## Purpose

Lucius is Ben’s dedicated sanitization and detonation subsystem.

Its purpose is to perform expensive, potentially dangerous, or long-running analysis on data that cannot be safely or efficiently handled in Ben’s hot telemetry path.

Lucius exists to isolate risk, not to participate in orchestration or control.

---

## Authority and Boundaries

Lucius is authoritative only for:
- Sanitization and detonation workflows
- Determining sanitization outcomes at each stage
- Producing sanitized analytics and metadata

Lucius is **not** authoritative for:
- Declaring intent
- Orchestrating system components
- Emitting signals to applications
- Enforcing policy outside sanitization
- Runtime coordination

Lucius does not participate in system topology decisions.

---

## Sanitization Model

Lucius operates in two distinct modes: **Light Scrub** and **Deep Scrub**.

### Light Scrub

Light scrub is a first-in, first-out sanitization pipeline.

It supports:
- Static analysis
- YARA rule evaluation
- Hash and reputation checks against security feeds
- Scoring and classification
- Metadata extraction

Light scrub is:
- Interchangeable
- User-extensible
- Open source (AGPL)
- Designed for predictable throughput

Light scrub may process the same artifact multiple times as additional context or rules become available.

---

### Deep Scrub

Deep scrub handles complex and high-risk sanitization.

It involves:
- VM orchestration
- Controlled detonation
- Environment preparation and teardown
- Multi-stage observation and normalization

Deep scrub is implemented as a background daemon and is intentionally isolated from Ben’s runtime systems.

The deep scrub implementation is closed source.

---

## Data Flow and Storage

Lucius maintains its own dedicated ClickHouse instance.

This database is:
- Append-only
- Loss-tolerant
- Used exclusively for Lucius-derived analytics

Lucius does not block or delay Ben’s primary telemetry ingestion.

Sanitization outcomes are:
- Written to Lucius ClickHouse
- Made available via data access layers
- Referenced by other components through declared interfaces

---

## Interaction With the Rest of Ben

Lucius communicates outcomes through:
- NATS notifications indicating sanitization stage completion
- Data availability signals consumable by proxies and downstream systems

Lucius does not:
- Push data directly into runtime components
- Participate in orchestration
- Modify system topology

All interactions are pull-based after notification.

---

## State Model

Lucius maintains only the state required to:
- Track in-flight sanitization cycles
- Manage VM lifecycle during deep scrub
- Ensure correct staging and teardown

Lucius state is local, bounded, and discardable.

---

## Isolation and Safety

Lucius is intentionally isolated from the rest of Ben.

This isolation:
- Limits blast radius
- Prevents expensive work from contaminating hot paths
- Allows different trust and deployment models
- Makes failure modes explicit and containable

Lucius is designed to fail without destabilizing the system.

---

## What This Component Does Not Do

Lucius does not:
- Declare or modify intent
- Emit application-facing signals
- Perform orchestration
- Act as a system of record for Ben
- Interfere with runtime telemetry flow

It exists solely to sanitize, analyze, and report.
