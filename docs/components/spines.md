# Spine

## Purpose

Spines are Ben’s hot-path processing components.

Their purpose is to execute declared intent on incoming data by evaluating rules, applying redaction, and producing structured metadata for downstream analysis.

Spines exist to transform data according to explicit contracts; not to reason, infer, or decide.

---

## Authority and Boundaries

Spines are authoritative only for:
- Evaluating Bitspec rules on incoming data
- Constructing masks derived from rule evaluation
- Applying Blot redaction
- Mutating and enriching metadata for detector consumption

Spines are **not** authoritative for:
- Declaring or modifying intent
- Emitting signals
- Routing data
- Performing orchestration
- Interpreting telemetry semantics
- Persisting data

Spines do not infer behavior or make decisions beyond what is explicitly declared.

---

## Rule Evaluation and Redaction

Spines execute:
- **Bitspec** rule evaluation to determine masks and eligibility
- **Blot** redaction to hide data according to declared policy for compliance

All evaluation is:
- Deterministic
- Versioned
- Driven entirely by declared intent
- Applied uniformly regardless of data source or application

Spines do not reinterpret or adapt rules at runtime.

---

## Metadata Mutation Role

Spines enrich data with structured metadata that includes:
- Rule evaluation outcomes
- Masking results
- Redaction markers
- Context required by detectors

This metadata is designed for downstream consumption and analysis. Spines do not act on the metadata they produce.

---

## Data and Application Independence

Spines are agnostic to:
- Application type
- Data origin
- Transport protocol

All data is treated uniformly once translated into Ben’s global schema.

This ensures that spines remain reusable, predictable, and free of application-specific logic.

---

## Configuration and Intent Loading

Spines expose no external configuration surface.

All behavior is driven by:
- Policy packs
- Schemas
- Rules

These artifacts are:
- Declared elsewhere
- Versioned
- Hot-loaded into memory
- Applied without restart where possible

Spines never accept ad-hoc or runtime mutation of behavior.

---

## Relationship With Other Components

- **Proxies** forward regulated data to spines.
- **Spines** may receive data from one or many proxies.
- **Detectors** consume metadata produced by spines.
- **Orchestrator** manages spine lifecycle but does not influence behavior.

Spines do not interact directly with applications, Lucius, or user-facing components.

---

## State Model

Spines are effectively stateless.

They may maintain:
- In-memory representations of policy packs
- Transient evaluation context

All state is:
- Derived from declared intent
- Replaceable
- Discardable

Restarting a spine does not alter system behavior or intent.

---

## What This Component Does Not Do

Spines do not:
- Infer policy or intent
- Emit signals
- Store data
- Coordinate system behavior
- Perform sanitization beyond declared redaction
- Expose configuration knobs

They exist to execute intent exactly as declared.

