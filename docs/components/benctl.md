# benctl

## Purpose

benctl is the primary human inflection point for declaring intent in Ben.

Its purpose is to allow operators and developers to inspect, build, stage, and deploy user-authored intent in the form of schemas, policy packs, playbooks, signals, and related configuration artifacts.

benctl is the only component intended for direct human-driven authorship of system intent.

---

## Authority and Boundaries

benctl is authoritative only for:
- Declaring and publishing user intent
- Building and validating contracts prior to deployment
- Staging and activating versioned intent into the system

benctl is **not** authoritative for:
- Runtime behavior
- Telemetry ingestion or analysis
- Coordination or orchestration
- Signal emission
- System recovery

benctl does not observe or infer system state. It only declares intent explicitly provided by the user.

---

## Interaction Model

benctl operates as a stateless command-line interface.

It interacts exclusively with **ben_ledger** to:
- Write versioned intent
- Inspect declared and staged configurations
- Activate or unstage changes

benctl does not communicate directly with runtime components such as spines, proxies, sidecars, or detectors.

All runtime effects of benctl actions occur indirectly through reconciliation and consumption of ledger-declared intent.

---

## Build, Inspect, and Deployment Role

Within a Ben project, benctl is used to:
- Inspect compiled schemas, policies, playbooks, and signals
- Build intent artifacts from code-as-configuration
- Stage changes prior to activation
- Deploy or unstage declared intent

Deployment is declarative. benctl does not push behavior or issue commands to the runtime system.

---

## Scriptability and Automation

benctl is designed to be scriptable and automation-friendly.

It supports:
- Non-interactive operation
- Integration into CI/CD pipelines
- Deterministic output suitable for tooling and verification

benctl does not maintain local state between invocations.

---

## Design Constraints

benctl is intentionally simple.

Complexity is resisted by default and must be explicitly justified. New behavior is introduced only when it provides clear benefit to correctness, safety, or legibility of intent declaration.

This constraint is treated as a versioned design decision. Future complexity is permitted only when it can be defended against these principles.

---

## What This Component Does Not Do

benctl does not:
- Execute or enforce runtime behavior
- Act as a control plane
- Perform orchestration or coordination
- Modify system state outside of declared intent
- Provide a long-running service

It exists solely to translate human intent into explicit, versioned declarations.

