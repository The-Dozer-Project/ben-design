# Detectors

## Purpose

Detectors are bounded autonomous agents responsible for interpreting sanitized telemetry and executing declared playbooks by emitting signals.

Their purpose is to transform user-declared intent into explainable, auditable action without embedding hidden logic, implicit reasoning, or unbounded autonomy into the system.

Detectors are the sole origin point for signals in Ben.

---

## Authority and Boundaries

Detectors are authoritative only for:
- Evaluating telemetry against declared intent
- Executing playbooks through signal emission
- Deciding *when* declared actions should occur, within bounded constraints

Detectors are **not** authoritative for:
- Declaring intent
- Modifying schemas, policies, or playbooks
- Routing signals
- Mutating application state directly
- Performing remediation outside declared playbooks

Detectors do not infer new policy. They execute only what has been explicitly declared.

---

## Autonomy Model

Detectors operate with **bounded autonomy**.

Autonomy is bounded by:
- User-declared playbooks
- Explicit thresholds and conditions
- Versioned intent
- Explainability requirements

Every action taken by a detector is:
- Traceable to declared intent
- Explainable in human terms
- Attributable to specific inputs and conditions

Detectors do not perform opaque or self-modifying behavior.

---

## Signal Emission Model

Detectors are the only components permitted to emit signals.

Signal flow is strictly downstream:
Detector → NATS → Sidecar → Application

Detectors do not communicate directly with applications and do not maintain routing tables. Fan-out and ignore semantics are preferred to targeted delivery.

---

## Data Interaction Model

Detectors consume sanitized telemetry and derived metadata.

They may:
- Ingest live telemetry streams
- Query ClickHouse for historical aggregates and artifacts
- Combine current observation with bounded historical context

Detectors do not operate on raw or unsanitized data.

---

## Versioning and Historical Context

Detectors operate against **active versions of declared intent**.

Historical data from prior versions may be used only as contextual reference. It is never treated as authoritative for current behavior.

Detectors do not retroactively reinterpret past data under new intent. Version boundaries are respected to prevent inferential drift.

---

## Analytical Capabilities

Detectors support both statistical and machine learning–based analysis.

Out-of-box statistical techniques include:
- EWMA (Exponentially Weighted Moving Average)
- CUSUM (Cumulative Sum Control)

More advanced models are trained and managed through **Benson**, with detectors executing trained models rather than learning implicitly at runtime.

---

## Interaction With Other Components

- **Spines** supply sanitized telemetry and metadata.
- **ClickHouse** provides historical aggregates and artifacts for contextual analysis.
- **NATS** is used for signal fan-out.
- **Sidecars** receive signals and translate them into application-facing actions.

Detectors do not coordinate deployment, manage storage, or participate in schema migration.

---

## State Model

Detectors maintain only the minimal state required for:
- In-flight analysis
- Short-term context
- Model execution

All state is discardable and bounded. Restarting a detector does not invalidate system intent.

---

## What This Component Does Not Do

Detectors do not:
- Declare or modify intent
- Perform unsanitized analysis
- Route signals directly to applications
- Persist authoritative state
- Conceal reasoning or decision paths

They exist to decide *when* declared actions occur; not *what* actions exist.

